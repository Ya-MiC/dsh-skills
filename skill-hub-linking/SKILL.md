---
name: skill-hub-linking
description: Use when onboarding any AI coding agent tool (DeepSeek Harness, Claude Code, Codex, QClaw, WorkBuddy, cc-switch or future tools) to the central skill hub, when creating or repairing directory junctions for skills, when migrating an agent''s existing skills into E:\skills, or when diagnosing why an agent cannot see shared skills.
---

# 技能中樞接線規範

## 拓撲
- GitHub 真相源：Ya-MiC/dsh-skills（只在 GitHub 上編輯規範與新技能）
- 本機聚合點：E:\skills（所有技能的唯一實體資料夾）
- 各工具側一律「整目錄聯接」指向 E:\skills；禁止逐個資料夾對應。

## 已接入
| 工具 | 工具側路徑 |
|---|---|
| DSH | C:\Users\cao41\.dsh\skills |
| Claude Code | C:\Users\cao41\.claude\skills |
| Codex | C:\Users\cao41\.codex\skills |
| QClaw | C:\Users\cao41\.qclaw-oversea\skills |
| WorkBuddy | C:\Users\cao41\.workbuddy\skills |
| cc-switch | C:\Users\cao41\.cc-switch\skills |

## 新工具接入流程
1. 工具有既有技能 → 先複製進 E:\skills\，確認無同名衝突。
2. 把工具的 skills 目錄改名 skills.backup-日期（若被運行中的工具佔用，先關閉該工具再改名）。
3. 建聯接：cmd /c mklink /J "<工具的skills路徑>" "E:\skills"
4. 驗證：穿透聯接能看到 E:\skills 全部技能，且工具熱載入生效。

## Guardrails
- 工具目錄裡永不放真實技能檔案——一律放 E:\skills。
- 拆聯接只用 rmdir，禁用遞歸刪除（會傷及 E:\skills 本體）。
- 改技能內容只在 GitHub（真相源）或 E:\skills 進行；兩處不一致時以 GitHub 為準。