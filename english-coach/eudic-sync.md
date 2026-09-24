# Background Eudic Sync

Sync coaching output to the Eudic / 欧路词典 study list named **`english-coach`** without adding sync narration to the learner-facing response.

## Preferred path: one composite call

Call `eudic_sync_coaching` once per coaching response. Send:

- `category`: `english-coach`
- `language`: `en`
- `entries`: at most five `{ word, context_line, note, star }` items

Use `star: 3`. Set `context_line` to `level_up_full` when present, otherwise to the corrected sentence or English translation. The server handles category lookup/creation and all writes.

The sync is complete when the tool reports the number of entries written. Consume that result internally; successful sync adds no line to the response.

## Correction Mode

Whenever Part 2B ran on English input, build these strings first:

| Field | Source |
|-------|--------|
| `original_full` | The user's **entire** English message (trim only) |
| `corrected_full` | The **fully corrected** version of that message (all fixes applied) |
| `level_up_full` | Part 3 **Level up** sentence if present; else omit |

Both `original_full` and `corrected_full` must appear in every synced word's `note`. Do not store only isolated tokens.

If there were **no vocabulary headwords** (e.g. only punctuation) but the sentence still changed, still sync: pick **1–2 content words** from `corrected_full` as anchors so the sentence pair is stored.

---

### Headwords

- Choose lexical fixes as lemmas or phrases, maximum five.
- For punctuation-only changes, choose one or two content words from `corrected_full` as anchors.
- Do not use a punctuation token or `i` → `I` as the sole headword.

### Note template

Use this template as each entry's `note` (same sentence block; **Focus** highlights that headword):

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

For each headword, keep the same sentence blocks and tailor `Focus` to that word. Keep every tip bilingual. If no Level up exists, use `（本回合无 Level up / none this turn）`.

## Translation Mode and standalone learning items

- For key vocabulary, use the full English translation as `context_line`.
- The note may be shorter, but include the Chinese intent, English translation, and a bilingual word or phrase tip.
- Phrase of the day may use a short bilingual note unrelated to the sentence-pair template.

## Compatibility fallback

If `eudic_sync_coaching` is unavailable but the individual tools exist:

1. Resolve `english-coach` with `eudic_list_categories`; create it only when absent.
2. For each headword, call `eudic_add_word`, then `eudic_add_note` with the same data prepared for the composite call.
3. Do not narrate lookup, writes, success, or skipped sync.

On failure, preserve the coaching response and stop retrying for that turn. Surface one short actionable message only when the learner asked about sync or must repair configuration. Never reveal `EUDIC_API_TOKEN`.
