# English Coach

一个支持 **Codex、Cursor 和 Claude Code** 的 Agent Skill，让每次对话都变成英语练习课。

**其他语言版本：**
[English](./README.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Português](./README.pt-BR.md) | [Tiếng Việt](./README.vi.md) | [ภาษาไทย](./README.th.md) | [Русский](./README.ru.md) | [العربية](./README.ar.md)

## 它能做什么

激活后，AI 编程助手会在每次回复中做这些事：

1. **正常回答你的问题** —— 先把正事办了
2. **纠正你的英语** —— 语法、拼写、用词、标点、表达方式（中英双语讲解）
3. **教你点新东西** —— 实用短语、语法技巧、更地道的说法
4. **（可选）同步到欧路词典** —— 生词本 `english-coach`：写入词条，并把**原文 / 修正句 / 完整纠正解析 / Level up**以长句分析笔记保存

专为**非英语母语者**设计。不用背单词、不用刷题，在日常使用中自然提升英语水平。

## 特性

- **分类纠错**：拼写 / 语法 / 用词 / 标点 / 表达，一目了然
- **中英双语讲解**：适合英语基础一般的学习者
- **自适应难度**：初学者侧重基础纠错，进阶者侧重地道表达
- **重复错误追踪**：同一个错误犯两次，会被重点标记
- **轮换教学内容**：实用短语、语法规则、高级改写、中国人常见错误，每次不同
- **欧路词典同步**：配合 Go 版 MCP（[go-sdk](https://github.com/modelcontextprotocol/go-sdk)），自动写入生词本 `english-coach`；支持本机 Windows 与 **Cursor Remote SSH（Linux）**（见 [docs/eudic-mcp-setup.md](./docs/eudic-mcp-setup.md)）
- **友好语气**：像朋友聊天一样纠正，不是老师批作业

## 安装

```bash
npx skills add tianmind-studio/english-coach -a codex cursor claude-code -g -y
```

如果只安装到一个客户端，可单独使用 `-a codex`、`-a cursor` 或 `-a claude-code`。

## 使用方法

在 Codex、Cursor 或 Claude Code 中开启新对话，请助手使用 `english-coach`；如果客户端提供 Skill 选择器，也可以直接按名称选择。然后正常用英语聊天，Skill 会自动纠正并教学。

## 示例

**你说：**
> dose this work for practice my english?

**AI 助手回复：**

*（正常回答你的问题）*

---

**English Corrections / 英语修正：**

> ~~dose this work for practice my english?~~ → **Does this work for practicing my English?**
>
> **[Spelling / 拼写]** "dose" → "does" — wrong word / 拼写错误
> **[Grammar / 语法]** "for practice" → "for practicing" — after "for", use the -ing form — 介词 for 后面要用动名词（-ing）
> **[Punctuation / 标点]** "english" → "English" — language names are always capitalized — 语言名称首字母要大写

**Phrase of the day / 今日短语：** "get the hang of" — to become comfortable with something new — 逐渐掌握、上手某件事。
*Example / 例句：I'm starting to get the hang of English prepositions.（我开始掌握英语介词的用法了。）*

## 许可证

MIT
