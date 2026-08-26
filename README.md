# dsh-skills · 中央技能與規範庫

Ya-MiC 全部 AI 工具（DeepSeek Harness / Claude Code / Codex / QClaw / WorkBuddy…）的**唯一技能真相源**。

> 鐵則：本倉庫一律在 GitHub 上編輯；本機只保留 git 同步克隆與目錄聯接。

## 結構

| 路徑 | 用途 |
|---|---|
| `ROOT.md` | 全域常駐規則（人類可讀版） |
| `rule-root/SKILL.md` | 同一套規則的機器載入版（skill 形式） |
| `skill-authoring/SKILL.md` | 「如何寫一個 skill」的家法規範（skill 形式） |

## 接線圖（現狀）

```
E:\skills   ←── ~/.dsh/skills（DSH）
            ←── ~/.claude/skills（Claude Code）
            ←── ~/.codex/skills（Codex）
```

待接入：QClaw、WorkBuddy、cc-switch（見 Issues）。