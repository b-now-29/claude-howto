<!-- i18n-source: CATALOG.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 기능 카탈로그

> Claude Code의 모든 기능(명령, 에이전트, 스킬, 플러그인, 훅)을 한눈에 보는 빠른 참조 가이드입니다.

**바로 가기**: [명령](#슬래시-명령) | [권한 모드](#권한-모드) | [서브에이전트](#서브에이전트) | [스킬](#스킬) | [플러그인](#플러그인) | [MCP 서버](#mcp-서버) | [훅](#훅) | [메모리](#메모리-파일) | [새 기능](#새-기능)

> **한국어판 경로:** 설치 명령은 저장소 루트의 원문 폴더를 기준으로 합니다. 번역된 템플릿(`.md`)을 쓰려면 경로 앞에 `ko/`를 붙입니다 (예: `ko/01-slash-commands/optimize.md`). 스크립트, JSON 설정, 플러그인 파일은 `ko/`에 없으므로 원문 경로를 그대로 씁니다.

---

## 요약

| 기능 | 내장 | 예제 | 합계 | 참고 |
|---------|----------|----------|-------|-----------|
| **슬래시 명령** | 60개 이상 | 8 | 68개 이상 | [01-slash-commands/](01-slash-commands/) |
| **서브에이전트** | 6 | 9 | 15 | [04-subagents/](04-subagents/) |
| **스킬** | 번들 20 | 6 | 26 | [03-skills/](03-skills/) |
| **플러그인** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 서버** | - | 4 | 4 | [05-mcp/](05-mcp/) |
| **훅** | 이벤트 33 | 11 | 44 | [06-hooks/](06-hooks/) |
| **메모리** | 유형 7 | 3 | 10 | [02-memory/](02-memory/) |
| **합계** | **126** | **44** | **170** | |

---

## 슬래시 명령

명령은 사용자가 직접 호출해 특정 작업을 실행하는 단축 명령입니다.

### 내장 명령

| 명령 | 설명 | 쓰는 때 |
|---------|-------------|-------------|
| `/help` | 도움말 표시 | 시작할 때, 명령을 익힐 때 |
| `/btw` | 일회성 곁가지 질문 — 주 컨텍스트를 어지럽히지 않음 | 잠깐 다른 질문을 할 때 |
| `/chrome` | Chrome 연동 설정 | 브라우저 자동화 |
| `/clear` | 대화 기록 지우기 | 새로 시작할 때, 컨텍스트를 줄일 때 |
| `/diff` | 대화형 diff 뷰어. 전체 화면 TUI(v2.1.260+)에서는 대화 옆에 diff 패널을 열어 두고, Claude가 파일을 고치거나 명령을 실행할 때마다 새로 고침. 기존 렌더러에서는 프롬프트 자리에 뷰어를 엶 | 변경 사항 검토 |
| `/config` | 설정 보기·편집 | 동작 사용자 맞춤 |
| `/status` | 세션 상태 표시 | 현재 상태 확인 |
| `/agents` | 제거됨: 마법사는 v2.1.198에서 없어졌고, 명령은 v2.1.281에서 `/` 메뉴와 `/help`에서 빠짐 — Claude에게 서브에이전트를 만들거나 관리해 달라고 요청하거나 `.claude/agents/`를 직접 편집 | Claude에게 요청해 서브에이전트 관리 |
| `/skills` | 사용할 수 있는 스킬 목록 | 자동 호출 기능 확인 |
| `/hooks` | 설정된 훅 목록 | 자동화 디버깅 |
| `/insights` | 세션 패턴 분석 | 세션 최적화 |
| `/install-slack-app` | Claude Slack 앱 설치 | Slack 연동 |
| `/keybindings` | 키보드 단축키 사용자 설정 | 키 사용자 맞춤 |
| `/mcp` | MCP 서버 목록 | 외부 연동 확인 |
| `/memory` | 불러온 메모리 파일 보기 | 컨텍스트 로딩 디버깅 |
| `/mobile` | 모바일 QR 코드 생성 | 모바일 접근 |
| `/passes` | 친구에게 Claude Code 무료 1주일 공유 (대상 계정만) | 추천 |
| `/plugin` | 플러그인 관리 | 확장 설치·제거 |
| `/plan` | 계획 모드 시작 | 복잡한 구현 |
| `/proactive` | `/loop`의 별칭 (v2.1.105) | `/loop`와 같음 |
| `/recap` | 세션에 돌아왔을 때 세션 요약 표시 | 자리를 비운 뒤 한 일을 파악할 때 |
| `/rewind` | 체크포인트로 되감기 | 변경 취소, 다른 방법 탐색 |
| `/checkpoint` | `/rewind`의 별칭 | `/rewind`와 같음 |
| `/cost` | `/usage`의 비용 탭을 여는 단축 별칭 (v2.1.118+) | 지출 모니터링 |
| `/context` | 컨텍스트 윈도 사용량 표시 | 대화 길이 관리 |
| `/export` | 대화 내보내기 | 참고용으로 저장 |
| `/usage-credits` | 추가 사용량 한도 설정 (v2.1.144에서 `/extra-usage`에서 이름이 바뀜. 예전 이름도 별칭으로 동작) | 사용량 한도 관리 |
| `/feedback` | 의견이나 버그 보고 제출 | 문제 보고 |
| `/login` | Anthropic 인증 | 기능 사용 |
| `/logout` | 로그아웃 | 계정 전환 |
| `/sandbox` | 샌드박스 모드 토글 | 안전한 명령 실행 |
| `/doctor` | 진단 실행 | 문제 해결 |
| `/reload-plugins` | 설치된 플러그인 다시 불러오기. v2.1.221부터 대부분의 설치는 바로 활성화되므로, 설치 요약에 `Run /reload-plugins to activate.`가 나올 때만 필요. v2.1.260+부터 헤드리스 세션에서도 쓸 수 있어 Claude Code Desktop과 SDK 명령 목록에도 나옴 | 플러그인 관리 |
| `/reload-skills` | 재시작 없이 스킬 디렉터리 다시 검색 (v2.1.152). v2.1.304부터 새로 생기거나 바뀐 스킬만 Claude에게 보내고, 압축(compaction) 때 스킬 목록을 스스로 다시 보냄 | 스킬 관리 |
| `/workflows` | 실행 중이거나 끝난 동적 워크플로 실행 보기 (v2.1.154) | 멀티 에이전트 오케스트레이션 |
| `/release-notes` | 릴리스 노트 표시 | 새 기능 확인 |
| `/remote-control` | Remote Control 켜기 | 원격 접근 |
| `/permissions` | 권한 관리 | 접근 제어 |
| `/auto-mode-setup` | 프로젝트와 최근 세션에서 `autoMode.environment` 항목 초안을 만들고, 검토한 뒤 사용자 설정에 저장. Pro/Max/Team은 v2.1.228+ (네이티브 Windows는 v2.1.233+), Enterprise나 Console API 키는 v2.1.283+, Bedrock·Vertex·Foundry와 텔레메트리를 끈 세션은 v2.1.304+. 클라우드 세션에서는 쓸 수 없음 | auto 모드에 내 환경 알려 주기 |
| `/rename` | 현재 세션 이름 바꾸기 | 세션 정리 |
| `/resume` | 이전 세션 재개 | 작업 이어 가기 |
| `/tui [default\|fullscreen]` | 터미널 UI 렌더러 설정 (`fullscreen` = 깜박임 없는 대체 화면). 인수 없이 `/tui`를 쓰면 현재 렌더러를 출력 | 전체 화면·tmux에서 깜박임 없는 렌더링 |
| `/tasks` | 백그라운드 작업 보기 | 비동기 작업 모니터링 |
| `/schedule [description]` | 일정, API 호출, GitHub 이벤트에 따라 Anthropic 클라우드에서 실행되는 클라우드 예약 작업(예전 routines)을 만들고, 고치고, 나열하고, 실행. 별칭: `/routines` | 내 컴퓨터 없이 돌아가는 반복 자동화 |
| `/copy` | 마지막 응답을 클립보드에 복사 | 출력을 빠르게 공유 |
| `/teleport` (별칭 `/tp`) | 웹의 Claude Code 세션을 이 터미널에서 재개. 선택기가 열림. claude.ai 구독이 필요 | 원격으로 작업 이어 가기 |
| `/desktop` | Claude Desktop 앱 열기 | 데스크톱 인터페이스로 전환 |
| `/theme` | 색 테마 바꾸기. v2.1.118에서 `~/.claude/themes/<name>.json`으로 이름 붙인 사용자 테마가 추가됨 (플러그인도 `themes/` 디렉터리를 담을 수 있음) | 모양 사용자 맞춤 |
| `/usage` | 사용량·비용·통계의 대표 명령 — `/cost`와 `/stats`를 탭 하나로 합침 (v2.1.118). v2.1.149부터 비용 보기가 분류별(스킬, 서브에이전트, 플러그인, MCP 서버별)로 지출을 나눠 보여 줌. **VSCode 확장**(v2.1.174)에서는 `/usage`(Account & usage) 대화 상자에 귀속 분석이 추가됨 — 캐시 미스, 긴 컨텍스트 비용, 서브에이전트, 스킬·에이전트·플러그인·MCP별 사용량을 24시간과 7일 단위로 보여 줌. v2.1.305부터 `/usage`의 권장 사항은 한 줄짜리 수정이며, `f`를 누르면 적용할 수 있는 것을 적용함 (같은 수정이 `/doctor`와 VS Code 대화 상자에도 나옴) | 할당량과 비용 모니터링 |
| `/focus` | 집중 보기 토글 (방해 없는 출력 화면) | 긴 작업 중 시각적 잡음 줄이기 |
| `/fork` | 대화를 새 독립 백그라운드 세션으로 복사 (v2.1.212+) | 여러 방법을 병렬로 탐색 |
| `/subtask` | 대화를 물려받아 결과를 보고하는 포크 서브에이전트 실행 (v2.1.212+) | 하던 자리를 잃지 않고 곁가지 작업 위임 |
| `/stats` | `/usage`의 통계 탭을 여는 단축 별칭 (v2.1.118+) | 세션 지표 검토 |
| `/statusline` | 상태 줄 설정 | 상태 표시 사용자 맞춤 |
| `/stickers` | Claude Code 스티커 주문 | 재미있는 보상 |
| `/fast` | 빠른 출력 모드 토글. **Opus 5.5**(v2.1.280부터 기본), **Opus 5, Opus 4.8**에 적용 | 응답 속도 높이기 |
| `/terminal-setup` | 터미널 연동 설정 | 터미널 기능 설정 |
| `/undo` | `/rewind`의 별칭 (v2.1.108) | `/rewind`와 같음 |
| `/upgrade` | 더 높은 요금제로 바꾸는 업그레이드 페이지 열기 | 요금제 업그레이드 |
| `/team-onboarding` | 이 프로젝트의 Claude Code 사용 기록으로 팀원 적응 가이드 생성 | 새 팀원 온보딩 (v2.1.101) |
| `/code-review ultra` | 현재 변경 사항에 클라우드 멀티 에이전트 코드 리뷰 실행. `/ultrareview`는 별칭으로 남아 있고 `/code-review ultra`를 권장. Pro와 Max에서 무료 3회 뒤에는 사용량 크레딧이 필요 | 병합 전 여러 에이전트로 깊이 있는 리뷰 (v2.1.112) |
| `/fewer-permission-prompts` | 대화 기록을 검사해 자주 쓰는 읽기 전용 도구의 허용 목록을 우선순위대로 제안 | 프로젝트에서 반복되는 권한 요청 줄이기 (v2.1.112) |
| `/skill-doctor` | 불러온 스킬 중 쓰이지 않는 것과 그 컨텍스트 비용 표시. `/plugin` 관리자의 **Stats** 탭에서 열리고, `-p`에서는 텍스트로 출력 | 더 이상 필요 없는 스킬 정리 (v2.1.252+) |

### 사용자 정의 명령 (예제)

| 명령 | 설명 | 쓰는 때 | 범위 | 설치 |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 코드 최적화 분석 | 성능 개선 | 프로젝트 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 풀 리퀘스트 준비 | PR 제출 전 | 프로젝트 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | API 문서 생성 | API 문서화 | 프로젝트 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 맥락을 담은 git 커밋 만들기 | 변경 커밋 | 사용자 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 스테이징, 커밋, 푸시 | 빠른 배포 | 사용자 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 문서 구조 재정리 | 문서 개선 | 프로젝트 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | CI/CD 파이프라인 설정 | 새 프로젝트 | 프로젝트 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 테스트 커버리지 확장 | 테스트 개선 | 프로젝트 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **범위**: `사용자` = 개인 워크플로 (`~/.claude/commands/`), `프로젝트` = 팀 공유 (`.claude/commands/`)

**참고**: [01-slash-commands/](01-slash-commands/) | [공식 문서](https://code.claude.com/docs/en/interactive-mode)

**빠른 설치 (모든 사용자 정의 명령)**:
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 권한 모드

Claude Code는 도구 사용을 어떻게 승인할지 정하는 권한 모드 6가지를 지원합니다.

| 모드 | 설명 | 쓰는 때 |
|------|-------------|-------------|
| `manual` | 도구 호출마다 확인 요청 | 모든 작업을 직접 검토할 때 (v2.1.200부터 Manual로 표시. 설정 값은 여전히 `default`이고 `manual`은 별칭). v2.1.283부터 대화형 터미널과 VS Code 세션의 기본 시작 모드는 auto. v2.1.302부터 `claude -p`와 Agent SDK 실행은 Manual로 시작하므로, 무언가가 승인하지 않으면 승인이 필요한 호출은 거부됨 |
| `acceptEdits` | 파일 편집은 자동 승인, 나머지는 확인 요청 | 믿을 수 있는 편집 워크플로 |
| `plan` | 조사하고 계획을 제안. 승인하기 전에는 소스를 고치지 않음 | 계획과 탐색 |
| `auto` | 모든 작업을 백그라운드 안전 분류기 검사와 함께 실행 | 긴 작업, 확인 요청 피로 줄이기 |
| `bypassPermissions` | 모든 권한 검사 건너뛰기 | CI/CD, 헤드리스 환경 |
| `dontAsk` | 권한이 필요한 도구는 건너뜀 | 비대화형 스크립트 |

> **참고**: `auto` 모드는 대상 요금제, 모델, 제공자가 필요합니다 — [09-advanced-features/](09-advanced-features/README.md#자동-모드)를 참고하세요. `bypassPermissions`는 믿을 수 있는 샌드박스 환경에서만 씁니다.

**참고**: [공식 문서](https://code.claude.com/docs/en/permissions)

---

## 서브에이전트

특정 작업을 위해 격리된 컨텍스트를 가진 전문 AI 어시스턴트입니다.

> **중첩 생성이 기본으로 켜져 있음, 깊이 3 (v2.1.219)**: 서브에이전트는 주 대화 아래 세 단계까지 자기 서브에이전트를 띄울 수 있습니다. 한도를 바꾸려면 `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH`를 설정하고, 중첩을 끄려면 `1`로 설정합니다. (이력: v2.1.172–v2.1.216에서는 바꿀 수 없이 기본으로 5단계까지 중첩됨. v2.1.217에서 깊이 1의 선택 기능이 됨. v2.1.219에서 기본값이 3이 됨.) 어떤 서브에이전트가 띄울 수 있는 서브에이전트를 제한하는 `Agent(agent_type)` 문법은 [04-subagents/README.md](04-subagents/README.md#띄울-수-있는-서브에이전트-제한하기)를 참고하세요.

### 내장 서브에이전트

| 에이전트 | 설명 | 도구 | 모델 | 쓰는 때 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | 여러 단계 작업, 조사 | 모든 도구 | 모델 상속 | 복잡한 조사, 여러 파일 작업 |
| **Plan** | 구현 계획 | Read, Glob, Grep, Bash | 모델 상속 | 구조 설계, 계획 |
| **Explore** | 코드베이스 탐색 | Read, Glob, Grep | 상속 (Opus까지) | 빠른 검색, 코드 이해 |
| **claude** | 더 전문화된 에이전트가 맞지 않는 작업을 맡는 범용 에이전트 | 모든 도구 | 모델 상속 | 전문 에이전트가 없는 작업. 백그라운드 세션을 보낼 때의 기본 에이전트 |
| **statusline-setup** | 상태 줄 설정 | Bash, Read, Write | Sonnet | 상태 줄 표시 설정 |
| **claude-code-guide** | 도움말과 문서 | Read, Glob, Grep | Haiku | 도움 받기, 기능 익히기 |

### 서브에이전트 설정 필드

| 필드 | 타입 | 설명 |
|-------|------|-------------|
| `name` | string | 에이전트 식별자 |
| `description` | string | 에이전트가 하는 일 |
| `model` | string | 모델 재정의 (`sonnet`, `opus`, `haiku`, `fable`, 전체 모델 ID, 또는 `inherit`) |
| `tools` | array | 허용 도구 목록 |
| `effort` | string | 노력 수준 (`low`, `medium`, `high`, `xhigh`, `max`. 모델에 따라 다름) |
| `initialPrompt` | string | 에이전트가 주 세션 에이전트(`--agent`)로 실행될 때 자동으로 제출되는 첫 사용자 턴 |
| `disallowedTools` | array | 이 에이전트에게 명시적으로 금지하는 도구 |

### 사용자 정의 서브에이전트 (예제)

| 에이전트 | 설명 | 쓰는 때 | 범위 | 설치 |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 종합적인 코드 품질 검토 | 코드 리뷰 | 프로젝트 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `clean-code-reviewer` | Clean Code 원칙 리뷰 | 유지보수성 검토 | 프로젝트 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 테스트 전략과 커버리지 | 테스트 계획 | 프로젝트 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 기술 문서 | API 문서, 가이드 | 프로젝트 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 보안 중심 리뷰 | 보안 감사 | 프로젝트 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 기능 전체 구현 | 기능 개발 | 프로젝트 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 근본 원인 분석 | 버그 조사 | 사용자 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 쿼리, 데이터 분석 | 데이터 작업 | 사용자 | `cp 04-subagents/data-scientist.md .claude/agents/` |
| `performance-optimizer` | 프로파일링과 성능 튜닝 | 병목 조사 | 프로젝트 | `cp 04-subagents/performance-optimizer.md .claude/agents/` |

> **범위**: `사용자` = 개인 (`~/.claude/agents/`), `프로젝트` = 팀 공유 (`.claude/agents/`)

**참고**: [04-subagents/](04-subagents/) | [공식 문서](https://code.claude.com/docs/en/sub-agents)

**빠른 설치 (모든 사용자 정의 에이전트)**:
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## 스킬

지침, 스크립트, 템플릿을 담고 자동으로 호출되는 기능입니다.

### 예제 스킬

| 스킬 | 설명 | 자동 호출되는 때 | 범위 | 설치 |
|-------|-------------|-------------------|-------|--------------|
| `code-review-specialist` | 종합적인 코드 리뷰 | "Review this code", "Check quality" | 프로젝트 | `cp -r 03-skills/code-review-specialist .claude/skills/` |
| `brand-voice` | 브랜드 일관성 검사 | 마케팅 문구를 쓸 때 | 프로젝트 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 문서 생성 | "Generate docs", "Document API" | 프로젝트 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 체계적인 코드 리팩터링 (Martin Fowler) | "Refactor this", "Clean up code" | 사용자 | `cp -r 03-skills/refactor ~/.claude/skills/` |
| `claude-md` | CLAUDE.md 파일 만들기·갱신 | "Create CLAUDE.md", "Audit CLAUDE.md" | 프로젝트 | `cp -r 03-skills/claude-md .claude/skills/` |
| `blog-draft` | 아이디어와 자료로 블로그 글 초안 작성 | "Write a blog post", "Draft an article" | 사용자 | `cp -r 03-skills/blog-draft ~/.claude/skills/` |

> **범위**: `사용자` = 개인 (`~/.claude/skills/`), `프로젝트` = 팀 공유 (`.claude/skills/`)

한국어판 스킬(`ko/03-skills/`)에는 `scripts/`가 없으므로, 스크립트를 쓰는 스킬은 원문 폴더의 `scripts/`를 함께 복사합니다.

### 스킬 구조

```text
~/.claude/skills/skill-name/
├── SKILL.md          # 스킬 정의와 지침
├── scripts/          # 보조 스크립트
└── templates/        # 출력 템플릿
```

### 스킬 프런트매터 필드

스킬은 `SKILL.md`의 YAML 프런트매터로 설정합니다.

| 필드 | 타입 | 설명 |
|-------|------|-------------|
| `name` | string | 스킬 표시 이름 |
| `description` | string | 스킬이 하는 일 |
| `when_to_use` | string | `description` 뒤에 붙는 트리거 문구나 요청 예시 |
| `effort` | string | 노력 수준 (`low`, `medium`, `high`, `xhigh`, `max`. 모델에 따라 다름) |
| `shell` | string | 인라인 `` !`command` `` 블록에 쓸 셸 (기본 `bash`, 또는 `powershell`) |

**참고**: [03-skills/](03-skills/) | [공식 문서](https://code.claude.com/docs/en/skills)

**빠른 설치 (모든 스킬)**:
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 번들 스킬

| 스킬 | 설명 | 자동 호출되는 때 |
|-------|-------------|-------------------|
| `/artifact-capabilities` | 게시된 아티팩트가 쓸 수 있는 런타임 기능(커넥터, 파일 다운로드)의 참조 자료를 불러옴 | 그런 기능을 쓰는 아티팩트 페이지를 만들기 전 |
| `/artifact-diagramming` *(v2.1.221+)* | 아티팩트용 다이어그램 지침을 불러옴: 다이어그램이 도움이 되는 때, 무엇을 그릴지, 라이트·다크 테마 모두에서 읽기 쉬운 인라인 SVG | 아티팩트에 다이어그램을 그릴 때 |
| `/batch` | 큰 변경을 독립 단위 5–30개로 나누고, 각 단위를 자기 워크트리의 백그라운드 서브에이전트가 실행 | 큰 병렬 리팩터링 |
| `/claude-api [subcommand]` | Claude API로 앱 만들기. 하위 명령으로 `migrate`, `api-version`(코드를 `2026-10-21` 같은 새 API 버전으로 갱신, v2.1.307+), `managed-agents-secure`(Managed Agents 에이전트 설정을 Anthropic 보안 가이드와 비교. `ant` CLI 필요, v2.1.304+)가 있음 | API 개발 |
| `/claude-in-chrome [task]` | Claude in Chrome으로 브라우저에서 작업 수행 (페이지 테스트, 양식 작성, 콘솔 로그 읽기). Chrome 연동을 켰을 때 쓸 수 있음 | 브라우저 작업 |
| `/dataviz [request]` | 차트, 그래프, 대시보드 디자인 지침: 차트 형태, 역할별 색, 색각 이상 안전성과 대비를 위한 팔레트 검증 | 차트나 대시보드를 만들 때 |
| `/debug [description]` | 세션의 디버그 로깅을 켜고 세션 디버그 로그로 문제 해결 | Claude Code 자체 디버깅 |
| `/design` *(v2.1.265+)* | 여러 아트보드가 있는 디자인 캔버스 만들기 — UI 목업, 화면 흐름, 랜딩 페이지, 포스터 — 코드가 아니라 시각적으로 다듬음. Pro/Max/Team/Enterprise | HTML보다 직접 손으로 다듬고 싶은 화면이나 페이지를 디자인할 때 |
| `/design-sync [hint]` | 저장소의 React 디자인 시스템을 변환해 Claude Design에 올려, 디자인이 실제 컴포넌트를 쓰게 함. Anthropic API 전용. 독립형 Claude Design(claude.ai/design)은 2026년 12월 14일에 종료됨. | 디자인 시스템을 Claude Design과 동기화할 때 |
| `/design-login` | `/design-sync`의 디자인 시스템 접근 권한 부여 (2026년 12월 14일까지 종료되는 독립형 claude.ai/design 사이트에 로그인) | `/design-sync`를 실행하기 전 |
| `/design consent`, `/design revoke` | Claude에게 Claude Design 접근 권한을 주거나 거둠. Claude가 `/design consent` 실행을 요청하면 프롬프트에 직접 입력 | Claude Design 접근 권한을 주거나 없앨 때 |
| `/doctor [prompt-audit [path]]` | 설치, 설정, 확장, `CLAUDE.md` 문제를 진단하고 수정을 제안하는 설정 점검. 별칭: `/checkup` | 설정 문제 해결 |
| `/fewer-permission-prompts` | 대화 기록을 검사해 우선순위가 매겨진 허용 목록을 제안 | 반복되는 권한 요청 줄이기 |
| `/loop` | 일정 간격으로 프롬프트 실행 | 반복 작업 |
| `/run` *(v2.1.145+)* | 이 프로젝트의 앱을 실행해 변경이 동작하는 모습 보기 | 실제 앱에서 변경 확인 |
| `/run-skill-generator` *(v2.1.145+)* | `/run`과 `/verify`가 특정 프로젝트를 다루는 법을 가르침 | `/run`을 처음 쓰는 프로젝트 설정 |
| `/code-review [low\|medium\|high\|xhigh\|max\|ultra] [--fix] [--comment] [pr#\|branch\|path]` | 현재 diff(또는 PR, 브랜치, 경로)에서 정확성 버그를 리뷰. `--fix`는 발견 사항을 적용하고 `--comment`는 PR 인라인 코멘트로 게시. 수준을 주지 않으면 마지막으로 입력한 수준을 다시 씀 (v2.1.223). 낮은 노력 수준은 v2.1.218에서, `high`·`xhigh`·`max`는 v2.1.232에서 **백그라운드 서브에이전트**로 옮겨져, 리뷰 출력이 더 이상 대화를 채우지 않음 | 코드를 쓴 뒤, PR을 병합하기 전 |
| `/slides [brief]` *(v2.1.265+)* | 요약 설명으로 새 프레젠테이션을 Claude Slides 아티팩트로 만듦. 아티팩트와 Slides 템플릿이 필요 | 슬라이드 덱을 만들 때 |
| `/simplify` *(v2.1.154부터 다시 별개)* | 수정까지 적용하는 정리 전용 리뷰 (재사용 / 단순화 / 효율 / 추상화 수준). 버그는 찾지 않음 | 버그 찾기 없이 코드 정리 |
| `/update-config [request]` | 설정 변경(명령 허용, 환경 변수 설정, 훅 추가)을 설명하면 Claude가 해당 `settings.json`을 편집 | JSON을 직접 고치지 않고 설정 바꾸기 |
| `/verify` *(v2.1.145+)* | 앱을 빌드·실행·관찰해 수정이 동작하는지 확인 | 수정을 처음부터 끝까지 검증 |
| `/workflow-authoring` *(v2.1.248+)* | 동적 워크플로 스크립트 작성 참조 자료(스크립트 API, 재개, 품질 패턴)를 불러옴. 동적 워크플로를 켰을 때 쓸 수 있음 | 워크플로 스크립트를 쓰거나 고칠 때 |

---

## 플러그인

명령, 에이전트, MCP 서버, 훅을 묶은 모음입니다.

### 예제 플러그인

| 플러그인 | 설명 | 구성 요소 | 쓰는 때 | 범위 | 설치 |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PR 리뷰 워크플로 | 명령 3개, 에이전트 3개, GitHub MCP | 코드 리뷰 | 프로젝트 | `claude --plugin-dir ./07-plugins/pr-review` |
| `devops-automation` | 배포와 모니터링 | 명령 4개, 에이전트 3개, K8s MCP | DevOps 작업 | 프로젝트 | `/plugin install devops-automation` |
| `documentation` | 문서 생성 도구 모음 | 명령 4개, 에이전트 3개, 템플릿 | 문서화 | 프로젝트 | `/plugin install documentation` |

> **범위**: `프로젝트` = 팀 공유, `사용자` = 개인 워크플로

### 플러그인 구조

```text
my-plugin/
├── .claude-plugin/
│   └── plugin.json   # 매니페스트 파일 (.claude-plugin/ 안의 유일한 파일)
├── commands/         # 슬래시 명령
├── agents/           # 서브에이전트
├── skills/           # 스킬 (<name>/SKILL.md)
├── hooks/
│   └── hooks.json    # 훅 설정
├── .mcp.json         # MCP 설정
├── bin/              # 실행 파일
└── scripts/          # 유틸리티 스크립트
```

**참고**: [07-plugins/](07-plugins/) | [공식 문서](https://code.claude.com/docs/en/plugins)

**플러그인 관리 명령**:
```bash
/plugin list              # 설치된 플러그인 목록
/plugin install <name>    # 플러그인 설치
/plugin uninstall <name>  # 플러그인 제거
claude plugin update <name>   # 플러그인 업데이트 (CLI. /plugin update 슬래시 형식은 본문에는 나오지만 명령 레퍼런스에는 없음)
```

---

## MCP 서버

외부 도구와 API에 접근하는 Model Context Protocol 서버입니다.

### 자주 쓰는 MCP 서버

| 서버 | 설명 | 쓰는 때 | 범위 | 설치 |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR 관리, 이슈, 코드 | GitHub 워크플로 | 로컬 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 쿼리, 데이터 접근 | 데이터베이스 작업 | 로컬 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 고급 파일 작업 | 복잡한 파일 작업 | 로컬 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 팀 소통 | 알림, 업데이트 | 프로젝트 | settings에서 설정 |
| **Google Docs** | 문서 접근 | 문서 편집, 검토 | 프로젝트 | settings에서 설정 |
| **Asana** | 프로젝트 관리 | 작업 추적 | 프로젝트 | settings에서 설정 |
| **Stripe** | 결제 데이터 | 재무 분석 | 프로젝트 | settings에서 설정 |
| **Memory** | 지속되는 메모리 | 세션 간 기억 | 사용자 | settings에서 설정 |
| **Context7** | 라이브러리 문서 | 최신 문서 조회 | 사용자 | settings에서 설정 (서드파티 서버) |

> **범위**: `로컬` = `--scope` 없이 `claude mcp add`를 쓸 때의 기본값 (나만, 이 프로젝트에서. `~/.claude.json`), `프로젝트` = 팀 (`.mcp.json`, `--scope project`), `사용자` = 개인 (`~/.claude.json`, `--scope user`), `내장` = 미리 설치됨

### MCP 설정 예시

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**참고**: [05-mcp/](05-mcp/) | [MCP 프로토콜 문서](https://modelcontextprotocol.io)

**빠른 설치 (GitHub MCP)**:
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## 훅

Claude Code 이벤트가 일어날 때 셸 명령을 실행하는 이벤트 기반 자동화입니다.

### 훅 이벤트

| 이벤트 | 설명 | 실행되는 때 | 사용 사례 |
|-------|-------------|----------------|-----------|
| `SessionStart` | 세션 시작·재개 | 세션 초기화 | 준비 작업 |
| `Setup` | 저장소 설정이나 유지 보수 실행 | `--init-only`, 또는 `-p`와 함께 쓴 `--init`/`--maintenance`에서만 | 도구 준비, 의존성 설치 |
| `InstructionsLoaded` | 지침을 불러옴 | CLAUDE.md나 규칙 파일을 불러올 때 | 사용자 정의 지침 처리 |
| `UserPromptSubmit` | 프롬프트 처리 전 (v2.1.299부터 스킬이나 사용자 정의 명령이 펼쳐지기 전에도) | 사용자가 메시지를 보낼 때 | 입력 검증 |
| `UserPromptExpansion` | 입력한 슬래시 명령(스킬, 사용자 정의 명령, MCP 프롬프트)이 프롬프트로 펼쳐짐 | `UserPromptSubmit` 뒤, 펼쳐진 프롬프트가 Claude에 닿기 전 | `/command` 직접 사용 차단, 스킬별 컨텍스트 추가, 명령 로깅 |
| `PreToolUse` | 도구 실행 전 | 어떤 도구든 실행되기 전 | 검증, 로깅 |
| `PermissionRequest` | 권한 대화 상자 표시 | 민감한 작업 전 | 사용자 정의 승인 흐름 |
| `PermissionDenied` | 사용자가 권한 요청을 거부 | 권한 거부 뒤 | 로깅, 분석, 정책 시행 |
| `PostToolUse` | 도구 성공 뒤 | 어떤 도구든 끝난 뒤 | 포맷, 알림 |
| `PostToolUseFailure` | 도구 실행 실패 | 도구 오류 뒤 | 오류 처리, 로깅 |
| `PostToolBatch` | 도구 사용 묶음이 끝난 뒤 | 도구 묶음이 끝날 때 | 종합 보고, 일괄 검증 |
| `Notification` | 알림 전송 | Claude가 알림을 보낼 때 | 외부 경보 |
| `MessageDisplay` | 어시스턴트 메시지 텍스트 표시 | 메시지가 렌더링되는 동안 | 표시되는 텍스트 변환이나 숨김 |
| `SubagentStart` | 서브에이전트 생성 | 서브에이전트 작업 시작 | 서브에이전트 컨텍스트 초기화 |
| `SubagentStop` | 서브에이전트 종료 | 서브에이전트 작업 완료 | 후속 작업 연결 |
| `Stop` | Claude가 응답을 마침 | 응답 완료 | 정리, 보고 |
| `StopFailure` | API 오류로 턴이 끝남 | API 오류 발생 | 오류 복구, 로깅 |
| `TeammateIdle` | 팀원 에이전트가 유휴 상태 | 에이전트 팀 조율 | 작업 분배 |
| `TaskCompleted` | 작업이 완료로 표시됨 (todo 도구를 켰을 때만 실행 — [훅](06-hooks/README.md#훅-이벤트) 참고) | 작업 완료 | 작업 후처리 |
| `TaskCreated` | TaskCreate로 작업이 만들어짐 (todo 도구를 켰을 때만 실행 — [훅](06-hooks/README.md#훅-이벤트) 참고) | 새 작업 생성 | 작업 추적, 로깅 |
| `ConfigChange` | 설정 갱신 | 설정이 바뀔 때 | 설정 변경에 대응 |
| `CwdChanged` | 작업 디렉터리 변경 | 디렉터리가 바뀔 때 | 디렉터리별 준비 |
| `DirectoryAdded` | 세션 도중 새 작업 디렉터리 등록 | `/add-dir` 또는 SDK `register_repo_root` | 새 디렉터리용 도구 준비 |
| `FileChanged` | 감시 중인 파일 변경 | 파일이 수정될 때 | 파일 모니터링, 다시 빌드 |
| `PreCompact` | 압축 작업 전 | 컨텍스트 압축 | 상태 보존 |
| `PostCompact` | 압축 완료 뒤 | 압축 완료 | 압축 후 작업 |
| `PreModelSwitch` | 요청된 모델 전환이 적용되기 전 | 모델 전환 요청 | 모델 변경 허용 여부 판단이나 거부 |
| `PostModelSwitch` | 세션 모델이 바뀐 뒤 | 모델 전환 완료 | 모델 변경 로깅이나 대응 |
| `WorktreeCreate` | 워크트리 생성 중 | Git 워크트리가 만들어질 때 | 워크트리 환경 준비 |
| `WorktreeRemove` | 워크트리 제거 중 | Git 워크트리가 제거될 때 | 워크트리 자원 정리 |
| `Elicitation` | MCP 서버가 입력 요청 | MCP elicitation | 입력 검증 |
| `ElicitationResult` | 사용자가 elicitation에 응답 | 사용자 응답 | 응답 처리 |
| `SessionEnd` | 세션 종료 | 세션이 끝날 때 | 정리, 상태 저장 |

### 예제 훅

| 훅 | 설명 | 이벤트 | 범위 | 설치 |
|------|-------------|-------|-------|--------------|
| `pre-tool-check.sh` | 위험한 Bash 명령을 막거나 경고 | PreToolUse:Bash | 사용자 | `cp 06-hooks/pre-tool-check.sh ~/.claude/hooks/` |
| `security-scan.sh` | 보안 검사 | PostToolUse:Write | 프로젝트 | `cp 06-hooks/security-scan.sh .claude/hooks/` |
| `format-code.sh` | 자동 포맷 | PostToolUse:Write | 사용자 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.sh` | 프롬프트 검증 | UserPromptSubmit | 프로젝트 | `cp 06-hooks/validate-prompt.sh .claude/hooks/` |
| `context-tracker.py` | 토큰 사용량 추적 | UserPromptSubmit, Stop | 사용자 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `context-tracker-tiktoken.py` | 토큰 사용량 추적 (tiktoken, 정확도 약 90-95%) | UserPromptSubmit, Stop | 사용자 | `cp 06-hooks/context-tracker-tiktoken.py ~/.claude/hooks/` |
| `pre-commit.sh` | 커밋 전 검증 | PreToolUse:Bash | 프로젝트 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 명령 로깅 | PostToolUse:Bash | 사용자 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |
| `dependency-check.sh` | 매니페스트가 바뀌면 취약점 검사 | PostToolUse:Write | 프로젝트 | `cp 06-hooks/dependency-check.sh .claude/hooks/` |
| `notify-team.sh` | git push 때 팀에 알림 | PostToolUse:Bash | 프로젝트 | `cp 06-hooks/notify-team.sh .claude/hooks/` |
| `session-end.sh` | 세션이 끝날 때 진행 상황 기록 | SessionEnd | 사용자 | `cp 06-hooks/session-end.sh ~/.claude/hooks/` |

> **범위**: `프로젝트` = 팀 (`.claude/settings.json`), `사용자` = 개인 (`~/.claude/settings.json`)

### 훅 설정

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "~/.claude/hooks/pre-tool-check.sh" }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          { "type": "command", "command": "~/.claude/hooks/format-code.sh" }
        ]
      }
    ]
  }
}
```

**참고**: [06-hooks/](06-hooks/) | [공식 문서](https://code.claude.com/docs/en/hooks)

**빠른 설치 (모든 훅)**:
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## 메모리 파일

세션을 넘어 자동으로 불러오는 지속 컨텍스트입니다.

### 메모리 유형

| 유형 | 위치 | 범위 | 쓰는 때 |
|------|----------|-------|-------------|
| **관리형 정책** | 조직이 관리하는 정책 | 조직 | 조직 전체 표준 강제 |
| **프로젝트** | `./CLAUDE.md` | 프로젝트 (팀) | 팀 표준, 프로젝트 컨텍스트 |
| **프로젝트 규칙** | `.claude/rules/` | 프로젝트 (팀) | 모듈식 프로젝트 규칙 |
| **사용자** | `~/.claude/CLAUDE.md` | 사용자 (개인) | 개인 선호 |
| **사용자 규칙** | `~/.claude/rules/` | 사용자 (개인) | 모듈식 개인 규칙 |
| **로컬** | `./CLAUDE.local.md` | 로컬 (git 무시) | 컴퓨터별 로컬 재정의 (gitignore 대상). https://code.claude.com/docs/en/memory 에 개발자별 재정의 파일로 문서화되어 있음. |
| **Auto Memory** | 자동 | 세션 | 자동으로 기록되는 통찰과 정정 |

> **범위**: `조직` = 관리자가 관리, `프로젝트` = git으로 팀과 공유, `사용자` = 개인 선호, `로컬` = 커밋하지 않음, `세션` = 자동 관리

**참고**: [02-memory/](02-memory/) | [공식 문서](https://code.claude.com/docs/en/memory)

**빠른 설치**:
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 새 기능

| 기능 | 설명 | 사용법 |
|---------|-------------|------------|
| **/focus** | 방해 없는 출력 화면을 위한 집중 보기 토글 (v2.1.110) | 긴 작업 중 시각적 잡음을 줄이려면 `/focus` 실행 |
| **/proactive** | `/loop`의 별칭 — 반복 작업 동작이 같음 (v2.1.105) | `/proactive`와 `/loop`를 바꿔 가며 씀 |
| **/recap** | 기존 세션에 돌아왔을 때 세션 요약 표시 (v2.1.108) | 자리를 비운 뒤 `/recap`을 실행해 한 일을 파악 |
| **/tui** | 터미널 UI 렌더러 설정: 깜박임 없는 렌더링은 `/tui fullscreen`, 되돌리려면 `/tui default`. 인수 없이 `/tui`는 현재 렌더러를 출력 (v2.1.110) | 전체 화면 터미널이나 tmux에서 `/tui fullscreen` 사용 |
| **/undo** | `/rewind`의 별칭 (v2.1.108) | `/undo`, `/checkpoint`, `/rewind`를 바꿔 가며 씀 |
| **Monitor 도구** | 폴링 대신 백그라운드 명령의 stdout 스트림을 지켜보다가 이벤트에 반응 (v2.1.98+) | [고급 기능](09-advanced-features/)에서 Monitor 도구 사용 |
| **출력 스타일** | 시스템 프롬프트로 Claude의 역할, 어조, 기본 응답 형식을 바꿈. 내장 스타일: Default, Proactive, Explanatory, Learning, Concise | `/config` → Output style, `outputStyle` 설정, 또는 `/output-style` 실행 (v2.1.91에서 제거됐다가 v2.1.269에서 다시 추가됨). [고급 기능](09-advanced-features/README.md#출력-스타일) 참고 |
| **상태 줄** | stdin으로 세션·모델·비용·컨텍스트 JSON을 받는 명령으로 사용자 정의 상태 줄을 렌더링 | `/statusline` 또는 `statusLine` 설정. [고급 기능](09-advanced-features/README.md#상태-줄) 참고 |
| **커뮤니티 마켓플레이스** | Anthropic의 자동 검증을 통과한 서드파티 플러그인. 각각 커밋 SHA에 고정됨 | `/plugin marketplace add anthropics/claude-plugins-community` 다음 `/plugin install <name>@claude-community` |
| **/team-onboarding** | 프로젝트의 Claude Code 설정으로 팀원 적응 가이드를 자동 생성 (v2.1.101) | 프로젝트에서 `/team-onboarding` 실행 |
| **Remote Control** | API로 Claude Code 세션을 원격 제어. **더 이상 리서치 프리뷰가 아님** — `claude remote-control`을 실행하는 컴퓨터는 Claude 앱의 Code 탭에 기기 카드로 나타나므로, 휴대폰에서 그 컴퓨터로 세션을 시작할 수 있음 | 컴퓨터에서 `claude remote-control`을 실행한 뒤 Claude 앱에서 기기 카드를 고름. [고급 기능](09-advanced-features/README.md) 참고 |
| **웹 세션** | 클라우드 세션에서 Claude Code 실행 | `claude --cloud "task"`나 claude.ai/code에서 시작 |
| **데스크톱 앱** | Claude Code 네이티브 데스크톱 앱 | `/desktop`을 쓰거나 Anthropic 웹사이트에서 내려받기 |
| **세션 간 메시지** | 세션끼리 메시지를 주고받음 — 다른 컴퓨터와 클라우드 세션 포함. `ListAgents`로 찾음 (macOS/Linux는 v2.1.224+, 네이티브 Windows는 v2.1.234+) | [고급 기능](09-advanced-features/README.md#세션-간-메시지) 참고. 받는 메시지는 `crossSessionInbound`로 제어 |
| **Self-Hosted Runner** | Claude Code 웹·모바일·데스크톱 세션을 내 컴퓨터나 컨테이너에서 실행 (v2.1.224+, Team/Enterprise) | `claude self-hosted-runner setup`. [CLI](10-cli/README.md) 참고 |
| **`archive` 플러그인 출처** | HTTPS zip에서 플러그인 설치. 필요하면 `sha256`으로 고정 (v2.1.224+) | [플러그인](07-plugins/README.md#archive-출처-v21224) 참고 |
| **`command` 플러그인 출처** | 로컬에 설치된 도구가 플러그인 디렉터리 경로를 출력 (v2.1.229+) | [플러그인](07-plugins/README.md#command-출처-v21229) 참고 |
| **마켓플레이스 소유자 와일드카드** | `"owner/*"`로 GitHub 소유자 아래의 모든 마켓플레이스 저장소를 허용하거나 차단 — `strictKnownMarketplaces`와 `blockedMarketplaces`에서만 (v2.1.223+) | [플러그인](07-plugins/README.md#마켓플레이스-설정) 참고 |
| **샌드박스 자격 증명 가리기** | 샌드박스 명령은 대체 값을 읽고, 프록시가 밖으로 나갈 때 실제 비밀 값으로 바꿔 넣음 (v2.1.221+, Linux/WSL) | [고급 기능](09-advanced-features/README.md#자격-증명-가리기-v21221-v21224) 참고 |
| **Agent Teams** | 관련 작업을 하는 여러 에이전트를 조율 | 서로 협업하고 컨텍스트를 공유하는 팀원 에이전트를 설정 |
| **작업 목록** | 백그라운드 작업 관리와 모니터링 | `/tasks`로 백그라운드 작업을 보고 관리 |
| **프롬프트 제안** | 맥락에 맞는 명령 제안 | 현재 맥락에 따라 제안이 자동으로 나타남 |
| **Git 워크트리** | 병렬 개발을 위한 격리된 git 워크트리 | 워크트리 명령으로 안전하게 병렬 브랜치 작업 |
| **샌드박스** | 안전을 위한 격리된 실행 환경 | `/sandbox`로 토글. 제한된 환경에서 명령을 실행 |
| **MCP OAuth** | MCP 서버의 OAuth 인증 | 안전한 접근을 위해 MCP 서버 설정에 OAuth 자격 증명을 설정 |
| **MCP 도구 검색** | MCP 도구를 동적으로 검색하고 찾기 | 도구 검색으로 연결된 서버 전체에서 쓸 수 있는 MCP 도구를 찾음 |
| **예약 작업 (세션 범위)** | `/loop`와 cron 도구로 세션 안에서 반복 작업 설정. 클라우드 예약 작업(예전 routines)은 `/schedule` 사용 | `/loop 5m /command` 또는 CronCreate 도구 |
| **Chrome 연동** | Claude in Chrome 확장으로 내 Chrome/Edge(보이는 창, 내 로그인)를 조작 | `--chrome` 플래그 또는 `/chrome` 명령 |
| **키보드 사용자 설정** | 코드(chord) 지원을 포함한 키 바인딩 사용자 설정 | `/keybindings`를 쓰거나 `~/.claude/keybindings.json` 편집 |
| **Auto Mode** | 백그라운드 안전 분류기 검사와 함께 완전 자율로 동작 | `Shift+Tab`으로 전환, 또는 `--permission-mode auto` |
| **Channels** | 여러 채널 소통 (Telegram, Slack 등) (리서치 프리뷰) | 채널 플러그인 설정. 2026년 3월 |
| **Voice Dictation** | 프롬프트 음성 입력 | 마이크 아이콘이나 음성 키 바인딩 사용 |
| **agent 훅 유형** | 셸 명령 대신 서브에이전트를 띄우는 훅 | 훅 설정에 `"type": "agent"` 지정 |
| **prompt 훅 유형** | 한 턴 평가를 위해 Claude 모델에 프롬프트를 보내는 훅. 모델은 결정을 JSON으로 돌려줌 | 훅 설정에 `"type": "prompt"` 지정 |
| **MCP Elicitation** | 도구 실행 중에 MCP 서버가 사용자 입력을 요청 | `Elicitation`과 `ElicitationResult` 훅 이벤트로 처리 |
| **플러그인 LSP 지원** | 플러그인을 통한 Language Server Protocol 연동 | 편집기 기능을 위해 `plugin.json`에 LSP 서버 설정 |
| **관리형 드롭인** | 조직이 관리하는 드롭인 설정 (v2.1.83) | 관리자가 관리형 정책으로 설정. 모든 사용자에게 자동 적용 |
| **`claude plugin init`** | `.claude/skills`에 새 플러그인 뼈대를 만듦. 이런 플러그인은 마켓플레이스 없이 자동 로드됨 (v2.1.157) | `claude plugin init <name>` 실행 |
| **서드파티 제공자의 Auto Mode** | Amazon Bedrock, Google Cloud의 Agent Platform, Microsoft Foundry, 로그인한 Claude 앱 게이트웨이 세션에서 기본 제공. 지원 모델은 Claude Sonnet 5 이상, Opus 4.7 이상, Haiku 5.5 (v2.1.293+), Fable 모델 (v2.1.158–v2.1.206에서는 직접 켜야 했고 v2.1.207에서 그 요구가 없어짐 — `CLAUDE_CODE_ENABLE_AUTO_MODE`는 여전히 받지만 효과가 없음) | `Shift+Tab`으로 전환, 또는 `--permission-mode auto` |
| **즉시 열리는 대화 상자 명령** | Claude가 작업하는 동안에도 `/permissions`를 열 수 있음 (규칙 변경은 그 턴의 나머지에 적용). `/add-dir`, `/autocompact`, `/theme`, `/help`, `/config`, `/advisor` 대화 상자도 **전체 화면 TUI에서는** Claude가 응답을 마칠 때까지 기다리지 않고 턴 도중에 열림 (`/bug`는 v2.1.232부터 이미 그랬음) (v2.1.234) | Claude가 작업하는 동안 이 명령 중 하나를 실행. [슬래시 명령](01-slash-commands/README.md) 참고 |
| **`CLAUDE_CODE_PROJECT_DIR_NAME`** | 프로젝트별 대화 기록 디렉터리 이름을 정하는 환경 변수 (v2.1.234) | Claude Code를 실행하기 전에 셸이나 환경에 설정. `CLAUDE_CONFIG_DIR`도 설정하지 않으면 무시됨. [CLI](10-cli/README.md) 참고 |
| **목표 중간 보고 기준** | `/goal`이 활성일 때 백그라운드 작업이 30분 넘게 진척이 없으면, 조용히 계속하지 않고 상태를 보고함. `CLAUDE_CODE_GOAL_CHECKIN_MINUTES`로 조정하고 `0`이면 끔 (v2.1.234) | 활성 `/goal`과 함께 `CLAUDE_CODE_GOAL_CHECKIN_MINUTES=<n>` 설정. [슬래시 명령](01-slash-commands/README.md) 참고 |
| **사용량 한도 자동 계속** | claude.ai 사용량 한도에 막혀 있던 세션은 한도가 초기화되면 Claude Code가 자동으로 이어 감 (v2.1.234) | 기본으로 켜져 있음. `/config` → "Continue automatically at usage limit"에서 끔. [고급 기능](09-advanced-features/) 참고 |
| **`spellcheck` 설정** | `PATH`에 있는 aspell, hunspell, ispell 중 하나로 프롬프트 입력의 철자 오류에 밑줄 표시. 기본은 꺼짐 (v2.1.235) | 검사기를 설치한 뒤 `~/.claude/settings.json`에 `"spellcheck": { "enabled": true }` 설정. 사용자, `--settings`, 관리형 설정에서만 동작하고 프로젝트 설정에서는 무시됨. [고급 기능](09-advanced-features/) 참고 |
| **에이전트 팀 기본 모델** | `/config`의 "Default teammate model" 설정이 없어짐. 이제 생성 호출에서 따로 지정하지 않으면 팀원은 팀 리드의 모델을 물려받음 (v2.1.234) | [서브에이전트 — 에이전트 팀](04-subagents/README.md#에이전트-팀-실험적) 참고 |
| **`/design`** | 디자인 캔버스 — 아티팩트 위에 만들어지는 여러 아트보드의 시각 디자인(UI 목업, 화면 흐름, 랜딩 페이지, 포스터)으로, 코드가 아니라 시각적으로 다듬음. v2.1.265+ 필요. Pro/Max/Team/Enterprise | CLI나 Desktop 앱에서 `/design` 실행. [슬래시 명령](01-slash-commands/README.md) 참고 |
| **`notify_when_idle`** | 같은 컴퓨터의 다른 세션에게 다음에 유휴 상태가 되면 알림을 한 번 보내 달라고 요청하는 세션 간 `SendMessage` 입력 — 선택 기능, 한 번만, 폴링 없음 (v2.1.236). 관련 변경: `ListAgents`가 세션 자신의 이름을 알려 주고 활성 팀원을 나열하며, Windows에서도 세션 간 메시지를 쓸 수 있게 됨 (v2.1.239) | `SendMessage`에 `notify_when_idle`을 넘김. [고급 기능](09-advanced-features/README.md#세션-간-메시지) 참고 |
| **플러그인 매니페스트 필드** | `plugin.json`이 `workflows`, `channels`, `dependencies` (semver), `outputStyles`, `keywords`, `metadata`, `lspServers`, `experimental.themes` / `experimental.monitors`를 받음. CLI에 `claude plugin new`, `remove`/`rm`, `prune`/`autoremove`와 플래그 `--with`, `-f`/`--force`, `--available`, `--push`, `--dry-run`이 추가됨 | [플러그인](07-plugins/README.md) 참고 |
| **Restricted Mode** | 명령이나 코드를 실행하는 내장 도구(Bash, PowerShell 등)와 WebFetch를 `--tools`로 하나씩 지정하지 않는 한 없앰 (`default` 프리셋으로는 안 됨). 사용자·프로젝트·로컬 설정을 무시함 (관리형 설정과 `--settings`는 적용됨). 파일 도구를 작업 디렉터리 안으로 제한. `bypassPermissions`를 거부하고 클라우드 세션 생성을 거부 (v2.1.248+) | `claude --restricted` 또는 `CLAUDE_CODE_RESTRICTED=1`. [CLI](10-cli/README.md) 참고 |
| **`/advisor` 텍스트 형식** | `/advisor`, `/advisor <model>`, `/advisor off`가 대화 상자뿐 아니라 데스크톱 앱, Remote Control, 그 밖의 헤드리스(`-p` / Agent SDK) 세션에서 텍스트 명령으로도 동작 (v2.1.260+) | 헤드리스 세션에서 `/advisor off` 입력. [슬래시 명령](01-slash-commands/README.md) 참고 |

---

## 빠른 참조 매트릭스

### 기능 선택 가이드

| 필요한 것 | 권장 기능 | 이유 |
|------|---------------------|-----|
| 빠른 단축 명령 | 슬래시 명령 | 직접 호출, 즉시 실행 |
| 지속되는 컨텍스트 | 메모리 | 자동 로드 |
| 복잡한 자동화 | 스킬 | 자동 호출 |
| 전문 작업 | 서브에이전트 | 격리된 컨텍스트 |
| 외부 데이터 | MCP 서버 | 실시간 접근 |
| 이벤트 자동화 | 훅 | 이벤트로 실행 |
| 완성된 솔루션 | 플러그인 | 올인원 묶음 |

### 설치 우선순위

| 우선순위 | 기능 | 명령 |
|----------|---------|---------|
| 1. 필수 | 메모리 | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 매일 사용 | 슬래시 명령 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 품질 | 서브에이전트 | `cp 04-subagents/*.md .claude/agents/` |
| 4. 자동화 | 훅 | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 외부 연동 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 고급 | 스킬 | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 완성형 | 플러그인 | `claude --plugin-dir ./07-plugins/pr-review` |

---

## 명령 한 번으로 전체 설치

이 저장소의 모든 예제를 설치합니다.

```bash
# 디렉터리 만들기
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# 모든 기능 설치
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

---

## 추가 자료

- [Claude Code 공식 문서](https://code.claude.com/docs/en/overview)
- [MCP 프로토콜 명세](https://modelcontextprotocol.io)
- [학습 로드맵](LEARNING-ROADMAP.md)
- [메인 README](README.md)

---

**최종 수정일**: 2026년 10월 26일
**Claude Code 버전**: 2.1.310
**출처**:
- https://code.claude.com/docs/en/sub-agents
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/changelog#2-1-172
- https://code.claude.com/docs/en/changelog#2-1-174
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
- https://code.claude.com/docs/en/plugins
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/plugin-marketplaces
- https://code.claude.com/docs/en/discover-plugins
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/plugins-reference
- https://code.claude.com/docs/en/slash-commands
- https://code.claude.com/docs/en/fast-mode
- https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode
- https://code.claude.com/docs/en/mcp
- https://code.claude.com/docs/en/chrome
- https://code.claude.com/docs/en/skills#bundled-skills
- https://code.claude.com/docs/en/plugins/manifest-reference
- https://code.claude.com/docs/en/plugins/cli-reference
- https://code.claude.com/docs/en/env-vars
- https://code.claude.com/docs/en/cross-session-messaging
- https://support.claude.com/en/articles/17440474-migrate-from-standalone-claude-design-to-claude
- https://code.claude.com/docs/en/permission-modes#which-mode-a-session-starts-in
- https://code.claude.com/docs/en/changelog#2-1-305
- https://code.claude.com/docs/en/costs#apply-recommendations-with-the-fix-shortcut
- https://code.claude.com/docs/en/changelog#2-1-304
- https://code.claude.com/docs/en/commands#all-commands
- https://code.claude.com/docs/en/context-window#what-survives-compaction
- https://code.claude.com/docs/en/changelog#2-1-307
- https://code.claude.com/docs/en/skills#work-on-claude-api-projects
- https://code.claude.com/docs/en/routines
- https://code.claude.com/docs/en/auto-mode-config#generate-environment-entries-with-auto-mode-setup
- https://code.claude.com/docs/en/scheduled-tasks
**호환 모델**: Claude Fable 5.1, Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
