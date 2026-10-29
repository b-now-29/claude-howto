<!-- i18n-source: INDEX.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 예제 - 전체 색인

이 문서는 모든 예제 파일을 기능 종류별로 정리한 전체 색인입니다.

> 한국어판 참고: 아래 경로는 원문 폴더 기준입니다. 레슨 문서(`README.md`, 템플릿 `.md`)의 번역본은 `ko/` 아래 같은 경로에 있고, 스크립트와 설정 파일(`.py`, `.sh`, `.json` 등)은 원문 폴더에만 있습니다.

## 요약 통계

- **전체 파일**: 100개 이상
- **분류**: 기능 분류 10개
- **플러그인**: 완성된 플러그인 3개
- **스킬**: 완성된 스킬 6개
- **훅**: 예제 훅 11개
- **바로 사용 가능**: 모든 예제

---

## 01. 슬래시 명령 (파일 10개)

자주 쓰는 워크플로를 사용자가 직접 호출하는 단축 명령입니다.

| 파일 | 설명 | 사용 사례 |
|------|------|-----------|
| `optimize.md` | 코드 최적화 분석기 | 성능 문제 찾기 |
| `pr.md` | 풀 리퀘스트 준비 | PR 워크플로 자동화 |
| `generate-api-docs.md` | API 문서 생성기 | API 문서 생성 |
| `commit.md` | 커밋 메시지 도우미 | 표준화된 커밋 |
| `setup-ci-cd.md` | CI/CD 파이프라인 설정 | DevOps 자동화 |
| `push-all.md` | 모든 변경 사항 푸시 | 빠른 푸시 워크플로 |
| `unit-test-expand.md` | 단위 테스트 커버리지 확장 | 테스트 자동화 |
| `doc-refactor.md` | 문서 리팩터링 | 문서 개선 |
| `pr-slash-command.png` | 스크린샷 예시 | 시각 자료 |
| `README.md` | 문서 | 설정과 사용 안내 |

**설치 경로**: `.claude/commands/`

**사용법**: `/optimize`, `/pr`, `/generate-api-docs`, `/commit`, `/setup-ci-cd`, `/push-all`, `/unit-test-expand`, `/doc-refactor`

---

## 02. 메모리 (파일 6개)

계속 유지되는 컨텍스트와 프로젝트 표준입니다.

| 파일 | 설명 | 범위 | 위치 |
|------|------|------|------|
| `project-CLAUDE.md` | 팀 프로젝트 표준 | 프로젝트 전체 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API 전용 규칙 | 디렉터리 | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` | 개인 선호 설정 | 사용자 | `~/.claude/CLAUDE.md` |
| `memory-saved.png` | 스크린샷: 메모리 저장 | - | 시각 자료 |
| `memory-ask-claude.png` | 스크린샷: Claude에게 묻기 | - | 시각 자료 |
| `README.md` | 문서 | - | 참고 자료 |

**설치**: 알맞은 위치에 복사합니다

**사용법**: Claude가 자동으로 불러옵니다

**AGENTS.md** (v2.1.277): 작업 디렉터리나 그 상위에 `CLAUDE.md`, `.claude/CLAUDE.md`, `CLAUDE.local.md`가 하나도 없으면 프로젝트 지침으로 읽습니다. `02-memory/README.md#agentsmd`를 참고하세요.

---

## 03. 스킬 (파일 23개)

스크립트와 템플릿을 갖추고 자동으로 호출되는 기능입니다.

### 코드 리뷰 스킬 (파일 5개)
```text
code-review-specialist/
├── SKILL.md                          # 스킬 정의
├── scripts/
│   ├── analyze-metrics.py            # 코드 지표 분석기
│   └── compare-complexity.py         # 복잡도 비교
└── templates/
    ├── review-checklist.md           # 리뷰 체크리스트
    └── finding-template.md           # 발견 사항 기록
```

**목적**: 보안, 성능, 품질 분석을 아우르는 포괄적인 코드 리뷰

**자동 호출**: 코드를 리뷰할 때

---

### 브랜드 보이스 스킬 (파일 4개)
```text
brand-voice/
├── SKILL.md                          # 스킬 정의
├── templates/
│   ├── email-template.txt            # 이메일 형식
│   └── social-post-template.txt      # 소셜 미디어 형식
└── tone-examples.md                  # 예시 메시지
```

**목적**: 커뮤니케이션에서 브랜드 보이스를 일관되게 유지

**자동 호출**: 마케팅 문구를 만들 때

---

### 문서 생성기 스킬 (파일 2개)
```text
doc-generator/
├── SKILL.md                          # 스킬 정의
└── generate-docs.py                  # Python 문서 추출기
```

**목적**: 소스 코드에서 포괄적인 API 문서 생성

**자동 호출**: API 문서를 만들거나 갱신할 때

---

### 리팩터링 스킬 (파일 5개)
```text
refactor/
├── SKILL.md                          # 스킬 정의
├── scripts/
│   ├── analyze-complexity.py         # 복잡도 분석기
│   └── detect-smells.py              # 코드 스멜 탐지기
├── references/
│   ├── code-smells.md                # 코드 스멜 카탈로그
│   └── refactoring-catalog.md        # 리팩터링 패턴
└── templates/
    └── refactoring-plan.md           # 리팩터링 계획 템플릿
```

**목적**: 복잡도 분석에 기반한 체계적인 코드 리팩터링

**자동 호출**: 코드를 리팩터링할 때

---

### Claude MD 스킬 (파일 1개)
```text
claude-md/
└── SKILL.md                          # 스킬 정의
```

**목적**: CLAUDE.md 파일 관리와 최적화

---

### 블로그 초안 스킬 (파일 3개)
```text
blog-draft/
├── SKILL.md                          # 스킬 정의
└── templates/
    ├── draft-template.md             # 블로그 초안 템플릿
    └── outline-template.md           # 블로그 개요 템플릿
```

**목적**: 일관된 구조로 블로그 글 초안 작성

**추가**: `README.md` - 스킬 개요와 사용 안내

**설치 경로**: `~/.claude/skills/` 또는 `.claude/skills/`

---

## 04. 서브에이전트 (파일 10개)

맞춤 기능을 갖춘 전문 AI 어시스턴트입니다.

| 파일 | 설명 | 도구 | 사용 사례 |
|------|------|------|-----------|
| `code-reviewer.md` | 코드 품질 분석 | Read, Grep, Glob, Bash | 포괄적인 리뷰 |
| `test-engineer.md` | 테스트 커버리지 분석 | Read, Write, Bash, Grep | 테스트 자동화 |
| `documentation-writer.md` | 문서 작성 | Read, Write, Grep | 문서 생성 |
| `secure-reviewer.md` | 보안 리뷰(읽기 전용) | Read, Grep | 보안 감사 |
| `implementation-agent.md` | 전체 구현 | Read, Write, Edit, Bash, Grep, Glob | 기능 개발 |
| `debugger.md` | 디버깅 전문가 | Read, Edit, Bash, Grep, Glob | 버그 조사 |
| `data-scientist.md` | 데이터 분석 전문가 | Bash, Read, Write | 데이터 워크플로 |
| `clean-code-reviewer.md` | 클린 코드 표준 | Read, Grep, Glob, Bash | 코드 품질 |
| `performance-optimizer.md` | 성능 병목 분석 | Read, Edit, Bash, Grep, Glob | 최적화 작업 |
| `README.md` | 문서 | - | 설정과 사용 안내 |

**설치 경로**: `.claude/agents/`

**사용법**: 메인 에이전트가 자동으로 위임합니다

---

## 05. MCP 프로토콜 (파일 5개)

외부 도구와 API 연동입니다.

| 파일 | 설명 | 연동 대상 | 사용 사례 |
|------|------|-----------|-----------|
| `github-mcp.json` | GitHub 연동 | GitHub API | PR·이슈 관리 |
| `database-mcp.json` | 데이터베이스 쿼리 | PostgreSQL/MySQL | 실시간 데이터 쿼리 |
| `filesystem-mcp.json` | 파일 작업 | 로컬 파일 시스템 | 파일 관리 |
| `multi-mcp.json` | 여러 서버 | GitHub + DB + Slack | 통합 연동 |
| `README.md` | 문서 | - | 설정과 사용 안내 |

**설치 경로**: `.mcp.json`(프로젝트 범위) 또는 `~/.claude.json`(사용자 범위)

**사용법**: `/mcp__github__list_prs` 등

---

## 06. 훅 (파일 12개)

이벤트에 따라 자동으로 실행되는 자동화 스크립트입니다.

| 파일 | 설명 | 이벤트 | 사용 사례 |
|------|------|--------|-----------|
| `format-code.sh` | 코드 자동 포맷 | PostToolUse (matcher: Write) | 코드 포맷 |
| `pre-commit.sh` | 커밋 전에 테스트 실행 | PreToolUse (matcher: Bash) | 테스트 자동화 |
| `pre-tool-check.sh` | 명령을 실행하기 전에 검증하고 감사 기록 | PreToolUse (matcher: Bash) | 안전장치, 감사 로그 |
| `security-scan.sh` | 보안 스캔 | PostToolUse (matcher: Write) | 보안 검사 |
| `dependency-check.sh` | 의존성 매니페스트의 취약점 스캔 | PostToolUse (matcher: Write) | 공급망 검사 |
| `log-bash.sh` | bash 명령 기록 | PostToolUse (matcher: Bash) | 명령 로그 |
| `notify-team.sh` | 알림 보내기 | PostToolUse (matcher: Bash) | 팀 알림 |
| `validate-prompt.sh` | 프롬프트 검증 | UserPromptSubmit | 입력 검증 |
| `session-end.sh` | 세션이 끝날 때 진행 상황 기록 | SessionEnd | 진행 상황 추적 |
| `context-tracker.py` | 컨텍스트 윈도 사용량 추적 | UserPromptSubmit, Stop | 컨텍스트 모니터링 |
| `context-tracker-tiktoken.py` | 토큰 기반 컨텍스트 추적 | UserPromptSubmit, Stop | 정확한 토큰 계산 |
| `README.md` | 문서 | - | 설정과 사용 안내 |

**설치 경로**: `~/.claude/settings.json`에서 설정합니다

**사용법**: 설정에 등록하면 자동으로 실행됩니다

**훅 유형** (5가지): `command`, `http`, `prompt`, `mcp_tool`, `agent` — 훅을 어떻게 실행하는지 정합니다.

**훅 이벤트** (33개, 4개 분류) — 훅을 언제 실행하는지 정합니다.
- 도구 훅: PreToolUse, PostToolUse, PostToolUseFailure, PostToolBatch, PermissionRequest, PermissionDenied
- 세션 훅: SessionStart, Setup, SessionEnd, Stop, StopFailure, SubagentStart, SubagentStop
- 작업 훅: UserPromptSubmit, UserPromptExpansion, MessageDisplay, TaskCompleted, TaskCreated, TeammateIdle (TaskCompleted/TaskCreated는 todo 도구가 켜져 있을 때만 발생합니다. todo 도구는 Claude 3.x 모델, Opus 4~4.7, Sonnet 4~4.6, Haiku 4.5에서만 기본으로 쓸 수 있습니다)
- 수명 주기 훅: ConfigChange, CwdChanged, DirectoryAdded, FileChanged, PreCompact, PostCompact, PreModelSwitch, PostModelSwitch, WorktreeCreate, WorktreeRemove, Notification, InstructionsLoaded, Elicitation, ElicitationResult

---

## 07. 플러그인 (완성된 플러그인 3개, 파일 39개)

여러 기능을 묶은 모음입니다.

### PR 리뷰 플러그인 (파일 10개)
```text
pr-review/
├── .claude-plugin/
│   └── plugin.json                   # 플러그인 매니페스트
├── commands/
│   ├── review-pr.md                  # 포괄적인 리뷰
│   ├── check-security.md             # 보안 검사
│   └── check-tests.md                # 테스트 커버리지 검사
├── agents/
│   ├── security-reviewer.md          # 보안 전문가
│   ├── test-checker.md               # 테스트 전문가
│   └── performance-analyzer.md       # 성능 전문가
├── mcp/
│   └── github-config.json            # GitHub 연동
├── hooks/
│   └── pre-review.js                 # 리뷰 전 검증
└── README.md                         # 플러그인 문서
```

**기능**: 보안 분석, 테스트 커버리지, 성능 영향

**명령**: `/review-pr`, `/check-security`, `/check-tests`

**설치**: `claude --plugin-dir ./07-plugins/pr-review` (저장소 루트에서 실행)

---

### DevOps 자동화 플러그인 (파일 15개)
```text
devops-automation/
├── .claude-plugin/
│   └── plugin.json                   # 플러그인 매니페스트
├── commands/
│   ├── deploy.md                     # 배포
│   ├── rollback.md                   # 롤백
│   ├── status.md                     # 시스템 상태
│   └── incident.md                   # 장애 대응
├── agents/
│   ├── deployment-specialist.md      # 배포 전문가
│   ├── incident-commander.md         # 장애 대응 조정자
│   └── alert-analyzer.md             # 알림 분석기
├── mcp/
│   └── kubernetes-config.json        # Kubernetes 연동
├── hooks/
│   ├── pre-deploy.js                 # 배포 전 검사
│   └── post-deploy.js                # 배포 후 작업
├── scripts/
│   ├── deploy.sh                     # 배포 자동화
│   ├── rollback.sh                   # 롤백 자동화
│   └── health-check.sh               # 상태 점검
└── README.md                         # 플러그인 문서
```

**기능**: Kubernetes 배포, 롤백, 모니터링, 장애 대응

**명령**: `/deploy`, `/rollback`, `/status`, `/incident`

**설치**: `claude --plugin-dir ./07-plugins/devops-automation` (저장소 루트에서 실행)

---

### 문서화 플러그인 (파일 14개)
```text
documentation/
├── .claude-plugin/
│   └── plugin.json                   # 플러그인 매니페스트
├── commands/
│   ├── generate-api-docs.md          # API 문서 생성
│   ├── generate-readme.md            # README 작성
│   ├── sync-docs.md                  # 문서 동기화
│   └── validate-docs.md              # 문서 검증
├── agents/
│   ├── api-documenter.md             # API 문서 전문가
│   ├── code-commentator.md           # 코드 주석 전문가
│   └── example-generator.md          # 예제 작성자
├── mcp/
│   └── github-docs-config.json       # GitHub 연동
├── templates/
│   ├── api-endpoint.md               # API 엔드포인트 템플릿
│   ├── function-docs.md              # 함수 문서 템플릿
│   └── adr-template.md               # ADR 템플릿
└── README.md                         # 플러그인 문서
```

**기능**: API 문서, README 생성, 문서 동기화, 검증

**명령**: `/generate-api-docs`, `/generate-readme`, `/sync-docs`, `/validate-docs`

**설치**: `claude --plugin-dir ./07-plugins/documentation` (저장소 루트에서 실행)

**추가**: `README.md` - 플러그인 개요와 사용 안내

---

## 08. 체크포인트와 되감기 (파일 2개)

대화 상태를 저장하고 다른 접근 방식을 탐색합니다.

| 파일 | 설명 | 내용 |
|------|------|------|
| `README.md` | 문서 | 체크포인트 종합 안내 |
| `checkpoint-examples.md` | 실제 사례 | 데이터베이스 마이그레이션, 성능 최적화, UI 반복 개선, 디버깅 |
| | | |

**핵심 개념**:
- **체크포인트**: 대화 상태의 스냅샷
- **되감기(Rewind)**: 이전 체크포인트로 돌아가기
- **분기점(Branch Point)**: 여러 접근 방식 탐색

**사용법**:
```text
# 체크포인트는 사용자 프롬프트마다 자동으로 만들어집니다
# 되감으려면 Esc를 두 번 누르거나 다음을 사용합니다:
/rewind
# 그다음 선택: Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, Summarize up to here, Never mind
```

**사용 사례**:
- 여러 구현 시도
- 실수에서 복구
- 안전한 실험
- 해결책 비교
- A/B 테스트

---

## 09. 고급 기능 (파일 4개)

복잡한 워크플로를 위한 고급 기능입니다.

| 파일 | 설명 | 기능 |
|------|------|------|
| `README.md` | 종합 안내 | 모든 고급 기능 문서 |
| `config-examples.json` | 설정 예제 | 사용 사례별 설정 10개 이상 |
| `planning-mode-examples.md` | 계획 예제 | REST API, 데이터베이스 마이그레이션, 리팩터링 |
| `setup-auto-mode-permissions.py` | auto 모드용 `permissions.allow` 초기 설정 | 여러 번 실행해도 결과가 같음, `--dry-run`과 선택형 플래그 |
| 동적 워크플로 | `/workflows`로 결정적 다중 에이전트 오케스트레이션 (v2.1.154) | 포괄적인 감사, 마이그레이션, 대규모 확장 |
| 예약 작업 | `/loop`와 cron 도구로 세션 범위 반복 작업 | 반복 워크플로 자동화 |
| Chrome 연동 | Claude in Chrome 확장 프로그램으로 내 Chrome에서 브라우저 자동화 | 웹 테스트와 스크래핑 |
| 원격 제어 (확장) | 연결 방법, 보안, 비교표, 기기 카드 | 원격 세션 관리 (이제 리서치 프리뷰가 아님) |
| 세션 간 메시징 | `SendMessage` / `ListAgents`, `notify_when_idle` 포함 (v2.1.236) | 같은 컴퓨터의 세션끼리 조율 |
| 키보드 사용자 지정 | 사용자 지정 키 바인딩, 코드(chord) 지원, 컨텍스트 | 나만의 단축키 |
| 데스크톱 앱 (확장) | 커넥터, launch.json, 엔터프라이즈 기능 | 데스크톱 연동 |
| | | |

**다루는 고급 기능**:

### 계획 모드
- 자세한 구현 계획 작성
- 시간 추정과 위험 평가
- 체계적인 작업 분해

### 확장 사고
- 복잡한 문제를 위한 깊은 추론
- 아키텍처 결정 분석
- 트레이드오프 평가

### 백그라운드 작업
- 작업을 막지 않는 장시간 작업
- 병렬 개발 워크플로
- 작업 관리와 모니터링

### 동적 워크플로 (v2.1.154)
- 수십에서 수백 개의 백그라운드 서브에이전트를 결정적으로 오케스트레이션
- 포괄적으로 다루기 위한 팬아웃 / 파이프라인 / 병렬 단계
- `/workflows`로 실행 내역을 봅니다. `/effort`의 Ultracode 토글(`/effort ultracode [on|off]`)은 effort 수준을 바꾸지 않고 세션에서 이 기능을 켭니다(v2.1.284). `ultracode` 설정을 쓰면 계속 유지됩니다
- v2.1.219부터 기본 규모 기준은 중간(에이전트 10개 미만 목표)입니다. `/config`의 **Dynamic workflow size**에서 바꿉니다

### 권한 모드
- **manual**: 읽기 외의 모든 작업 전에 묻습니다 (v2.1.200부터 Manual로 표시되며, 설정 값은 여전히 `default`)
- **acceptEdits**: 파일 편집은 자동 승인하고, 그 밖의 작업은 묻습니다
- **plan**: 읽기 전용 분석, 수정 없음
- **auto**: 모든 작업을 허용하되 백그라운드에서 안전 검사를 합니다. 분류기가 명령과 보호된 디렉터리 쓰기를 검토합니다(`autoMode` 설정 객체로 구성)
- **dontAsk**: 미리 승인된 도구만 씁니다. 원래 확인을 묻는 호출은 모두 자동으로 거부합니다. Claude는 `permissions.allow`에 맞는 호출, 읽기 전용 Bash 명령, `PreToolUse` 훅이 승인한 호출만 실행합니다
- **bypassPermissions**: 모두 허용 (`--permission-mode bypassPermissions` 또는 같은 효과의 `--dangerously-skip-permissions`로 켭니다)

### 헤드리스 모드 (`claude -p`)
- CI/CD 연동
- 자동 작업 실행
- 일괄 처리

### 세션 관리
- 여러 작업 세션
- 세션 전환과 저장
- 세션 유지

### 대화형 기능
- 키보드 단축키
- 명령 기록
- 탭 자동 완성
- 여러 줄 입력

### 설정
- 포괄적인 설정 관리
- 환경별 설정
- 프로젝트별 맞춤 설정

### 예약 작업
- `/loop` 명령으로 세션 범위 반복 작업
- cron 도구: CronCreate, CronList, CronDelete
- 반복 워크플로 자동화

### Chrome 연동
- Claude in Chrome 확장 프로그램으로 내 Chrome에서 브라우저 자동화
- 웹 테스트와 스크래핑 기능
- 페이지 상호작용과 데이터 추출

### 원격 제어 (확장)
- 연결 방법과 프로토콜
- 보안 고려 사항과 모범 사례
- 원격 접속 방식 비교표
- 리서치 프리뷰 종료 — `claude remote-control`을 실행 중인 컴퓨터가 Claude 앱의 Code 탭에 기기 카드로 나타납니다

### 세션 간 메시징
- 같은 컴퓨터의 세션끼리 `SendMessage`
- `notify_when_idle` — 상대 세션이 다음에 유휴 상태가 되면 한 번 알림을 받습니다(선택, v2.1.236)
- `ListAgents`가 세션 자신의 이름을 알려 주고 살아 있는 팀원 세션을 나열합니다(v2.1.239)

### 키보드 사용자 지정
- 사용자 지정 키 바인딩 설정
- 여러 키를 잇는 단축키를 위한 코드(chord) 지원
- 컨텍스트에 따른 키 바인딩 활성화

### 데스크톱 앱 (확장)
- IDE 연동용 커넥터
- launch.json 설정
- 엔터프라이즈 기능과 배포

---

## 10. CLI 사용법 (파일 1개)

명령줄 인터페이스 사용 패턴과 레퍼런스입니다.

| 파일 | 설명 | 내용 |
|------|------|------|
| `README.md` | CLI 문서 | 플래그, 옵션, 사용 패턴 |

**주요 CLI 기능**:
- `claude` - 대화형 세션 시작
- `claude -p "prompt"` - 헤드리스(비대화형) 모드
- `claude --cloud "task"` - 클라우드(웹) 세션 시작
- `claude --model` - 모델 선택 (Opus 5.5, Sonnet 5.5, Opus 5, Sonnet 5, Sonnet 4.6, Opus 4.8, Haiku 5.5, Haiku 4.5)
- `claude --permission-mode` - 권한 모드 설정
- `claude --remote-control` - claude.ai나 Claude 앱에서도 제어할 수 있는 세션 시작

---

## 문서 파일 (13개)

| 파일 | 위치 | 설명 |
|------|------|------|
| `README.md` | `/` | 예제 전체 개요 |
| `INDEX.md` | `/` | 이 전체 색인 |
| `QUICK_REFERENCE.md` | `/` | 빠른 참조 카드 |
| `README.md` | `/01-slash-commands/` | 슬래시 명령 안내 |
| `README.md` | `/02-memory/` | 메모리 안내 |
| `README.md` | `/03-skills/` | 스킬 안내 |
| `README.md` | `/04-subagents/` | 서브에이전트 안내 |
| `README.md` | `/05-mcp/` | MCP 안내 |
| `README.md` | `/06-hooks/` | 훅 안내 |
| `README.md` | `/07-plugins/` | 플러그인 안내 |
| `README.md` | `/08-checkpoints/` | 체크포인트 안내 |
| `README.md` | `/09-advanced-features/` | 고급 기능 안내 |
| `README.md` | `/10-cli/` | CLI 안내 |

---

## 전체 파일 트리

```text
claude-howto/
├── README.md                                    # 전체 개요
├── INDEX.md                                     # 이 파일
├── QUICK_REFERENCE.md                           # 빠른 참조 카드
├── claude_concepts_guide.md                     # 원래 가이드
│
├── 01-slash-commands/                           # 슬래시 명령
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   ├── commit.md
│   ├── setup-ci-cd.md
│   ├── push-all.md
│   ├── unit-test-expand.md
│   ├── doc-refactor.md
│   ├── pr-slash-command.png
│   └── README.md
│
├── 02-memory/                                   # 메모리
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md
│
├── 03-skills/                                   # 스킬
│   ├── code-review-specialist/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-metrics.py
│   │   │   └── compare-complexity.py
│   │   └── templates/
│   │       ├── review-checklist.md
│   │       └── finding-template.md
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   │   ├── email-template.txt
│   │   │   └── social-post-template.txt
│   │   └── tone-examples.md
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   ├── refactor/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-complexity.py
│   │   │   └── detect-smells.py
│   │   ├── references/
│   │   │   ├── code-smells.md
│   │   │   └── refactoring-catalog.md
│   │   └── templates/
│   │       └── refactoring-plan.md
│   ├── claude-md/
│   │   └── SKILL.md
│   ├── blog-draft/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── draft-template.md
│   │       └── outline-template.md
│   └── README.md
│
├── 04-subagents/                                # 서브에이전트
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   ├── debugger.md
│   ├── data-scientist.md
│   ├── clean-code-reviewer.md
│   └── README.md
│
├── 05-mcp/                                      # MCP 프로토콜
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
│
├── 06-hooks/                                    # 훅
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   ├── context-tracker.py
│   ├── context-tracker-tiktoken.py
│   └── README.md
│
├── 07-plugins/                                  # 플러그인
│   ├── pr-review/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── review-pr.md
│   │   │   ├── check-security.md
│   │   │   └── check-tests.md
│   │   ├── agents/
│   │   │   ├── security-reviewer.md
│   │   │   ├── test-checker.md
│   │   │   └── performance-analyzer.md
│   │   ├── mcp/
│   │   │   └── github-config.json
│   │   ├── hooks/
│   │   │   └── pre-review.js
│   │   └── README.md
│   ├── devops-automation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── deploy.md
│   │   │   ├── rollback.md
│   │   │   ├── status.md
│   │   │   └── incident.md
│   │   ├── agents/
│   │   │   ├── deployment-specialist.md
│   │   │   ├── incident-commander.md
│   │   │   └── alert-analyzer.md
│   │   ├── mcp/
│   │   │   └── kubernetes-config.json
│   │   ├── hooks/
│   │   │   ├── pre-deploy.js
│   │   │   └── post-deploy.js
│   │   ├── scripts/
│   │   │   ├── deploy.sh
│   │   │   ├── rollback.sh
│   │   │   └── health-check.sh
│   │   └── README.md
│   ├── documentation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── generate-api-docs.md
│   │   │   ├── generate-readme.md
│   │   │   ├── sync-docs.md
│   │   │   └── validate-docs.md
│   │   ├── agents/
│   │   │   ├── api-documenter.md
│   │   │   ├── code-commentator.md
│   │   │   └── example-generator.md
│   │   ├── mcp/
│   │   │   └── github-docs-config.json
│   │   ├── templates/
│   │   │   ├── api-endpoint.md
│   │   │   ├── function-docs.md
│   │   │   └── adr-template.md
│   │   └── README.md
│   └── README.md
│
├── 08-checkpoints/                              # 체크포인트
│   ├── checkpoint-examples.md
│   └── README.md
│
├── 09-advanced-features/                        # 고급 기능
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
│
└── 10-cli/                                      # CLI 사용법
    └── README.md
```

---

## 사용 사례별 빠른 시작

### 코드 품질과 리뷰
```bash
# 슬래시 명령 설치
cp 01-slash-commands/optimize.md .claude/commands/

# 서브에이전트 설치
cp 04-subagents/code-reviewer.md .claude/agents/

# 스킬 설치
cp -r 03-skills/code-review-specialist ~/.claude/skills/

# 또는 완성된 플러그인 설치
claude --plugin-dir ./07-plugins/pr-review
```

### DevOps와 배포
```bash
# 플러그인 설치 (모든 구성 요소 포함)
claude --plugin-dir ./07-plugins/devops-automation
```

### 문서화
```bash
# 슬래시 명령 설치
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# 서브에이전트 설치
cp 04-subagents/documentation-writer.md .claude/agents/

# 스킬 설치
cp -r 03-skills/doc-generator ~/.claude/skills/

# 또는 완성된 플러그인 설치
claude --plugin-dir ./07-plugins/documentation
```

### 팀 표준
```bash
# 프로젝트 메모리 설정
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 팀 표준에 맞게 편집합니다
```

### 외부 연동
```bash
# 환경 변수 설정
export GITHUB_PERSONAL_ACCESS_TOKEN="your_token"
export DATABASE_URL="postgresql://..."
export SLACK_BOT_TOKEN="your-slack-bot-token"
export SLACK_TEAM_ID="T0000000000"

# MCP 설정 설치 (프로젝트 범위)
cp 05-mcp/multi-mcp.json .mcp.json
```

### 자동화와 검증
```bash
# 훅 설치
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 설정 파일(~/.claude/settings.json)에서 훅 등록
# 06-hooks/README.md 참고
```

### 안전한 실험
```bash
# 체크포인트는 사용자 프롬프트마다 자동으로 만들어집니다
# 되감기: Esc+Esc를 누르거나 /rewind 사용
# 그다음 되감기 메뉴에서 복원할 대상을 고릅니다

# 예제는 08-checkpoints/README.md 참고
```

### 고급 워크플로
```bash
# 고급 기능 설정
# 09-advanced-features/config-examples.json 참고

# 계획 모드 사용
/plan Implement feature X

# 권한 모드 사용
claude --permission-mode plan          # 코드 리뷰용 (읽기 전용)
claude --permission-mode acceptEdits   # 편집 자동 승인
claude --permission-mode auto          # 안전한 작업 자동 승인

# CI/CD에서 헤드리스 모드로 실행
claude -p "Run tests and report results" --permission-mode auto

# 백그라운드 작업 실행
Run tests in background

# 전체 안내는 09-advanced-features/README.md 참고
```

---

## 기능별 구성 매트릭스

| 분류 | 명령 | 에이전트 | MCP | 훅 | 스크립트 | 템플릿 | 문서 | 이미지 | 합계 |
|------|------|----------|-----|----|----------|--------|------|--------|------|
| **01 슬래시 명령** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 메모리** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 스킬** | - | - | - | - | 5 | 7 | 11 | - | **23** |
| **04 서브에이전트** | - | 9 | - | - | - | - | 1 | - | **10** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 훅** | - | - | - | 11 | - | - | 1 | - | **12** |
| **07 플러그인** | 11 | 9 | 3 | 3 | 3 | 3 | 7 | - | **39** |
| **08 체크포인트** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 고급 기능** | - | - | - | - | 1 | 1 | 2 | - | **4** |
| **10 CLI** | - | - | - | - | - | - | 1 | - | **1** |

---

## 학습 경로

### 초급 (1주차)
1. ✅ `README.md` 읽기
2. ✅ 슬래시 명령 1~2개 설치
3. ✅ 프로젝트 메모리 파일 만들기
4. ✅ 기본 명령 써 보기

### 중급 (2~3주차)
1. ✅ GitHub MCP 설정
2. ✅ 서브에이전트 설치
3. ✅ 작업 위임해 보기
4. ✅ 스킬 설치

### 고급 (4주차 이후)
1. ✅ 완성된 플러그인 설치
2. ✅ 사용자 지정 슬래시 명령 만들기
3. ✅ 사용자 지정 서브에이전트 만들기
4. ✅ 사용자 지정 스킬 만들기
5. ✅ 나만의 플러그인 만들기

### 전문가 (5주차 이후)
1. ✅ 자동화용 훅 설정
2. ✅ 체크포인트로 실험하기
3. ✅ 계획 모드 설정
4. ✅ 권한 모드 효과적으로 쓰기
5. ✅ CI/CD용 헤드리스 모드 설정
6. ✅ 세션 관리 익히기

---

## 키워드로 찾기

### 성능
- `01-slash-commands/optimize.md` - 성능 분석
- `04-subagents/code-reviewer.md` - 성능 리뷰
- `03-skills/code-review-specialist/` - 성능 지표
- `07-plugins/pr-review/agents/performance-analyzer.md` - 성능 전문가

### 보안
- `04-subagents/secure-reviewer.md` - 보안 리뷰
- `03-skills/code-review-specialist/` - 보안 분석
- `07-plugins/pr-review/` - 보안 검사

### 테스트
- `04-subagents/test-engineer.md` - 테스트 엔지니어
- `07-plugins/pr-review/commands/check-tests.md` - 테스트 커버리지

### 문서화
- `01-slash-commands/generate-api-docs.md` - API 문서 명령
- `04-subagents/documentation-writer.md` - 문서 작성 에이전트
- `03-skills/doc-generator/` - 문서 생성 스킬
- `07-plugins/documentation/` - 완성된 문서 플러그인

### 배포
- `07-plugins/devops-automation/` - 완성된 DevOps 솔루션

### 자동화
- `06-hooks/` - 이벤트 기반 자동화
- `06-hooks/pre-commit.sh` - pre-commit 자동화
- `06-hooks/format-code.sh` - 자동 포맷
- `09-advanced-features/` - CI/CD용 헤드리스 모드

### 검증
- `06-hooks/security-scan.sh` - 보안 검증
- `06-hooks/validate-prompt.sh` - 프롬프트 검증

### 실험
- `08-checkpoints/` - 되감기로 안전하게 실험
- `08-checkpoints/checkpoint-examples.md` - 실제 사례

### 계획
- `09-advanced-features/planning-mode-examples.md` - 계획 모드 예제
- `09-advanced-features/README.md` - 확장 사고

### 설정
- `09-advanced-features/config-examples.json` - 설정 예제

---

## 참고

- 모든 예제는 바로 쓸 수 있습니다
- 필요에 맞게 고쳐 쓰세요
- 예제는 Claude Code 모범 사례를 따릅니다
- 분류마다 자세한 안내가 담긴 README가 있습니다
- 스크립트에는 알맞은 오류 처리가 들어 있습니다
- 템플릿은 원하는 대로 바꿀 수 있습니다

---

## 기여하기

예제를 더 추가하고 싶다면 다음 구조를 따릅니다.
1. 알맞은 하위 디렉터리를 만듭니다
2. 사용법을 담은 README.md를 넣습니다
3. 이름 규칙을 따릅니다
4. 충분히 테스트합니다
5. 이 색인을 갱신합니다

---

**마지막 업데이트**: 2026년 10월 26일
**Claude Code 버전**: 2.1.310
**출처**:
- https://code.claude.com/docs/en/tools-reference#task-tool-availability
- https://code.claude.com/docs/en/workflows#set-a-size-guideline
- https://code.claude.com/docs/en/memory#agents-md
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/permission-modes
- https://github.com/anthropics/claude-code/releases/tag/v2.1.153
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/workflows#let-claude-decide-with-ultracode
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/plugins/install
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/remote-control
- https://code.claude.com/docs/en/chrome
- https://code.claude.com/docs/en/headless#auto-approve-tools
- https://code.claude.com/docs/en/permission-modes#which-mode-a-non-interactive-run-starts-in
- https://code.claude.com/docs/en/scheduled-tasks
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
**전체 예제**: 파일 100개 이상
**분류**: 기능 10가지
**훅**: 자동화 스크립트 11개
**설정 예제**: 시나리오 10개 이상
**바로 사용 가능**: 모든 예제
