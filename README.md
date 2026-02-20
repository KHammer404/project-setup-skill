# Project Setup Skill

워크플로우 기반 프로젝트 관리 구조를 자동으로 설정하는 Claude Code 스킬입니다.

## 🎯 기능

이 스킬은 다음과 같은 개발 워크플로우를 자동화합니다:

```
토론 → "정리해줘" → 확인 → "진행해줘" → 구현 → "next step!" → 반복
```

### 자동 생성되는 파일

```
.project/
├── TODO.md                    # 태스크 트래킹
├── NOTES.md                   # 토론 요약 & 아이디어
├── WORKFLOW.md                # 워크플로우 가이드
├── scripts/
│   ├── status.bat            # Windows 상태 확인
│   └── status.sh             # Linux/Mac 상태 확인
└── docs/
    ├── context/              # 프로젝트 컨텍스트
    ├── design/               # 설계 문서 (+ TEMPLATE.md)
    ├── implementations/      # 구현 문서 (+ TEMPLATE.md)
    ├── prompts/              # 저장된 프롬프트
    └── roadmap/              # 로드맵

.claude/memory/
└── MEMORY.md                 # 워크플로우 트리거 (자동 로드)

CLAUDE.md                     # 프로젝트 가이드
```

## 📥 설치

1. 이 레포지토리를 클론:
```bash
cd ~/.claude/skills
git clone https://github.com/KHammer404/project-setup-skill.git setup-project
```

2. Claude Code 재시작

## 🚀 사용법

### 1. 프로젝트 초기화

프로젝트 디렉토리에서:
```
/setup-project
```

### 2. 워크플로우 시작

**Step 1: 토론**
```
사용자: "소셜 로그인 기능 만들고 싶어"
AI: "어떤 플랫폼을 지원할까요?"
사용자: "구글이랑 카카오!"
...
```

**Step 2: 정리**
```
사용자: "정리해줘"
```
→ AI가 자동으로:
- `.project/NOTES.md` 에 토론 요약 추가
- `.project/TODO.md` 에 태스크 분할 추가
- `.project/docs/design/` 에 설계 문서 생성 (필요시)

**Step 3: 구현**
```
사용자: "진행해줘"
```
→ AI가 자동으로:
- 첫 번째 태스크 구현
- `TODO.md` 업데이트
- `.project/docs/implementations/` 에 구현 문서 작성

**Step 4: 반복**
```
사용자: "next step!"
```
→ AI가 다음 태스크 구현

## 🎯 핵심 명령어

| 명령어 | 효과 |
|--------|------|
| `정리해줘` | 토론 내용을 NOTES.md, TODO.md, docs/ 에 정리 |
| `진행해줘` | 첫 번째 태스크 구현 시작 |
| `next step!` | 다음 태스크 구현 |

## 📚 워크플로우 트리거

스킬 설치 후, AI는 다음 명령어를 자동으로 인식합니다:

### "정리해줘" / "organize this"
- 토론 요약을 NOTES.md에 추가
- 태스크를 TODO.md에 분할
- 설계 문서 생성 (필요시)

### "진행해줘" / "proceed" / "start"
- TODO.md에서 첫 태스크를 In Progress로 이동
- 코드 구현
- 구현 문서 작성

### "next step!" / "다음" / "next"
- 다음 태스크로 이동
- 구현 반복

## 💡 예시

```
# 1. 프로젝트 시작
/setup-project

# 2. 기능 토론
"Next.js로 대시보드 만들고 싶어. 차트도 보여주고."
"어떤 차트 라이브러리 쓸까?"
"Recharts가 좋을 것 같은데?"
...

# 3. 정리
"정리해줘"

# AI가 자동으로 생성:
# - .project/NOTES.md (토론 요약)
# - .project/TODO.md (태스크 3개)
# - .project/docs/design/DASHBOARD_DESIGN.md

# 4. 확인 후 구현
"진행해줘"

# AI가 첫 태스크 구현 완료
# - src/components/Dashboard.tsx 생성
# - .project/docs/implementations/DASHBOARD_IMPLEMENTATION.md 생성

# 5. 다음 태스크
"next step!"
```

## 🔧 커스터마이징

### MEMORY.md 수정

`.claude/memory/MEMORY.md`에서 워크플로우 트리거를 수정할 수 있습니다.

### 템플릿 수정

`.project/docs/design/TEMPLATE.md`와 `.project/docs/implementations/TEMPLATE.md`를 프로젝트에 맞게 수정하세요.

## 📖 상세 문서

프로젝트 초기화 후 `.project/WORKFLOW.md`를 참고하세요.

## 🤝 기여

이슈나 PR은 언제나 환영합니다!

## 📄 라이선스

MIT License

---

Made with ❤️ by KHammer404 & Claude Sonnet 4.5
