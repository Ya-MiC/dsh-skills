---
name: skill-authoring
description: Use when asked to create, edit, review, install, migrate, or restructure any Agent Skill (SKILL.md) for DeepSeek Harness, Claude Code, Codex, or any SKILL.md-compatible agent. Encodes Ya-MiC''s house conventions for naming, frontmatter, directory layout, storage location, and GitHub-first authoring.
---

# 寫 Skill 的家法

## 1. 格式硬規則
- 目錄：`<根>/<kebab-case名>/SKILL.md`，只有這一層深度；也接受扁平 `<根>/<名>.md`。
- frontmatter 必填兩項：
  - `name`：小寫連字號（kebab-case），必須等於資料夾名；
  - `description`：第三人稱寫「什麼時候用它」，這句話是唯一的觸發器，要含動詞與場景關鍵詞。
- 選填：`whenToUse`、`metadata`、`disable-model-invocation`、`user-invocable`（布林）。

## 2. 正文骨架
```markdown
# 名稱
一句話總則。
## Workflow      ← 編號步驟，每步一個動作
## Guardrails    ← 禁止事項（fail-closed）
## Resources     ← references/ scripts/ assets/ 子目錄說明
```

## 3. 存放與接線
- 真相源：本倉庫（GitHub 上編輯）。
- 本機聚合：`E:\skills\<名>\`；各工具目錄用**整目錄聯接**指向 E:\skills，禁止逐個資料夾對應。
- 同步：改完 GitHub → 本機 `git pull` 即生效（所有工具熱載入）。

## 4. 品質檢查清單
- [ ] name 合法且與資料夾同名？
- [ ] description 含觸發場景，別人讀得懂何時用？
- [ ] 有 Guardrails 嗎？
- [ ] 不含機密（token／密碼）？