# Eudic Sync (via eudic-mcp-go)

Sync coaching output to the Eudic / 欧路词典 study list named **`english-coach`**.

Requires an Eudic MCP server exposing `eudic_*` tools and `EUDIC_API_TOKEN` (token only — no `NIS ` prefix).

**Recommended server:** Go binary `eudic-mcp-go` ([go-sdk](https://github.com/modelcontextprotocol/go-sdk)).  
On **Cursor Remote SSH / Linux**, MCP runs on the remote host — use a **Linux** binary. Setup: [docs/eudic-mcp-setup.md](../docs/eudic-mcp-setup.md).

## Resolve category ID (once per session)

1. Call `eudic_list_categories` with `language: "en"`.
2. If a category named exactly `english-coach` exists, reuse its `id`.
3. Otherwise call `eudic_create_category` with `{ "name": "english-coach", "language": "en" }` and use the returned `id`.
4. Remember this `category_id` for the rest of the session.

---

## MUST record full sentences (Correction Mode)

Whenever Part 2B ran on English input, build these strings first:

| Field | Source |
|-------|--------|
| `original_full` | The user's **entire** English message (trim only) |
| `corrected_full` | The **fully corrected** version of that message (all fixes applied) |
| `level_up_full` | Part 3 **Level up** sentence if present; else omit |

Both `original_full` and `corrected_full` **must** appear in every synced word's `note` (长句分析格式). Do not sync only isolated tokens without the sentence pair.

If there were **no vocabulary headwords** (e.g. only punctuation) but the sentence still changed, still sync: pick **1–2 content words** from `corrected_full` as anchors so the sentence pair is stored.

---

## What to sync

| Source | Action |
|--------|--------|
| Correction Mode (any real fix, including punctuation-only sentence cleanup) | Anchor word(s) + `context_line` = `corrected_full` (prefer `level_up_full` if present) + **长句分析 `note`** |
| Spelling / Grammar / Word Choice / Expression | Include those lemmas/phrases as headwords (max 5) |
| Pure punctuation token alone (`i`→`I`) | Do **not** use as sole headword; still keep sentence pair via a content-word anchor |
| Phrase of the day | `eudic_add_word` + short bilingual note (may omit full sentence block if unrelated) |
| Translation Mode key vocabulary | Headword + short note; if a full English translation sentence exists, put it in `context_line` and include 原文意图/译文 in note |

---

## 长句分析 note 格式 (REQUIRED for Correction Mode)

Use this template for `eudic_add_note` on each synced headword (same sentence block; **Focus** section highlights that headword):

```text
【原文 Original】
<original_full>

【修正 Corrected】
<corrected_full>

【进阶 Level up】
<level_up_full or (无 / none)>
（中文译文，若有）

【本词要点 Focus: <headword>】
[Category / 中文标签] English tip. / 中文说明。

【其它修正 Other fixes】
- a → b — short tip / 中文
- …
```

Rules for the note:

- Always include **原文** and **修正** blocks, even when they are identical (then write `（无修改 / unchanged）` under 修正 only if truly unchanged — normally Correction Mode implies a difference).
- If Level up exists, always include **进阶**; otherwise write `（本回合无 Level up / none this turn）`.
- Keep Chinese glosses in tips.
- If `eudic_get_note` returns an existing note, **replace** with this latest analysis (do not silently drop the new sentence pair).

---

## Tool payloads

**1) Add word with corrected / Level-up sentence as context:**

```json
{
  "word": "refined",
  "category_ids": ["<english-coach-category-id>"],
  "star": 3,
  "context_line": "I'm aiming for a compact, refined product. Is there room to simplify?",
  "language": "en"
}
```

`context_line` priority: `level_up_full` > `corrected_full`.

**2) Note — 长句分析 (example):**

```json
{
  "word": "refined",
  "note": "【原文 Original】\ni want a compact yet redeined product, is there room to simplify?\n\n【修正 Corrected】\nI want a compact yet refined product. Is there room to simplify?\n\n【进阶 Level up】\nThe anomaly detection stack feels heavy — I'm aiming for a compact, refined product. Is there room to simplify?\n（异常检测这套方案偏重 —— 我想做更精简、打磨到位的产品。还有没有简化空间？）\n\n【本词要点 Focus: refined】\n[Spelling / 拼写] redeined → refined. Missing f; refined = polished. / 漏写 f；refined = 精炼的、打磨过的。\n\n【其它修正 Other fixes】\n- i want → I want — I is always capitalized. / 主语 I 永远大写。\n- … product, is there → … product. Is there — split into two sentences. / 拆成两句，问句更清楚。",
  "language": "en"
}
```

**3) Bulk add (optional, when many headwords share the same sentence):**  
Still call `eudic_add_word` + `eudic_add_note` per headword so each has `context_line` + 长句分析 note. Prefer per-word calls over bulk-only.

---

## Limits

- Max **5** headwords per response.
- Always sync the **sentence pair** (原文 + 修正) when Correction Mode produced output; Level up goes into the same note when present.
- If MCP / token is unavailable: skip sync, continue coaching, brief notice.
- Never print the API token.
