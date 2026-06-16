---
name: experience-library-sync
description: Read, classify, write, and Git-sync a shared Markdown AI experience library. Use when the user says "读经验库", "沉淀一下", "沉淀经验", "存到经验库", "更新项目状态", "新建项目", or asks to share/package the experience-library workflow across devices or collaborators.
---

# Experience Library Sync

## Core Contract

Use one fixed Git repository as the source of truth for reusable AI experience notes. Do not save experience cards into the current project repository unless the user explicitly overrides this.

Default repository for the original owner:

```text
C:\Users\Administrator\WorkBuddy\ai-knowledge-base
```

Default library directory:

```text
C:\Users\Administrator\WorkBuddy\ai-knowledge-base\AI经验库
```

Default remote:

```text
https://github.com/wang-zefeng/-ai-knowledge-base.git
```

For another person or device, replace `C:\Users\Administrator` with that device's user home path, for example:

```text
C:\Users\<用户名>\WorkBuddy\ai-knowledge-base
```

## Hard Rules

- Never treat `C:\Users\<用户名>\WorkBuddy` as the Git repo root; the repo root is `...\WorkBuddy\ai-knowledge-base`.
- Never create `docs/AI经验库` inside the current project as the default destination.
- Never create a new GitHub repository unless the user explicitly asks for a separate fork or copy.
- Never commit API keys, tokens, cookies, account passwords, raw private data, large assets, output videos, logs, or `secrets.local.json`.
- Do not dump raw chat transcripts. Save conclusions, steps, prompts, pitfalls, project status, and reusable workflows.
- If the experience-library repo has unrelated dirty changes, stage only the files touched for the current save and report the remaining dirty files.

## Read Workflow

Use this when the user says "读经验库" or asks for project context.

1. Locate the repo root.
   - Prefer a user-provided path.
   - Otherwise use `C:\Users\Administrator\WorkBuddy\ai-knowledge-base` on the original owner's machine.
   - On a collaborator's machine, use `%USERPROFILE%\WorkBuddy\ai-knowledge-base`.
2. If the repo is missing and a remote is known, clone it:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\WorkBuddy"
git clone https://github.com/wang-zefeng/-ai-knowledge-base.git "$env:USERPROFILE\WorkBuddy\ai-knowledge-base"
```

3. Run `git pull` from the repo root.
4. Read these files first:
   - `AI经验库\00_总入口.md`
   - `AI经验库\01_项目总览.md`
   - `AI经验库\02_沉淀规则.md`
5. If the user names a project, read:
   - `AI经验库\项目经验\<项目名>\项目状态.md`
   - relevant `经验卡片.md` or `Skill卡片.md`
6. If the project is unclear, infer from strong keywords. If confidence is low, ask one concise question or save to `AI经验库\待整理\临时记录.md` only when the user asked to save.

## Write Workflow

Use this when the user says "沉淀一下", "沉淀经验", "存到经验库", "更新项目状态", or similar.

1. Run `git pull` from the repo root before editing.
2. Read `AI经验库\02_沉淀规则.md`.
3. Classify the content into exactly one primary destination:
   - Project experience: `AI经验库\项目经验\<项目名>\经验卡片.md`
   - Project Skill: `AI经验库\项目经验\<项目名>\Skill卡片.md`
   - Project status: `AI经验库\项目经验\<项目名>\项目状态.md`
   - Common Skill: `AI经验库\通用Skill\Skill卡片.md`
   - Unclear: `AI经验库\待整理\临时记录.md`
4. If this is a new project, create:
   - `AI经验库\项目经验\<项目名>\经验卡片.md`
   - `AI经验库\项目经验\<项目名>\Skill卡片.md`
   - `AI经验库\项目经验\<项目名>\项目状态.md`
   - Update `AI经验库\01_项目总览.md`
5. Write only distilled reusable content:
   - verified conclusions
   - clear steps
   - reusable prompts
   - pitfalls
   - current status and next actions
6. Mark unverified items as `待验证`.
7. Run validation commands:

```powershell
git status --short
git diff --check
```

8. Commit and push.
   - If the worktree contains only intentional changes, `git add -A` is acceptable.
   - If unrelated dirty files exist, stage only touched paths with `git add -- <path1> <path2>`.

```powershell
git commit -m "沉淀: <简短描述>"
git push
```

9. Report the commit hash and any uncommitted unrelated files that remain.

## Sharing With Another Person

The other person needs:

- Git installed.
- GitHub access to the shared repository.
- A local clone at `C:\Users\<用户名>\WorkBuddy\ai-knowledge-base`.
- This skill folder copied to their Codex skills directory:

```text
C:\Users\<用户名>\.codex\skills\experience-library-sync
```

If their AI tool cannot load Codex skills, give them this startup prompt:

```md
读经验库。

固定经验库 Git 仓库路径：
C:\Users\<用户名>\WorkBuddy\ai-knowledge-base

远端仓库：
https://github.com/wang-zefeng/-ai-knowledge-base.git

先 git pull，读取 AI经验库\00_总入口.md、AI经验库\01_项目总览.md、AI经验库\02_沉淀规则.md。
以后我说“沉淀一下”，都写入这个仓库的 AI经验库 目录，并执行 git commit + git push。
不要写当前项目仓库，不要新建 GitHub 仓库，不要上传密钥、素材、输出视频或日志。
```

## Completion Checklist

Before saying the save is complete, verify:

- The correct repo root was used.
- The destination file matches `02_沉淀规则.md`.
- No secrets or bulky generated files were included.
- `git commit` succeeded.
- `git push` succeeded.
- The final response includes changed files, validation, risks, and next step.
