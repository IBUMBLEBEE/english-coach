# Eudic Sync (via eudic-mcp-go)

Sync coaching output to the Eudic / 欧路词典 study list named **`english-coach`**.

Requires an Eudic MCP server exposing `eudic_*` tools and `EUDIC_API_TOKEN` (token only — no `NIS ` prefix).

**Recommended server:** Go binary `eudic-mcp-go` ([go-sdk](https://github.com/modelcontextprotocol/go-sdk)).  
On **Cursor Remote SSH / Linux**, MCP runs on the remote host — use a **Linux** binary (`~/bin/eudic-mcp-go`), not a Windows `.exe`. Setup: [docs/eudic-mcp-setup.md](../docs/eudic-mcp-setup.md).

## Resolve category ID (once per session)

1. Call `eudic_list_categories` with `language: "en"`.
2. If a category named exactly `english-coach` exists, reuse its `id`.
3. Otherwise call `eudic_create_category` with `{ "name": "english-coach", "language": "en" }` and use the returned `id`.
4. Remember this `category_id` for the rest of the session.

## What to sync

| Source | Action | Skip when |
|--------|--------|-----------|
| Spelling / Word Choice / Expression corrections | `eudic_add_word` (corrected lemma or phrase) + `eudic_add_note` | Pure Punctuation (e.g. `i` → `I`) with no useful vocabulary |
| Grammar with a clear target word/phrase | Same as above | Abstract structure tips with no headword |
| Phrase of the day | `eudic_add_word` + note with EN+ZH meaning | — |
| Level up sentence | Put the polished sentence in `context_line` on the main word(s) | — |
| Translation-mode key vocabulary | Add English headwords + short bilingual note | — |

## Tool payloads

**Add word (prefer when you have a context sentence):**

```json
{
  "word": "refined",
  "category_ids": ["<english-coach-category-id>"],
  "star": 3,
  "context_line": "I'm aiming for a compact, refined product.",
  "language": "en"
}
```

**Bulk add (multiple headwords, no per-word context):**

```json
{
  "words": ["refined", "anomaly"],
  "category_id": "<english-coach-category-id>",
  "language": "en"
}
```

**Note (bilingual correction / tip):**

```json
{
  "word": "refined",
  "note": "[Spelling / 拼写] redeined → refined. Missing f; refined = polished. / 漏写 f；refined = 精炼的、打磨过的。",
  "language": "en"
}
```

## Limits

- Max **5** headwords per response (align with max corrections).
- If MCP / token is unavailable: skip sync, continue coaching, and say briefly that Eudic sync was skipped.
- Never print the API token.
