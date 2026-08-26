---
name: machine-profile
description: Use before any task that touches system configuration, software installation, PATH or environment variables, service wiring, large disk writes, or cross-tool setup on Ya-MiC''s Windows machine. Provides the authoritative hardware OS runtime PATH editor and AI-agent inventory snapshot plus the rescan procedure, so agents never guess the machine baseline.
---

# YANMING 機器全景檔案（快照日 2026-08-26）

> 使用原則：任何影響系統的操作前，先核對本檔；懷疑過期就跑「刷新程序」重掃並回寫。

## 硬體與系統
- 主機 YANMING｜Windows 11 Pro｜OS 10.0.26200｜64-bit AMD64
- CPU：Intel i7-10870H（8核16執行緒）｜RAM：15.8 GB
- GPU：NVIDIA RTX 2060 Max-Q＋Intel UHD（雙顯）
- 磁碟：C: 系統碟（剩 50/199GB）｜D: 「Yamic」（剩 13/68GB ⚠偏緊）｜E: 工作盤（剩 23/208GB）

## 執行環境版本
| 工具 | 版本／位置 |
|---|---|
| Node.js | v24.19.0（E:\nodejs） |
| pnpm | 11.7.0（另有全域 npm 包 pnpm@11.21.0） |
| npm | 11.17.0 |
| Git | 2.54.0.windows.1（D:\VScode\Git） |
| Python | 3.10（D:\Python）；AppData 另有 3.10/3.12 兩套 ⚠多版並存 |
| PowerShell | **5.1**（未裝 pwsh7）⚠中文請求體必須位元組級 UTF-8 |
| 其他 | Docker Desktop、dotnet、Ollama、WinSCP |

代碼頁 chcp 65001｜時區 Taipei Standard Time

## PATH 結構要點
- 系統段：E:\nodejs、D:\Python、D:\VScode\Git 三件套、Docker、dotnet、Cursor CLI(E:\cursor)
- 使用者段：npm 全域(AppData\Roaming\npm)、cargo、Python×3、編輯器 CLI(VS Code/Cursor/Windsurf/PyCharm/Antigravity)、Ollama、CodeBuddy CN(workBuddy)、windclaw cli、Obsidian、WinGet Links
- 完整值以登錄檔為準：HKLM\...\Session Manager\Environment 與 HKCU\Environment

## AI 工具與技能接線
- 全域 npm：@anthropic-ai/claude-code@2.1.246、claude-code-router@2.0.0、openclaw@2026.5.7、clawhub@0.8.0、qmd@2.0.1、tavily-mcp@0.2.17
- 技能中樞：E:\skills ← 六工具整目錄聯接（DSH/.dsh、Claude/.claude、Codex/.codex、QClaw/.qclaw-oversea、WorkBuddy/.workbuddy、cc-switch/.cc-switch）
- DSH 本體：E:\20260816-\deepseek-harness（pnpm dsh web --port 3080）
- GitHub：帳號 Ya-MiC｜中央庫 dsh-skills（規範與日程）

## 刷新程序（重掃描）
1. 硬體/系統：Get-CimInstance Win32_OperatingSystem／Processor／VideoController／ComputerSystem
2. 版本：各工具 --version；PATH：讀兩處登錄檔 Environment；npm 清單：npm ls -g --depth=0
3. 回寫本檔快照區＋更新快照日期；GitHub 為真相源，本機隨 git pull 同步

## Guardrails
- 改 PATH／環境變數前先輸出現值 diff；永久變數僅經 [Environment]::SetEnvironmentVariable(...,'User'/'Machine')
- 大型寫入避開 D:（剩量偏緊），優先 E:
- pip/python 操作先確認目標直譯器（多版並存）