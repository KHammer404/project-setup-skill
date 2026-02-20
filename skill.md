---
name: setup-project
description: Initialize a new project with standard workflow files. Creates TODO.md for task tracking, NOTES.md for ideas and prompts, scripts/ folder with status scripts, and configures MEMORY.md for automatic context loading. Use when starting a new project or setting up project management structure.
disable-model-invocation: true
allowed-tools: Read, Write, Bash
---

Set up the standard project management structure for this repository.

## Step 0: Create .project directory

Create the `.project/` directory if it does not exist.

## Step 1: Create TODO.md

Create `.project/TODO.md` with this content:

```markdown
# TODO & Tasks

> 💡 **워크플로우**: 토론 → "정리해줘" → 확인 → "진행해줘" → 구현 → "next step!"

## 🔥 In Progress

**현재 작업중인 태스크가 여기 표시됩니다**

- [ ]

## 📅 Planned

**다음에 구현할 태스크들**

### 기능명
- [ ] 세부 태스크 1
- [ ] 세부 태스크 2

## ✅ Completed

**완료된 태스크들 (최신순)**

- [x] Project initialized

## 🗑️ Backlog

**나중에 고려할 아이디어들**

- [ ]
```

## Step 2: Create NOTES.md

Create `.project/NOTES.md` with this content:

```markdown
# Development Notes & Ideas

## 💬 토론 요약 (Discussion Summaries)

**토론 내용을 여기에 정리합니다. "정리해줘"라고 요청하면 AI가 자동으로 작성합니다.**

### [날짜] 기능명 토론
**결정사항:**
- 결정 1
- 결정 2

**기술 선택:**
- 선택한 기술과 이유

**다음 단계:**
- TODO.md에 추가된 태스크 참조

---

## 💡 Ideas & Future Features

### Feature Name
- Description
- Goals

---

## 📝 Saved Prompts

### Prompt Title

\`\`\`
[Describe what you want to build]

Requirements:
1.
2.

Current architecture:
-

Expected outcome:
-
\`\`\`

---

## 🔧 Technical Notes

### Note Title
- Context
- Solution
- Code location

---

## 🎨 Design Patterns

### Pattern Name
- Pattern type
- Implementation location

---

## 📚 References

### Useful Libraries
- Library name - Description

### Documentation
- Link - Description

---

## 🐛 Known Issues

1. **Issue title**
   - Cause:
   - Workaround:
   - TODO:

---

## 💭 Random Ideas

- Idea 1
- Idea 2
```

## Step 3: Create scripts/ folder with status scripts

Create the `.project/scripts/` directory if it does not exist.

Create `.project/scripts/status.bat`:

```bat
@echo off
chcp 65001 > nul
REM Project Status Script (Windows)
REM Usage: scripts\status.bat

echo ================================================
echo Project Status
echo ================================================
echo.

cd /d "%~dp0.."

REM TODO.md Summary
if exist "TODO.md" (
    echo [CURRENT TASKS]
    echo ================================================
    echo.
    echo In Progress:
    findstr /R /C:"^- \[" TODO.md | findstr /V /C:"[x]" | more /P
    echo.
    echo Next Up:
    findstr /R /C:"^###" TODO.md | more /P
    echo.
) else (
    echo WARNING: TODO.md not found
    echo.
)

REM NOTES.md Summary
if exist "NOTES.md" (
    echo [RECENT IDEAS]
    echo ================================================
    findstr /R /C:"^###" NOTES.md | more /P
    echo.
) else (
    echo WARNING: NOTES.md not found
    echo.
)

echo ================================================
echo For full details, read TODO.md and NOTES.md
echo ================================================
pause
```

Create `.project/scripts/status.sh`:

```bash
#!/bin/bash
# Project Status Script
# Usage: bash scripts/status.sh

echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "📊 Project Status"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo ""

# Project root (relative to scripts/)
PROJECT_ROOT="$(cd "$(dirname "$0")/.." && pwd)"

# TODO.md Summary
if [ -f "$PROJECT_ROOT/TODO.md" ]; then
    echo "📋 CURRENT TASKS:"
    echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

    # Extract "In Progress" section
    echo "🔥 In Progress:"
    sed -n '/## 🔥 In Progress/,/## 📅 Planned/p' "$PROJECT_ROOT/TODO.md" | grep -E '^\s*-\s*\[' | head -5
    echo ""

    # Extract "Planned" section
    echo "📅 Next Up:"
    sed -n '/## 📅 Planned/,/## ✅ Completed/p' "$PROJECT_ROOT/TODO.md" | grep -E '^###' | head -3
    echo ""
else
    echo "⚠️  TODO.md not found"
    echo ""
fi

# NOTES.md Summary
if [ -f "$PROJECT_ROOT/NOTES.md" ]; then
    echo "💡 RECENT IDEAS:"
    echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

    # Extract first few ideas
    sed -n '/## 💡 Ideas & Future Features/,/## 📝 Saved Prompts/p' "$PROJECT_ROOT/NOTES.md" | grep -E '^###' | head -3
    echo ""
else
    echo "⚠️  NOTES.md not found"
    echo ""
fi

# Quick Stats
echo "📈 QUICK STATS:"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
if [ -f "$PROJECT_ROOT/TODO.md" ]; then
    IN_PROGRESS=$(grep -c '^\s*-\s*\[' "$PROJECT_ROOT/TODO.md" 2>/dev/null || echo "0")
    COMPLETED=$(sed -n '/## ✅ Completed/,/## 🗑️ Backlog/p' "$PROJECT_ROOT/TODO.md" | grep -c '^\s*-\s*\[x\]' 2>/dev/null || echo "0")
    echo "  Tasks In Progress: $IN_PROGRESS"
    echo "  Tasks Completed: $COMPLETED"
fi

if [ -f "$PROJECT_ROOT/NOTES.md" ]; then
    SAVED_PROMPTS=$(grep -c '^###' "$PROJECT_ROOT/NOTES.md" 2>/dev/null || echo "0")
    echo "  Saved Prompts/Ideas: $SAVED_PROMPTS"
fi

echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "💬 For full details, read TODO.md and NOTES.md"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
```

Make `.project/scripts/status.sh` executable:

```bash
chmod +x .project/scripts/status.sh
```

## Step 4: Create docs/ folder structure

Create the `.project/docs/` directory structure with README templates.

Create `.project/docs/context/README.md`:

```markdown
# Context

This folder contains project context and background information.

## What to include:
- Project overview and goals
- Requirements and specifications
- Technical stack documentation
- Architecture decisions

## Example files:
- `PROJECT_CONTEXT.md` - Overall project description
- `REQUIREMENTS.md` - Feature requirements
- `TECH_STACK.md` - Technology choices and rationale
```

Create `.project/docs/design/README.md`:

```markdown
# Design

This folder contains design documents and system specifications.

## What to include:
- System architecture designs
- API specifications
- Database schemas
- Component designs

## Example files:
- `ARCHITECTURE.md` - System architecture
- `API_DESIGN.md` - API endpoints and contracts
- `DATABASE_SCHEMA.md` - Database structure
```

Create `.project/docs/prompts/README.md`:

```markdown
# Prompts

This folder contains saved prompts and AI instructions for complex tasks.

## What to include:
- Multi-step implementation prompts
- Complex feature requests
- Refactoring instructions
- Testing scenarios

## Example files:
- `feature_X_implementation.txt` - Detailed feature prompt
- `refactor_Y.md` - Refactoring instructions
```

Create `.project/docs/roadmap/README.md`:

```markdown
# Roadmap

This folder contains project roadmaps and planning documents.

## What to include:
- Feature roadmaps
- Version planning
- Milestone definitions
- Long-term vision

## Example files:
- `ROADMAP.md` - Project roadmap
- `MILESTONES.md` - Release milestones
- `VISION.md` - Long-term vision
```

Create `.project/docs/implementations/README.md`:

```markdown
# Implementations

This folder contains implementation summary documents for completed features.

## What to include:
- Files created/modified
- Features implemented
- Testing results
- API endpoints (if applicable)
- Usage instructions

## Naming convention:
- `{FEATURE}_IMPLEMENTATION.md`

## Example files:
- `MONITORING_IMPLEMENTATION.md` - Monitoring system implementation
- `AUTH_IMPLEMENTATION.md` - Authentication system implementation
- `API_IMPLEMENTATION.md` - API endpoints implementation
```

Create `.project/docs/design/TEMPLATE.md`:

```markdown
# [기능명] 설계 문서

> 작성일: [날짜]
> 상태: 🎯 설계 중 / ✅ 구현 완료

## 📋 개요

**목적:**
- 이 기능이 해결하는 문제

**핵심 기능:**
1. 기능 1
2. 기능 2

## 🏗️ 기술 스택

- **프론트엔드:**
- **백엔드:**
- **데이터베이스:**
- **기타:**

## 📐 설계

### API 엔드포인트
\`\`\`
GET /api/example
POST /api/example
\`\`\`

### 데이터베이스 스키마
\`\`\`sql
CREATE TABLE example (
  id INT PRIMARY KEY,
  name VARCHAR(255)
);
\`\`\`

### 컴포넌트 구조
\`\`\`
src/
├── components/
│   └── Example.tsx
└── services/
    └── exampleService.ts
\`\`\`

## 🔄 구현 순서

1. [ ] 태스크 1
2. [ ] 태스크 2
3. [ ] 태스크 3

## 📝 참고사항

- 주의할 점
- 참고 링크
```

Create `.project/docs/implementations/TEMPLATE.md`:

```markdown
# [기능명] 구현 완료

> 구현일: [날짜]
> 구현자: Claude + [사용자명]

## ✅ 완료된 작업

- [x] 태스크 1
- [x] 태스크 2
- [x] 태스크 3

## 📂 생성/수정된 파일

### 생성된 파일
- \`src/example.ts\` - 설명

### 수정된 파일
- \`src/app.ts\` - 변경 내용

## 🎯 구현된 기능

### 기능 1
- 설명
- 사용법

### 기능 2
- 설명
- 사용법

## 🧪 테스트

**테스트 방법:**
\`\`\`bash
npm test
\`\`\`

**테스트 결과:**
- ✅ 기능 1 테스트 통과
- ✅ 기능 2 테스트 통과

## 📚 API 문서 (해당시)

### GET /api/example
\`\`\`json
{
  "response": "example"
}
\`\`\`

## 🔗 관련 문서

- 설계 문서: \`docs/design/EXAMPLE_DESIGN.md\`
- TODO 항목: \`.project/TODO.md\`

## 💡 배운 점 / 이슈

- 배운 점 1
- 해결한 이슈 1
```

## Step 5: Update .claude/memory/MEMORY.md

Create the `.claude/memory/` directory if it does not exist.

If `.claude/memory/MEMORY.md` does not exist, create it with this Session Start Protocol:

```markdown
# Project Memory

## 🚀 CRITICAL: Session Start Protocol

**MANDATORY ACTION - YOU MUST DO THIS:**

When a user:
- Starts a new conversation
- Asks "what should I work on?" / "current status?" / "what's next?"
- Or ANY variation of asking about work/tasks

**YOU MUST IMMEDIATELY:**
1. 📋 Read \`.project/TODO.md\` in full
2. 💡 Read \`.project/NOTES.md\` (at least the Ideas & Saved Prompts sections)
3. 📊 Provide a status update:
   \`\`\`
   📋 Current Status:
   - 🔥 In Progress: [list from TODO]
   - 📅 Next Up: [planned tasks]
   - 💡 Recent Ideas: [from NOTES]
   \`\`\`

**This is NOT optional. Execute this protocol automatically.**

Alternative: User can also run \`bash .project/scripts/status.sh\` for quick overview.

## 🔄 Workflow Triggers

**사용자가 이렇게 말하면 자동으로 실행:**

### "정리해줘" / "organize this" / ".project에 정리"
1. 토론 내용을 요약하여 \`.project/NOTES.md\` 의 "토론 요약" 섹션에 추가
2. 구현할 기능을 태스크로 분할하여 \`.project/TODO.md\` 에 추가
3. 필요시 \`.project/docs/design/\` 에 설계 문서 작성
4. 사용자에게 "확인하시고 '진행해줘'라고 말씀해주세요!" 안내

### "진행해줘" / "proceed" / "start"
1. \`.project/TODO.md\` 에서 "📅 Planned" 의 첫 번째 태스크를 "🔥 In Progress" 로 이동
2. 해당 태스크 구현 시작
3. 구현 완료 후:
   - TODO.md에서 태스크를 체크 ✅
   - "✅ Completed"로 이동
   - \`.project/docs/implementations/\` 에 구현 문서 작성
4. 사용자에게 "확인하시고 'next step!'이라고 말씀해주세요!" 안내

### "next step!" / "다음" / "next"
1. \`.project/TODO.md\` 에서 다음 태스크 확인
2. 다음 태스크를 "🔥 In Progress"로 이동
3. 구현 시작 (위 "진행해줘" 프로세스 반복)

## 📁 File Organization Rules

**모든 문서는 적절한 위치에:**
- \`.project/TODO.md\`: 태스크 트래킹
- \`.project/NOTES.md\`: 토론 요약, 아이디어, 기술 결정
- \`.project/docs/context/\`: 프로젝트 컨텍스트, 비전
- \`.project/docs/design/\`: 설계 문서 (API, DB 스키마 등)
- \`.project/docs/implementations/\`: 구현 완료 문서

## Project Structure
[Add your project structure here]

## Key Learnings
[Document important learnings as the project progresses]
```

If `.claude/memory/MEMORY.md` already exists, check if the Session Start Protocol is present. If not, prepend it to the file.

## Step 5.5: Create WORKFLOW.md

Create `.project/WORKFLOW.md` with the workflow guide:

```markdown
# 🔄 Development Workflow

이 프로젝트의 표준 개발 워크플로우입니다.

## 📖 워크플로우 단계

### 1️⃣ 토론 단계 (Discussion)

**목적:** 기능에 대해 AI와 충분히 논의하고 요구사항을 명확히 함

\`\`\`
사용자: "소셜 로그인 기능 만들고 싶어"
AI: "어떤 플랫폼을 지원할까요? 구글/카카오/깃허브?"
사용자: "구글이랑 카카오!"
AI: "인증 방식은 JWT vs Session 중 어떤 걸로 할까요?"
사용자: "JWT로!"
AI: "토큰 저장은 localStorage vs Cookie?"
...토론 계속...
\`\`\`

### 2️⃣ 정리 단계 (Organization)

**트리거:** 사용자가 **"정리해줘"** 또는 **".project에 정리"** 라고 말함

**AI가 자동으로 수행:**
1. ✅ \`NOTES.md\`에 토론 요약 추가
2. ✅ \`TODO.md\`에 구현 태스크 분할 추가
3. ✅ \`docs/design/\`에 설계 문서 생성 (필요시)

**출력 예시:**
\`\`\`
✅ 정리 완료!

생성/업데이트된 파일:
- .project/NOTES.md (토론 요약 추가)
- .project/TODO.md (3개 태스크 추가)
- .project/docs/design/AUTH_DESIGN.md (설계 문서 생성)

📋 추가된 태스크:
1. Google OAuth 연동
2. Kakao OAuth 연동
3. JWT 토큰 발급 로직

👉 확인하시고 "진행해줘"라고 말씀해주세요!
\`\`\`

### 3️⃣ 확인 단계 (Review)

**사용자가 할 일:**
- \`.project/TODO.md\` 읽기
- \`.project/NOTES.md\` 토론 요약 확인
- \`docs/design/\` 설계 문서 확인 (있다면)

**질문:**
"내가 이해한 것과 같은가?"

- ✅ 같다면: **"진행해줘!"**
- ❌ 다르다면: 추가 토론 또는 수정 요청

### 4️⃣ 구현 단계 (Implementation)

**트리거:** 사용자가 **"진행해줘"** 또는 **"start"** 라고 말함

**AI가 자동으로 수행:**
1. ✅ \`TODO.md\`에서 첫 번째 Planned 태스크를 In Progress로 이동
2. ✅ 코드 구현
3. ✅ 구현 완료 후:
   - TODO.md에서 태스크 체크 및 Completed로 이동
   - \`docs/implementations/\` 에 구현 문서 작성

**출력 예시:**
\`\`\`
✅ 구현 완료!

📂 생성/수정된 파일:
- src/auth/google.ts (신규 생성)
- src/auth/index.ts (업데이트)

📄 문서:
- .project/docs/implementations/GOOGLE_AUTH_IMPLEMENTATION.md

📋 TODO 업데이트:
- [x] Google OAuth 연동 ✅

👉 확인하시고 "next step!"이라고 말씀해주세요!
\`\`\`

### 5️⃣ 검증 단계 (Verification)

**사용자가 할 일:**
- 구현된 코드 확인
- 구현 문서 읽기
- 테스트 실행 (필요시)

**질문:**
"구현이 올바른가?"

- ✅ 맞다면: **"next step!"**
- ❌ 틀렸다면: 수정 요청

### 6️⃣ 반복 (Iteration)

**트리거:** 사용자가 **"next step!"** 또는 **"다음"** 이라고 말함

**AI가 자동으로 수행:**
1. ✅ \`TODO.md\`에서 다음 Planned 태스크를 In Progress로 이동
2. ✅ 4️⃣ 구현 단계로 돌아가서 반복

**모든 태스크가 완료되면:**
\`\`\`
🎉 모든 태스크 완료!

✅ Completed:
- [x] Google OAuth 연동
- [x] Kakao OAuth 연동
- [x] JWT 토큰 발급 로직

📚 생성된 문서:
- docs/design/AUTH_DESIGN.md
- docs/implementations/GOOGLE_AUTH_IMPLEMENTATION.md
- docs/implementations/KAKAO_AUTH_IMPLEMENTATION.md
- docs/implementations/JWT_IMPLEMENTATION.md

🚀 다음에 작업할 내용이 있다면 말씀해주세요!
\`\`\`

## 🎯 핵심 명령어

| 명령어 | 단계 | 효과 |
|--------|------|------|
| **"정리해줘"** | 토론 → 정리 | NOTES.md, TODO.md, docs/ 업데이트 |
| **"진행해줘"** | 정리 → 구현 | 첫 태스크 구현 시작 |
| **"next step!"** | 구현 → 다음 | 다음 태스크 구현 |

## 📁 파일 역할

| 파일 | 역할 |
|------|------|
| \`TODO.md\` | 태스크 트래킹 (In Progress, Planned, Completed) |
| \`NOTES.md\` | 토론 요약, 기술 결정 사항 |
| \`docs/design/\` | 설계 문서 (API, DB 스키마 등) |
| \`docs/implementations/\` | 구현 완료 문서 |

## 💡 팁

1. **한 번에 하나씩:** 한 태스크 완료 후 "next step!" 으로 다음으로
2. **확인은 필수:** 각 단계마다 사용자 확인 후 진행
3. **문서화:** 모든 결정과 구현은 자동으로 문서화됨
4. **유연성:** 중간에 방향 바꾸고 싶으면 언제든 새로운 토론 시작
```

## Step 6: Create CLAUDE.md (if it doesn't exist)

If `CLAUDE.md` does not already exist, create it with this basic template:

```markdown
# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## 🚀 Getting Started (Read This First!)

**When starting a new session or receiving a task:**
1. **Check \`.project/TODO.md\`** - See current tasks, planned features, and completed work
2. **Check \`.project/NOTES.md\`** - Review saved ideas, prompts, and technical notes
3. **Check \`.project/docs/\`** - For detailed design docs and context (if applicable)

These files contain the current project state, plans, and saved prompts that guide development.

## Build & Run Commands

TODO: Add your build and run commands here.

## Architecture

TODO: Describe your system architecture here.

## Key Files

TODO: List important files and their roles.
```

If `CLAUDE.md` already exists, do not overwrite it — just report that it already exists.

## Step 7: Report results

After completing all steps, print a summary:

```
✅ 프로젝트 워크플로우 셋업 완료!

📂 생성된 파일:
- .project/TODO.md (태스크 트래킹 - 워크플로우 가이드 포함)
- .project/NOTES.md (토론 요약 & 아이디어)
- .project/WORKFLOW.md (상세 워크플로우 가이드)
- .project/docs/ (문서 구조)
  ├── context/ (프로젝트 컨텍스트)
  ├── design/ (설계 문서 + TEMPLATE.md)
  ├── implementations/ (구현 문서 + TEMPLATE.md)
  ├── prompts/ (저장된 프롬프트)
  └── roadmap/ (로드맵)
- .project/scripts/status.bat (Windows 상태 확인)
- .project/scripts/status.sh (Linux/Mac 상태 확인)
- .claude/memory/MEMORY.md (워크플로우 트리거 자동 로드)
- CLAUDE.md (프로젝트 가이드) [신규 생성시]

🔄 워크플로우 사용법:

1. **토론:** AI와 만들고 싶은 기능에 대해 토론
   "소셜 로그인 만들고 싶어"

2. **정리:** 토론이 완벽하면
   👉 **"정리해줘"**
   → AI가 NOTES.md, TODO.md, docs/ 자동 업데이트

3. **확인:** 생성된 문서들 확인 후
   👉 **"진행해줘"**
   → AI가 첫 번째 태스크 구현 시작

4. **반복:** 구현 확인 후
   👉 **"next step!"**
   → AI가 다음 태스크 구현

📚 상세 가이드:
- .project/WORKFLOW.md 참고

💡 다음 단계:
1. AI와 첫 기능에 대해 토론 시작
2. 토론 완료 후 "정리해줘" 입력
3. 워크플로우 시작!
```
