---
name: organize
description: Organize current discussion into structured project docs. Summarizes discussion into NOTES.md, creates tasks in TODO.md, and generates design docs.
allowed-tools: Read, Write, Bash, Glob, Grep
---

Organize the current conversation's discussion into structured project documents. Follow these steps exactly:

## Step 1: Read current state

Read these files to understand existing state:
- `.project/TODO.md`
- `.project/NOTES.md`

If `.project/` directory does not exist, tell the user:
```
⚠️ .project/ 디렉토리가 없습니다. 먼저 /setup-project 를 실행해주세요.
```
And stop here.

## Step 2: Summarize discussion

Analyze the current conversation and extract:
1. **What was discussed** — feature ideas, technical decisions, requirements
2. **Decisions made** — technology choices, architecture decisions, constraints
3. **Action items** — what needs to be implemented

## Step 3: Update NOTES.md

Append a new discussion summary to `.project/NOTES.md` under the "💬 토론 요약" section:

```markdown
### [오늘 날짜] [주제] 토론
**결정사항:**
- [Decision 1]
- [Decision 2]

**기술 선택:**
- [Technology choice and reasoning]

**다음 단계:**
- TODO.md에 추가된 태스크 참조
```

## Step 4: Update TODO.md

Break down the discussed feature into concrete tasks and add them to `.project/TODO.md` under the "📅 Planned" section:

```markdown
### [기능명]
- [ ] 세부 태스크 1
- [ ] 세부 태스크 2
- [ ] 세부 태스크 3
```

Each task should be:
- Small enough to implement in one session
- Specific enough that the implementation is unambiguous
- Ordered by dependency (prerequisite tasks first)

## Step 5: Create design doc (if needed)

If the discussion involved significant architecture or design decisions, create a design document at `.project/docs/design/[FEATURE_NAME]_DESIGN.md` using the template in `.project/docs/design/TEMPLATE.md`.

Skip this step if the discussion was simple (e.g., a bug fix or minor feature).

## Step 6: Report results

Output a summary:

```
✅ 정리 완료!

📄 업데이트된 파일:
- .project/NOTES.md (토론 요약 추가)
- .project/TODO.md ([N]개 태스크 추가)
- .project/docs/design/[NAME]_DESIGN.md (설계 문서 생성) ← 해당시만

📋 추가된 태스크:
1. [태스크 1]
2. [태스크 2]
3. [태스크 3]

👉 확인하시고 /proceed 로 구현을 시작하세요.
   수정이 필요하면 말씀해주세요.
```
