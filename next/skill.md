---
name: next
description: Move to the next planned task in TODO.md and start implementation. Continues the development cycle.
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
---

Advance to the next planned task and implement it. Follow these steps exactly:

## Step 1: Check current state

Read `.project/TODO.md` and check:

1. **If there's a task still in "🔥 In Progress"** that hasn't been completed:
   ```
   ⚠️ 아직 완료되지 않은 태스크가 있습니다:
   - [태스크명]

   먼저 완료하거나, 수정이 필요하면 말씀해주세요.
   /verify 로 현재 구현을 검증할 수 있습니다.
   ```
   And stop here.

2. **If there are no more planned tasks:**
   ```
   🎉 모든 태스크 완료!

   ✅ Completed:
   - [x] 완료 태스크 1
   - [x] 완료 태스크 2
   - ...

   📚 생성된 문서:
   - .project/docs/implementations/[list all]

   🚀 새로운 기능을 토론하고 /organize 로 정리하세요!
   ```
   And stop here.

## Step 2: Move next task to In Progress

Update `.project/TODO.md`:
1. Take the first task group from "📅 Planned"
2. Move it to "🔥 In Progress"

## Step 3: Read context

Before implementing, read relevant context:
1. Check `.project/docs/design/` for any related design documents
2. Check `.project/NOTES.md` for relevant discussion summaries
3. Read `CLAUDE.md` for project conventions

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

Create `.project/docs/implementations/[FEATURE_NAME]_IMPLEMENTATION.md` using the template.

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

📊 전체 진행률:
- 완료: [N]개
- 남은 태스크: [M]개

👉 확인 후 /verify 로 검증하거나, /next 로 계속 진행하세요.
   수정이 필요하면 말씀해주세요.
```
