# English Coach

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that turns every conversation into an English practice session.

[![Release](https://img.shields.io/github/v/release/491034170/english-coach)](https://github.com/491034170/english-coach/releases/latest)
[![Live Guide](https://img.shields.io/badge/Live_Guide-GitHub_Pages-4FACFE)](https://491034170.github.io/english-coach/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Built for Claude Code](https://img.shields.io/badge/built%20for-Claude%20Code-8A2BE2)](https://docs.anthropic.com/en/docs/claude-code)

Quick links:
- Live guide: <https://491034170.github.io/english-coach/>
- Latest release: <https://github.com/491034170/english-coach/releases/latest>
- Skill source: [`english-coach/SKILL.md`](./english-coach/SKILL.md)
- Chinese README: [`README.zh-CN.md`](./README.zh-CN.md)
- Practical scenarios: [`examples/chat-scenarios.md`](./examples/chat-scenarios.md)
- FAQ: [`FAQ.md`](./FAQ.md)

**README in other languages:**
[中文](./README.zh-CN.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Português](./README.pt-BR.md) | [Tiếng Việt](./README.vi.md) | [ภาษาไทย](./README.th.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md)

## Why people star this repo

- It turns everyday Claude usage into English practice without changing your workflow
- It answers first, then teaches — so it stays useful even when you are not in "study mode"
- It works well for non-native speakers who want practical, daily improvement instead of textbook drills

## What it does

When activated, Claude will:

1. **Answer your question normally** — it does its actual job first
2. **Correct your English** — grammar, spelling, word choice, punctuation, expression
3. **Teach something new** — a useful phrase, grammar tip, or more natural way to say things

Designed for **non-native English speakers** who want to improve their English through real daily use rather than textbook exercises.

## Features

- Categorized corrections (Spelling / Grammar / Word Choice / Punctuation / Expression)
- Adapts to your level — focuses on basics for beginners, naturalness for advanced users
- Tracks recurring mistakes and highlights patterns
- Rotates learning content: idioms, grammar tips, advanced rephrasing, common mistakes
- Friendly tone — like a helpful coworker, not a strict teacher

## Install

Option 1 — install directly from GitHub:

```bash
npx skills add 491034170/english-coach -a claude-code -g -y
```

Option 2 — install from a packaged release:
1. Open <https://github.com/491034170/english-coach/releases/latest>
2. Download `english-coach.skill`
3. Import it into Claude Code / Claude Desktop

## Usage

In Claude Code, type:

```
/english-coach
```

Then just chat normally in English. The skill will automatically correct and teach as you go.

## Example 1

**You say:**
> dose this work for practice my english?

**Claude responds:**

*(answers your question)*

---

**English Corrections / 英语修正：**

> ~~dose this work for practice my english?~~ → **Does this work for practicing my English?**
>
> **[Spelling / 拼写]** "dose" → "does" — wrong word / 拼写错误
> **[Grammar / 语法]** "for practice" → "for practicing" — after "for", use the -ing form — 介词 for 后面要用动名词（-ing）
> **[Punctuation / 标点]** "english" → "English" — language names are always capitalized — 语言名称首字母要大写

## Example 2

**You say:**
> His has a great time yesterday.

**Claude responds:**

*(answers your question)*

---

**English Corrections / 英语修正：**

> ~~His has a great time yesterday.~~ → **He had a great time yesterday.**
>
> **[Grammar / 语法]** "His" → "He" — "His" is possessive, "He" is a subject pronoun — His 是所有格，主语应用 He
> **[Grammar / 语法]** "has" → "had" — "yesterday" indicates past time — yesterday 表示过去，动词要用过去式

## Example 3

**You say:**
> I look forward to meet you and making a discussion about the project.

**Claude responds:**

*(answers your question)*

---

**English Corrections / 英语修正：**

> ~~I look forward to meet you and making a discussion about the project.~~ → **I look forward to meeting you and having a discussion about the project.**
>
> **[Grammar / 语法]** "look forward to meet you" → "look forward to meeting you" — After "look forward to," use the -ing form (gerund). The "to" here is a preposition, not part of an infinitive. — look forward to 后面接动名词；这里的 to 是介词，不是不定式标记
>
> **[Word Choice / 用词]** "making a discussion" → "having a discussion" (or simply discussing) — In English, we "have" or "hold" a discussion — we don't "make" one. — 英语里讨论用 have/hold a discussion，不用 make

**Phrase of the day / 今日短语：** "get the hang of" — to become comfortable with something new — 逐渐掌握、上手某件事。
*Example / 例句: "I'm starting to get the hang of English prepositions."（我开始掌握英语介词的用法了。）*

## Optional: Sync to Eudic (欧路词典)

With a Go Eudic MCP server (`eudic-mcp-go`, [go-sdk](https://github.com/modelcontextprotocol/go-sdk)) configured in Cursor, English Coach can write vocabulary and bilingual correction notes into a study list named **`english-coach`**. Works on local Windows and **Cursor Remote SSH (Linux)** — use a Linux binary on the remote host.

Setup: [docs/eudic-mcp-setup.md](./docs/eudic-mcp-setup.md) · FAQ: “How do I sync corrections to Eudic”

## License

MIT

---

If this skill helps you practice English more consistently, consider starring the repo so more learners can find it.
