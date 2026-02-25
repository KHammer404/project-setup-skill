---
name: verify
description: Verify current implementation against TODO tasks. Runs build/lint/test, checks git changes, and generates a verification report.
allowed-tools: Read, Bash, Glob, Grep, Write
---

Verify the current in-progress task implementation. Follow these steps exactly:

## Step 1: Identify current task

Read `.project/TODO.md` and find the task(s) under "🔥 In Progress" section.

If no task is in progress, report:
```
⚠️ 현재 진행 중인 태스크가 없습니다.
TODO.md에서 In Progress 항목을 확인해주세요.
```
And stop here.

## Step 2: Check what changed

Run `git status` and `git diff --stat` to identify all files that were created or modified.

If this is not a git repository, skip git checks and instead list recently modified files using:
```bash
find . -name "*.ts" -o -name "*.tsx" -o -name "*.js" -o -name "*.jsx" -o -name "*.py" -o -name "*.css" | head -30
```

## Step 3: Run project checks

Check for available tooling and run what exists. Check in this order:

1. **package.json** — look for these scripts and run them if they exist:
   - `npm run build` or `next build` (build check)
   - `npm run lint` (lint check)
   - `npm run type-check` or `npx tsc --noEmit` (type check)
   - `npm test` (only if test scripts exist AND there are test files)

2. **If no package.json**, look for:
   - `Cargo.toml` → `cargo check && cargo clippy`
   - `pyproject.toml` / `requirements.txt` → `python -m py_compile` on changed files
   - `go.mod` → `go build ./... && go vet ./...`

3. If nothing is found, skip this step and note it in the report.

**IMPORTANT:** Run each check with a timeout. If a check fails, record the error but continue with remaining checks. Do NOT stop at the first failure.

## Step 4: Generate verification report

Read the in-progress task details from TODO.md and compare against actual changes. Then output the report in this format:

```
🔍 검증 리포트

📋 태스크: [In Progress task name from TODO.md]

📂 변경된 파일:
- [file1] (신규/수정)
- [file2] (신규/수정)
- ...

✅ 통과한 검사:
- [x] 빌드 성공
- [x] 린트 통과
- [x] 타입 체크 통과

❌ 실패한 검사: (없으면 이 섹션 생략)
- [ ] 린트 오류: [간단한 설명]

📊 태스크 vs 구현 매칭:
- [x] 세부 태스크 1 → [구현된 파일/위치]
- [ ] 세부 태스크 2 → ⚠️ 미구현
- [x] 세부 태스크 3 → [구현된 파일/위치]

💡 제안사항: (있으면)
- [발견한 개선점이나 누락 사항]

👉 모두 확인되면 "next step!" 으로 다음 태스크를 진행하세요.
   수정이 필요하면 수정 사항을 말씀해주세요.
```

## Step 5: Save verification record

Append the verification result summary to `.project/NOTES.md` under the "🔧 Technical Notes" section with today's date:

```markdown
### [날짜] [태스크명] 검증
- 결과: ✅ 통과 / ⚠️ 일부 이슈 / ❌ 실패
- 변경 파일: N개
- 발견 이슈: [간단 요약 또는 "없음"]
```
