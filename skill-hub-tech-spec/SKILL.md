---
name: skill-hub-tech-spec
description: Use when asked what format, encoding, tooling, or code underlies the central skill hub (E:\skills and Ya-MiC/dsh-skills), when reproducing the skill-organizing pipeline on another Windows machine, when debugging mojibake or HTTP 400/403 in GitHub API calls from PowerShell, or when auditing how junctions scanning verification and sync were implemented.
---

# 技能中樞技術規格（管線完整披露）

## 1. 技能檔案格式是什麼
- 檔案固定名為 SKILL.md；格式＝YAML frontmatter（--- 包夾，必填 name、description）＋ Markdown 正文。
- 這是跨工具的「Agent Skills」開放慣例：DeepSeek Harness、Claude Code、Codex、QClaw、WorkBuddy 都認。
- 目錄契約：`<根>/<kebab-case名>/SKILL.md`，只掃一層深；扁平 `<根>/<名>.md` 也合法。

## 2. 編碼格式是什麼
- 一律 **UTF-8，無 BOM**。BOM 會破壞部分工具的 YAML 解析。
- PowerShell 寫檔範式：`[IO.File]::WriteAllText($path,$text,[Text.UTF8Encoding]::new($false))`

## 3. 掃描電腦用的是什麼程序
- 直譯器：**Windows PowerShell 5.1**（命令列 pwsh），非獨立軟體。
- 掃描指令族：
  - 列目錄：`Get-ChildItem -Force -Directory`
  - 存在探測：`Test-Path`
  - 一致性核對：`Get-FileHash -Algorithm SHA256`
  - 辨認聯接：`Get-Item -Force` 後讀 `.LinkType`／`.Target`

## 4. 「超鏈接」的實際代碼語言是什麼
- 不是任何程式語言——是 **NTFS 檔案系統的目錄聯接（Junction，重解析點）**，作業系統層功能。
- 建立（二選一）：
  - cmd：`mklink /J "<聯接路徑>" "<實際路徑>"`（不需管理員）
  - PowerShell：`New-Item -ItemType Junction -Path <聯接> -Target <實際>`
- 刪除：`rmdir "<聯接路徑>"`（只拆鏈不傷目標）。**禁** `Remove-Item -Recurse`。
- 特性：對所有程式透明；可跨磁碟機（限本機卷）；與快捷方式 .lnk 完全不同。

## 5. GitHub 遠端操作協議
- REST v3：檔案寫入 `PUT /repos/{owner}/{repo}/contents/{path}`，content 欄＝UTF-8 位元組的 Base64；Issue 建立 POST／更新 PATCH。
- GraphQL v4：僅 Projects 看板用。端點 api.github.com/graphql；注意 CreateProjectV2Payload 的返回欄位叫 **projectV2**。
- 認證：標頭 `Authorization: Bearer <細粒度PAT>`。

## 6. 編碼鐵律（亂碼唯一根源，fail-closed）
- PowerShell 5.1 直接把含中文的字串當 `-Body` 發送 → 按 Latin-1 重編碼 → 中文變 `?`、U+00B7 等字符產生非法 UTF-8 序列 → GitHub 回 `400 Problems parsing JSON`。
- **唯一正解**：`$bytes=[Text.Encoding]::UTF8.GetBytes($json)` 再 `-Body $bytes`。
- PS 5.1 的 ConvertFrom-Json 對部分回應會崩；改用 `Invoke-WebRequest -UseBasicParsing` 取 .Content 後自行解析。
- PS 5.1 不支援 `??` 運算子、GraphQL schema 以內省查詢為準，不靠記憶。

## 7. 全流程重放清單
掃描盤點（§3）→ 遷移既有技能（Copy-Item）→ 改名備份（Rename-Item skills.backup-日期；被佔用先關閉該工具）→ 建聯接（§4）→ 穿透驗證（Test-Path＋計數前後相等）→ 上傳真相源（§5 base64）→ 日程掛帳（Issues/Projects）→ 同步循環（git pull 即生效）。