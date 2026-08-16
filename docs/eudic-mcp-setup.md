# Cursor MCP — Eudic (Go) / Linux & Remote SSH

english-coach 通过 MCP 同步欧路生词本。推荐使用 Go 版 [`eudic-mcp-go`](https://github.com/modelcontextprotocol/go-sdk)（源码默认：`/path/to/eudic-mcp-go` 或本机 `C:\data\code\go\src\eudic-mcp-go`）。

**Cursor 远程开发（SSH / Dev Container）时，MCP 跑在远程 Linux 上**，必须使用 **Linux 二进制**，不能用 `.exe`。

---

## A. 远程 Linux（推荐：Cursor Remote SSH）

### 1. 在远程机器上准备二进制

任选其一：

**方式 1 — 远程本机编译（需已装 Go）**

```bash
cd /path/to/eudic-mcp-go   # 把源码放到远程，或 git clone
go build -ldflags="-s -w" -o eudic-mcp-go .
mkdir -p ~/bin
cp eudic-mcp-go ~/bin/
chmod +x ~/bin/eudic-mcp-go
```

**方式 2 — 本机交叉编译后 scp 上去**

在有 Go 的机器上：

```powershell
# Windows
cd C:\data\code\go\src\eudic-mcp-go
.\scripts\build.ps1
# 产物: dist\linux-amd64\eudic-mcp-go  （或 linux-arm64）
```

```bash
# macOS / Linux
cd /path/to/eudic-mcp-go
bash scripts/build.sh
```

上传到远程：

```bash
scp dist/linux-amd64/eudic-mcp-go user@remote:~/bin/eudic-mcp-go
ssh user@remote 'chmod +x ~/bin/eudic-mcp-go'
```

架构：`uname -m` → `x86_64` 用 `linux-amd64`，`aarch64`/`arm64` 用 `linux-arm64`。

### 2. 在远程配置 MCP

在 **远程 Linux** 上编辑（不是 Windows 本机）：

`~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "eudic": {
      "command": "/home/你的用户名/bin/eudic-mcp-go",
      "env": {
        "EUDIC_API_TOKEN": "你的token"
      }
    }
  }
}
```

说明：

- `command` 用**绝对路径**最稳（部分环境 `${env:HOME}` 可能不展开；不确定时请写死 `/home/.../bin/eudic-mcp-go`）
- Token 只填 `NIS` **后面**的部分，见 [Authorization](https://my.eudic.net/OpenAPI/Authorization)
- **不要**把 Token 写进仓库里的 `mcp.json.example`

仓库模板：

- 远程 Linux：[`.cursor/mcp.json.example`](../.cursor/mcp.json.example)
- 本机 Windows：[`.cursor/mcp.windows.json.example`](../.cursor/mcp.windows.json.example)

### 3. 重载窗口

Cursor：`Developer: Reload Window`（或断开再连 Remote），确认 MCP 工具里有 `eudic_list_categories` 等。

### 4. 自检

在远程终端：

```bash
EUDIC_API_TOKEN='你的token' ~/bin/eudic-mcp-go
# 应阻塞等待 stdio（MCP 正常）；Ctrl+C 退出
# 若立刻报 missing EUDIC_API_TOKEN，说明环境变量未传入
```

---

## B. 本机 Windows（非远程）

```powershell
cd C:\data\code\go\src\eudic-mcp-go
go build -o eudic-mcp-go.exe .
```

编辑 `%USERPROFILE%\.cursor\mcp.json`，`command` 指向 `.exe`（见 `mcp.windows.json.example`），然后重启 Cursor。

---

## C. 与 english-coach

Skill 同步到生词本名 **`english-coach`**。流程见 [`english-coach/eudic-sync.md`](../english-coach/eudic-sync.md)。

远程开发时只要远程 MCP 正常，Agent 调用方式与本机相同。
