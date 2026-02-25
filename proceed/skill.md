---
name: proceed
description: Start implementing the first planned task from TODO.md. Moves it to In Progress, implements the code, and creates implementation docs.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

Start implementing the first planned task from the project TODO. Follow these steps exactly:

## Step 1: Read TODO.md

Read `.project/TODO.md` and identify:
1. The first task group under "📅 Planned" section
2. All sub-tasks within that group

If `.project/` directory does not exist, tell the user:
```
⚠️ .project/ 디렉토리가 없습니다. 먼저 /setup-project 를 실행해주세요.
```
And stop here.

If there are no planned tasks:
```
⚠️ 구현할 태스크가 없습니다.
TODO.md의 Planned 섹션에 태스크를 추가해주세요.
토론 후 /organize 로 정리하면 자동으로 추가됩니다.
```
And stop here.

## Step 2: Move task to In Progress

Update `.project/TODO.md`:
1. Move the first task group from "📅 Planned" to "🔥 In Progress"
2. Keep all sub-tasks intact

## Step 3: Read context

Before implementing, read relevant context:
1. Check `.project/docs/design/` for any related design documents
2. Check `.project/NOTES.md` for relevant discussion summaries and technical decisions
3. Read `CLAUDE.md` for project conventions and architecture

## Step 4: Implement

Implement the task following the project's existing patterns and conventions.

For each sub-task:
1. Write the code
2. Check the sub-task in TODO.md: `- [x] 세부 태스크`

## Step 5: Update TODO.md

After implementation:
1. Move the completed task group from "🔥 In Progress" to "✅ Completed"
2. Ensure all sub-tasks are checked

## Step 6: Create implementation doc

Create `.project/docs/implementations/[FEATURE_NAME]_IMPLEMENTATION.md` using the template in `.project/docs/implementations/TEMPLATE.md`.

Include:
- Files created/modified
- Features implemented
- How to test
- Any notes or caveats

## Step 7: Report results

Output a summary:

```
✅ 구현 완료!

📋 태스크: [태스크명]

📂 생성/수정된 파일:
- [file1] (신규 생성)
- [file2] (수정)

📄 문서:
- .project/docs/implementations/[NAME]_IMPLEMENTATION.md

📋 TODO 업데이트:
- [x] 세부 태스크 1 ✅
- [x] 세부 태스크 2 ✅

👉 확인 후 /verify 로 검증하거나, /next 로 다음 태스크를 진행하세요.
   수정이 필요하면 말씀해주세요.
```
