<!-- i18n-source: claude_concepts_guide.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Claude 개념 완전 가이드

Claude Code의 기능이 어떻게 동작하고 서로 어떻게 맞물리는지 개념으로 정리한 개요입니다. 슬래시 명령, 서브에이전트, 메모리, MCP, 스킬, 플러그인, 훅 등을 구조 다이어그램, 판단표, 비교표로 설명합니다.

> **이 가이드 사용법**: 이 페이지는 *개념*을 설명합니다. 각 기능이 무엇이고, 안에서 어떻게 동작하며, 언제 쓰는지입니다. **복사해서 쓰는 템플릿과 전체 레퍼런스는 번호가 붙은 모듈**(`01-`부터 `10-`까지)에 있고, 단락마다 해당 모듈로 링크합니다. 여기서 큰 그림을 잡은 뒤, 실제로 설정할 때는 모듈로 가세요.

---

## 목차

1. [슬래시 명령](#슬래시-명령) — [모듈](01-slash-commands/)
2. [서브에이전트](#서브에이전트) — [모듈](04-subagents/)
3. [메모리](#메모리) — [모듈](02-memory/)
4. [MCP 프로토콜](#mcp-프로토콜) — [모듈](05-mcp/)
5. [에이전트 스킬](#에이전트-스킬) — [모듈](03-skills/)
6. [플러그인](#claude-code-플러그인) — [모듈](07-plugins/)
7. [비교와 통합](#비교와-통합)
8. [요약표](#요약표)
9. [빠른 시작 가이드](#빠른-시작-가이드)
10. [훅](#훅) — [모듈](06-hooks/)
11. [체크포인트와 되감기](#체크포인트와-되감기) — [모듈](08-checkpoints/)
12. [고급 기능](#고급-기능) — [모듈](09-advanced-features/)
13. [모델과 추론 노력](#모델과-추론-노력) — [모듈](10-cli/)
14. [자료](#자료)

---

## 슬래시 명령

### 개요

슬래시 명령은 Claude Code가 실행할 수 있도록 Markdown 파일로 저장한, 사용자가 호출하는 단축 명령입니다. 팀이 자주 쓰는 프롬프트와 워크플로를 표준화할 수 있습니다.

### 구조

```mermaid
graph TD
    A["User Input: /command-name"] -->|Triggers| B["Search .claude/commands/"]
    B -->|Finds| C["command-name.md"]
    C -->|Loads| D["Markdown Content"]
    D -->|Executes| E["Claude Processes Prompt"]
    E -->|Returns| F["Result in Context"]
```

### 파일 구조

```mermaid
graph LR
    A["Project Root"] -->|contains| B[".claude/commands/"]
    B -->|contains| C["optimize.md"]
    B -->|contains| D["test.md"]
    B -->|contains| E["docs/"]
    E -->|contains| F["generate-api-docs.md"]
    E -->|contains| G["generate-readme.md"]
```

### 명령 위치별 정리

| 위치 | 범위 | 쓸 수 있는 사람 | 사용 사례 | Git 추적 |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | 프로젝트 전용 | 팀원 | 팀 워크플로, 공유 표준 | ✅ 예 |
| `~/.claude/commands/` | 개인 | 개인 사용자 | 여러 프로젝트에 걸친 개인 단축 명령 | ❌ 아니요 |
| 하위 디렉터리 | 네임스페이스 | 상위 위치를 따름 | 분류별 정리 | ✅ 예 |

### 기능과 능력

| 기능 | 예시 | 지원 |
|---------|---------|-----------|
| 셸 스크립트 실행 | `bash scripts/deploy.sh` | ✅ 예 |
| 파일 참조 | `@path/to/file.js` | ✅ 예 |
| Bash 연동 | `$(git log --oneline)` | ✅ 예 |
| 인수 | `/pr --verbose` | ✅ 예 |
| MCP 명령 | `/mcp__github__list_prs` | ✅ 예 |

### 실전 예제

복사해서 쓰는 명령 템플릿 8개가 [**01-slash-commands/**](01-slash-commands/)에 있습니다 — `/optimize`, `/pr`, `/commit`, `/push-all`, `/generate-api-docs`, `/doc-refactor`, `/setup-ci-cd`, `/unit-test-expand`.

[**01-slash-commands/README.md**](01-slash-commands/README.md)에는 전체 내장 명령 레퍼런스(60개 이상)와 프런트매터 필드 표도 있습니다.

### 명령 생명 주기 다이어그램

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: Types /optimize
    Claude->>FS: Searches .claude/commands/
    FS-->>Claude: Returns optimize.md
    Claude->>Claude: Loads Markdown content
    Claude->>User: Displays prompt context
    User->>Claude: Provides code to analyze
    Claude->>CLI: (May execute scripts)
    CLI-->>Claude: Results
    Claude->>User: Returns analysis
```

### 모범 사례

| ✅ 권장 | ❌ 피할 것 |
|------|---------|
| 동작이 드러나는 명확한 이름 쓰기 | 한 번만 할 작업으로 명령 만들기 |
| description에 트리거 단어 적기 | 명령 안에 복잡한 로직 만들기 |
| 명령마다 작업 하나에 집중하기 | 겹치는 명령 만들기 |
| 프로젝트 명령을 버전 관리하기 | 민감한 정보를 직접 넣기 |
| 하위 디렉터리로 정리하기 | 명령 목록을 길게 늘리기 |
| 단순하고 읽기 쉬운 프롬프트 쓰기 | 줄임말이나 알기 어려운 표현 쓰기 |

---

## 서브에이전트

### 개요

서브에이전트는 격리된 컨텍스트 윈도와 사용자 정의 시스템 프롬프트를 가진 전문 AI 어시스턴트입니다. 관심사를 깔끔하게 나누면서 작업을 위임해 실행할 수 있습니다.

서브에이전트는 자기 서브에이전트를 띄울 수 있으며, **중첩은 기본으로 켜져 있고 깊이 3까지입니다 (v2.1.219)**. 그래서 계층이 아래 그림처럼 주 에이전트 → 서브에이전트 한 단계로 끝나지 않습니다. 한도를 바꾸려면 `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH`를 설정하고, 중첩을 끄려면 `1`로 설정합니다. (이력: v2.1.172–v2.1.216에서는 바꿀 수 없이 기본으로 5단계까지 중첩됨. v2.1.217에서 깊이 1의 선택 기능이 됨. v2.1.219에서 기본값이 3이 됨.)

### 구조 다이어그램

```mermaid
graph TB
    User["👤 User"]
    Main["🎯 Main Agent<br/>(Coordinator)"]
    Reviewer["🔍 Code Reviewer<br/>Subagent"]
    Tester["✅ Test Engineer<br/>Subagent"]
    Docs["📝 Documentation<br/>Subagent"]

    User -->|asks| Main
    Main -->|delegates| Reviewer
    Main -->|delegates| Tester
    Main -->|delegates| Docs
    Reviewer -->|returns result| Main
    Tester -->|returns result| Main
    Docs -->|returns result| Main
    Main -->|synthesizes| User
```

### 서브에이전트 생명 주기

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as Main Agent
    participant CodeReviewer as Code Reviewer<br/>Subagent
    participant Context as Separate<br/>Context Window

    User->>MainAgent: "Build new auth feature"
    MainAgent->>MainAgent: Analyze task
    MainAgent->>CodeReviewer: "Review this code"
    CodeReviewer->>Context: Initialize clean context
    Context->>CodeReviewer: Load reviewer instructions
    CodeReviewer->>CodeReviewer: Perform review
    CodeReviewer-->>MainAgent: Return findings
    MainAgent->>MainAgent: Incorporate results
    MainAgent-->>User: Provide synthesis
```

### 서브에이전트 설정표

| 설정 | 타입 | 목적 | 예시 |
|---------------|------|---------|---------|
| `name` | String | 에이전트 식별자 | `code-reviewer` |
| `description` | String | 목적과 트리거 용어 | `Comprehensive code quality analysis` |
| `tools` | List/String | 허용 도구 | `Read, Grep, Glob, Bash` |
| *(markdown 본문)* | Markdown | 시스템 프롬프트 / 행동 지침 | 사용자 정의 지침 |

### 도구 접근 계층

```mermaid
graph TD
    A["Subagent Configuration"] -->|Option 1| B["Inherit All Tools<br/>from Main Thread"]
    A -->|Option 2| C["Specify Individual Tools"]
    B -->|Includes| B1["File Operations"]
    B -->|Includes| B2["Shell Commands"]
    B -->|Includes| B3["MCP Tools"]
    C -->|Explicit List| C1["read, grep, diff"]
    C -->|Explicit List| C2["Bash(npm:*), Bash(test:*)"]
```

### 실전 예제

바로 쓸 수 있는 서브에이전트 정의 9개가 [**04-subagents/**](04-subagents/)에 있습니다 — `code-reviewer`, `clean-code-reviewer`, `secure-reviewer`, `test-engineer`, `documentation-writer`, `implementation-agent`, `performance-optimizer`, `debugger`, `data-scientist`.

[**04-subagents/README.md**](04-subagents/README.md)에는 전체 프런트매터 레퍼런스, 도구 접근 규칙, 중첩 한도, Agent Teams가 정리되어 있습니다.

### 서브에이전트 컨텍스트 관리

```mermaid
graph TB
    A["Main Agent Context<br/>50,000 tokens"]
    B["Subagent 1 Context<br/>20,000 tokens"]
    C["Subagent 2 Context<br/>20,000 tokens"]
    D["Subagent 3 Context<br/>20,000 tokens"]

    A -->|Clean slate| B
    A -->|Clean slate| C
    A -->|Clean slate| D

    B -->|Results only| A
    C -->|Results only| A
    D -->|Results only| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 서브에이전트를 쓸 때

| 상황 | 서브에이전트 사용 | 이유 |
|----------|--------------|-----|
| 단계가 많은 복잡한 기능 | ✅ 예 | 관심사를 나누고 컨텍스트 오염을 막음 |
| 간단한 코드 리뷰 | ❌ 아니요 | 불필요한 부담 |
| 병렬 작업 실행 | ✅ 예 | 서브에이전트마다 자기 컨텍스트가 있음 |
| 전문 지식이 필요한 작업 | ✅ 예 | 사용자 정의 시스템 프롬프트 |
| 오래 걸리는 분석 | ✅ 예 | 주 컨텍스트가 바닥나는 것을 막음 |
| 단일 작업 | ❌ 아니요 | 쓸데없이 지연이 늘어남 |

### Agent Teams

Agent Teams는 관련 작업을 하는 여러 에이전트를 조율합니다. 한 번에 서브에이전트 하나에게 위임하는 대신, 주 에이전트가 에이전트 무리를 오케스트레이션해서 서로 협업하고 중간 결과를 나누며 공동 목표를 향해 일하게 합니다. 프런트엔드 에이전트, 백엔드 에이전트, 테스트 에이전트가 병렬로 일하는 풀스택 기능 개발 같은 대규모 작업에 유용합니다.

---

## 메모리

### 개요

메모리를 쓰면 Claude가 세션과 대화를 넘어 컨텍스트를 유지합니다. 두 가지 형태가 있습니다. claude.ai의 자동 종합과, Claude Code의 파일 시스템 기반 CLAUDE.md입니다.

### 메모리 구조

```mermaid
graph TB
    A["Claude Session"]
    B["User Input"]
    C["Memory System"]
    D["Memory Storage"]

    B -->|User provides info| C
    C -->|Synthesizes every 24h| D
    D -->|Loads automatically| A
    A -->|Uses context| C
```

### Claude Code의 메모리 계층 (7단계)

Claude Code는 7단계에서 메모리를 불러옵니다. CLAUDE.md 파일은 서로를 덮어쓰지 않고 컨텍스트에 이어 붙으며, 관리형 → 사용자 → 프로젝트 → 로컬 순서로 불러옵니다 (나중에 오는, 더 구체적인 파일을 마지막에 읽음).

```mermaid
graph TD
    A["1. Managed Policy<br/>Enterprise admin policies"] --> B["2. Project Memory<br/>./CLAUDE.md"]
    B --> C["3. Project Rules<br/>.claude/rules/*.md"]
    C --> D["4. User Memory<br/>~/.claude/CLAUDE.md"]
    D --> E["5. User Rules<br/>~/.claude/rules/*.md"]
    E --> F["6. Local Memory<br/>./CLAUDE.local.md"]
    F --> G["7. Auto Memory<br/>Automatically captured preferences"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### 메모리 위치표

| 단계 | 위치 | 범위 | 우선순위 | 공유 | 알맞은 용도 |
|------|----------|-------|----------|--------|----------|
| 1. 관리형 정책 | 엔터프라이즈 관리자 | 조직 | 가장 높음 | 조직 전체 사용자 | 규정 준수, 보안 정책 |
| 2. 프로젝트 | `./CLAUDE.md` | 프로젝트 | 높음 | 팀 (Git) | 팀 표준, 구조 |
| 3. 프로젝트 규칙 | `.claude/rules/*.md` | 프로젝트 | 높음 | 팀 (Git) | 모듈식 프로젝트 관례 |
| 4. 사용자 | `~/.claude/CLAUDE.md` | 개인 | 보통 | 개인 | 개인 선호 |
| 5. 사용자 규칙 | `~/.claude/rules/*.md` | 개인 | 보통 | 개인 | 개인 규칙 모듈 |
| 6. 로컬 | `./CLAUDE.local.md` | 로컬 | 낮음 | 공유하지 않음 | 컴퓨터별 설정 |
| 7. Auto Memory | 자동 | 저장소별 | 가장 낮음 | 개인 | 학습한 선호, 패턴 |

### Auto Memory

Auto Memory는 세션 중에 관찰한 사용자 선호와 패턴을 자동으로 기록합니다. Claude는 상호작용에서 배우고 다음을 기억합니다.

- 코딩 스타일 선호
- 자주 하는 수정
- 프레임워크와 도구 선택
- 소통 방식 선호

Auto Memory는 백그라운드에서 동작하며 따로 설정할 필요가 없습니다.

### 메모리 갱신 생명 주기

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as File System
    participant Memory as CLAUDE.md

    User->>Claude: "Remember: use async/await"
    Claude->>User: "Which memory file?"
    User->>Claude: "Project memory"
    Claude->>Editor: Open ~/.claude/settings.json
    Claude->>Memory: Write to ./CLAUDE.md
    Memory-->>Claude: File saved
    Claude->>Claude: Load updated memory
    Claude-->>User: "Memory saved!"
```

### 실전 예제

복사해서 쓰는 메모리 템플릿이 [**02-memory/**](02-memory/)에 있습니다.

- **`project-CLAUDE.md`** — `./CLAUDE.md`용 팀 전체 프로젝트 표준
- **`personal-CLAUDE.md`** — `~/.claude/CLAUDE.md`용 개인 선호
- **`directory-api-CLAUDE.md`** — 하위 트리용 디렉터리 범위 표준

[**02-memory/README.md**](02-memory/README.md)에서 전체 계층, import 문법, `.claude/rules/`, auto memory, CLAUDE.md를 200줄 아래로 유지하는 법을 다룹니다.

### Claude Web/Desktop의 메모리

#### 메모리 종합 일정

```mermaid
graph LR
    A["Day 1: User<br/>Conversations"] -->|24 hours| B["Day 2: Memory<br/>Synthesis"]
    B -->|Automatic| C["Memory Updated<br/>Summarized"]
    C -->|Loaded in| D["Day 2-N:<br/>New Conversations"]
    D -->|Add to| E["Memory"]
    E -->|24 hours later| F["Memory Refreshed"]
```

### Auto Memory의 내용

Auto memory는 Claude가 세션을 넘어 사용자에 대해 배운 것을 `~/.claude/projects/<project>/memory/`에 저장하고 `MEMORY.md`로 색인합니다. 파일 구성, 불러오기 한도, 켜고 끄는 법은 [**02-memory/README.md**](02-memory/README.md)를 참고하세요.

### 메모리 기능 비교

| 기능 | Claude Web/Desktop | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 자동 종합 | ✅ 24시간마다 | ❌ 직접 |
| 프로젝트 간 | ✅ 공유 | ❌ 프로젝트별 |
| 팀 접근 | ✅ 공유 프로젝트 | ✅ Git 추적 |
| 검색 | ✅ 내장 | ✅ `/memory`로 |
| 편집 | ✅ 채팅에서 | ✅ 파일 직접 편집 |
| 가져오기·내보내기 | ✅ 예 | ✅ 복사·붙여넣기 |
| 지속성 | ✅ 24시간 이상 | ✅ 무기한 |

---

## MCP 프로토콜

### 개요

MCP(Model Context Protocol)는 Claude가 외부 도구, API, 실시간 데이터 소스에 접근하는 표준 방식입니다. 메모리와 달리 MCP는 바뀌는 데이터에 실시간으로 접근합니다.

### MCP 구조

```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["External Service"]

    A -->|Request: list_issues| B
    B -->|Query| C
    C -->|Data| B
    B -->|Response| A

    A -->|Request: create_issue| B
    B -->|Action| C
    C -->|Result| B
    B -->|Response| A
```

### MCP 생태계

```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|File I/O| G["Local Files"]
    C -->|API| H["GitHub Repos"]
    D -->|Query| I["PostgreSQL/MySQL"]
    E -->|Messages| J["Slack Workspace"]
    F -->|Docs| K["Google Drive"]
```

### MCP 설정 과정

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: Type /mcp
    Claude->>Claude: List available MCP servers
    Claude->>User: Show options
    User->>Claude: Select GitHub MCP
    Claude->>Config: Update configuration
    Config->>Claude: Activate connection
    Claude->>Service: Test connection
    Service-->>Claude: Authentication successful
    Claude->>User: ✅ MCP connected!
```

### 사용할 수 있는 MCP 서버

| MCP 서버 | 목적 | 주요 도구 | 인증 | 실시간 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 파일 작업 | read, write, delete | OS 권한 | ✅ 예 |
| **GitHub** | 저장소 관리 | list_prs, create_issue, push | OAuth | ✅ 예 |
| **Slack** | 팀 소통 | send_message, list_channels | 토큰 | ✅ 예 |
| **Database** | SQL 쿼리 | query, insert, update | 자격 증명 | ✅ 예 |
| **Google Docs** | 문서 접근 | read, write, share | OAuth | ✅ 예 |
| **Asana** | 프로젝트 관리 | create_task, update_status | API 키 | ✅ 예 |
| **Stripe** | 결제 데이터 | list_charges, create_invoice | API 키 | ✅ 예 |
| **Memory** | 지속되는 메모리 | store, retrieve, delete | 로컬 | ❌ 아니요 |

### 실전 예제

바로 쓸 수 있는 MCP 서버 설정이 [**05-mcp/**](05-mcp/)에 있습니다: `github-mcp.json`, `database-mcp.json`, `filesystem-mcp.json`, `multi-mcp.json`(서버 4개를 한 파일에).

[**05-mcp/README.md**](05-mcp/README.md)에서 `claude mcp add` 전체 문법, 전송 방식, 범위, OAuth, 엔터프라이즈 허용 목록을 다룹니다.

### MCP와 메모리: 판단 매트릭스

```mermaid
graph TD
    A["Need external data?"]
    A -->|No| B["Use Memory"]
    A -->|Yes| C["Does it change frequently?"]
    C -->|No/Rarely| B
    C -->|Yes/Often| D["Use MCP"]

    B -->|Stores| E["Preferences<br/>Context<br/>History"]
    D -->|Accesses| F["Live APIs<br/>Databases<br/>Services"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### 요청·응답 패턴

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as Database

    App->>MCP: Request: "SELECT * FROM users WHERE id=1"
    MCP->>DB: Execute query
    DB-->>MCP: Result set
    MCP-->>App: Return parsed data
    App->>App: Process result
    App->>App: Continue task

    Note over MCP,DB: Real-time access<br/>No caching
```

---

## 에이전트 스킬

### 개요

에이전트 스킬은 지침, 스크립트, 자료를 담은 폴더로 묶인, 모델이 호출하는 재사용 기능입니다. Claude가 관련 스킬을 자동으로 찾아 씁니다.

### 스킬 구조

```mermaid
graph TB
    A["Skill Directory"]
    B["SKILL.md"]
    C["YAML Metadata"]
    D["Instructions"]
    E["Scripts"]
    F["Templates"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```

### 스킬 불러오기 과정

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as System
    participant Skill as Skill

    User->>Claude: "Create Excel report"
    Claude->>System: Scan available skills
    System->>System: Load skill metadata
    Claude->>Claude: Match user request to skills
    Claude->>Skill: Load xlsx skill SKILL.md
    Skill-->>Claude: Return instructions + tools
    Claude->>Claude: Execute skill
    Claude->>User: Generate Excel file
```

### 스킬 유형과 위치

| 유형 | 위치 | 범위 | 공유 | 동기화 | 알맞은 용도 |
|------|----------|-------|--------|------|----------|
| 사전 제작 | 내장 | 전역 | 모든 사용자 | 자동 | 문서 만들기 |
| 개인 | `~/.claude/skills/` | 개인 | 아니요 | 직접 | 개인 자동화 |
| 프로젝트 | `.claude/skills/` | 팀 | 예 | Git | 팀 표준 |
| 플러그인 | 플러그인 설치로 | 경우에 따라 | 경우에 따라 | 자동 | 통합 기능 |

### 사전 제작 스킬

```mermaid
graph TB
    A["Pre-built Skills"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["Create presentations"]
    B --> B2["Edit slides"]
    C --> C1["Create spreadsheets"]
    C --> C2["Analyze data"]
    D --> D1["Create documents"]
    D --> D2["Format text"]
    E --> E1["Generate PDFs"]
    E --> E2["Fill forms"]
```

### 번들 스킬

Claude Code에는 설치 없이 바로 쓸 수 있는 번들 스킬이 들어 있습니다. 예를 들면 다음과 같습니다.

| 스킬 | 명령 | 목적 |
|-------|---------|---------|
| **Batch** | `/batch` | 여러 파일이나 항목에 걸쳐 작업 실행 |
| **Claude API** | `/claude-api` | 프로젝트 언어에 맞는 Claude API와 Managed Agents 참조 자료를 불러옴. 코드가 `anthropic`이나 `@anthropic-ai/sdk`를 import하면 자동으로 켜짐. 하위 명령으로 `api-version`(v2.1.307)과 `managed-agents-secure`(v2.1.304)가 있음 |
| **Code Review** | `/code-review` | 고른 노력 수준으로 현재 diff의 정확성 버그를 리뷰. v2.1.154에서 다시 분리된 `/simplify`(품질·재사용 정리)와는 별개 스킬. v2.1.246부터 모든 환경에서 Claude가 스스로 시작할 수도 있음 |
| **Simplify** | `/simplify` | 품질, 재사용, 정리 리뷰 — v2.1.154부터 다시 `/code-review`와 별개 |
| **Debug** | `/debug` | 현재 세션의 디버그 로깅을 켜고 세션 디버그 로그를 읽어 문제 해결 |
| **Fewer Permission Prompts** | `/fewer-permission-prompts` | 대화 기록을 검사해 자주 쓰는 읽기 전용 도구의 허용 목록을 우선순위대로 제안 |
| **Loop** | `/loop` | 타이머로 반복 작업 예약 |
| **Run** | `/run` | 프로젝트 앱을 실행하고 조작해 변경을 확인 (v2.1.145+) |
| **Run Skill Generator** | `/run-skill-generator` | `/run`과 `/verify`에 앱을 빌드·실행·조작하는 법을 가르치는 프로젝트별 스킬 작성 (v2.1.145+) |
| **Verify** | `/verify` | 변경이 실제로 동작하는지 확인 (v2.1.145+). v2.1.215부터 직접 호출할 때만 실행 |

이 번들 스킬은 늘 쓸 수 있으며 설치나 설정이 필요 없습니다.

### 실전 예제

스크립트, 템플릿, 참조 파일까지 갖춘 완성된 스킬 6개가 [**03-skills/**](03-skills/)에 있습니다.

- **`code-review-specialist/`** — 리뷰 체크리스트, 발견 사항 템플릿, Python 지표 스크립트 2개
- **`refactor/`** — 코드 스멜 카탈로그, 리팩터링 카탈로그, 계획 템플릿, 분석 스크립트 2개
- **`doc-generator/`** — 소스에서 API 문서 생성
- **`blog-draft/`** — 버전이 붙는 출력 관례를 따르는 개요·초안 템플릿
- **`brand-voice/`** — 어조 규칙과 메시지 템플릿 (`user-invocable: false` 예시)
- **`claude-md/`** — CLAUDE.md 파일 만들기, 갱신, 감사

전체 프런트매터 레퍼런스와 점진적 공개(progressive disclosure) 모델은 [**03-skills/README.md**](03-skills/README.md)를 참고하세요. 한국어판 스킬에는 `scripts/`가 없으므로 스크립트는 원문 폴더에서 복사합니다.

### 스킬 탐색과 호출

```mermaid
graph TD
    A["User Request"] --> B["Claude Analyzes"]
    B -->|Scans| C["Available Skills"]
    C -->|Metadata check| D["Skill Description Match?"]
    D -->|Yes| E["Load SKILL.md"]
    D -->|No| F["Try next skill"]
    F -->|More skills?| D
    F -->|No more| G["Use general knowledge"]
    E --> H["Extract Instructions"]
    H --> I["Execute Skill"]
    I --> J["Return Results"]
```

### 스킬과 다른 기능 비교

```mermaid
graph TB
    A["Extending Claude"]
    B["Slash Commands"]
    C["Subagents"]
    D["Memory"]
    E["MCP"]
    F["Skills"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|User-invoked| G["Quick shortcuts"]
    C -->|Auto-delegated| H["Isolated contexts"]
    D -->|Persistent| I["Cross-session context"]
    E -->|Real-time| J["External data access"]
    F -->|Auto-invoked| K["Autonomous execution"]
```

---

## Claude Code 플러그인

### 개요

Claude Code 플러그인은 사용자 정의 요소(슬래시 명령, 서브에이전트, MCP 서버, 훅)를 묶은 모음으로, 명령 한 번으로 설치됩니다. 가장 높은 수준의 확장 방식으로, 여러 기능을 하나로 엮어 공유할 수 있는 패키지로 만듭니다.

### 구조

```mermaid
graph TB
    A["Plugin"]
    B["Slash Commands"]
    C["Subagents"]
    D["MCP Servers"]
    E["Hooks"]
    F["Configuration"]

    A -->|bundles| B
    A -->|bundles| C
    A -->|bundles| D
    A -->|bundles| E
    A -->|bundles| F
```

### 플러그인 불러오기 과정

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as Plugin Marketplace
    participant Install as Installation
    participant SlashCmds as Slash Commands
    participant Subagents
    participant MCPServers as MCP Servers
    participant Hooks
    participant Tools as Configured Tools

    User->>Claude: claude --plugin-dir ./07-plugins/pr-review
    Claude->>Plugin: Download plugin manifest
    Plugin-->>Claude: Return plugin definition
    Claude->>Install: Extract components
    Install->>SlashCmds: Configure
    Install->>Subagents: Configure
    Install->>MCPServers: Configure
    Install->>Hooks: Configure
    SlashCmds-->>Tools: Ready to use
    Subagents-->>Tools: Ready to use
    MCPServers-->>Tools: Ready to use
    Hooks-->>Tools: Ready to use
    Tools-->>Claude: Plugin installed ✅
```

### 플러그인 유형과 배포

| 유형 | 범위 | 공유 | 주체 | 예시 |
|------|-------|--------|-----------|----------|
| 공식 | 전역 | 모든 사용자 | Anthropic | PR Review, Security Guidance |
| 커뮤니티 | 공개 | 모든 사용자 | 커뮤니티 | DevOps, Data Science |
| 조직 | 내부 | 팀원 | 회사 | 내부 표준, 도구 |
| 개인 | 개인 | 사용자 한 명 | 개발자 | 사용자 정의 워크플로 |

### 플러그인 정의 구조

매니페스트는 `.claude-plugin/plugin.json`에 있는 JSON입니다. `name`만 필수이고, 알 수 없는 최상위 키는 제거됩니다.

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "What this plugin does"
}
```

### 플러그인 구조

```text
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   └── skill-name/
│       └── SKILL.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── bin/
│   └── my-tool
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```

### 실전 예제

설치해서 쓸 수 있는 완성된 플러그인 3개가 [**07-plugins/**](07-plugins/)에 있습니다.

- **`pr-review/`** — 리뷰 명령, 전문 에이전트 3개, GitHub MCP, 리뷰 전 훅
- **`documentation/`** — 문서 생성 명령, 에이전트 3개, 재사용 템플릿
- **`devops-automation/`** — deploy/rollback/status/incident 명령, 에이전트 3개, Kubernetes MCP, 셸 스크립트

각 플러그인에 `.claude-plugin/plugin.json` 매니페스트와 디렉터리 구성이 들어 있습니다. 플러그인 파일은 `ko/`에 없으므로 설치할 때는 원문 폴더를 지정합니다.

### 플러그인 마켓플레이스

```mermaid
graph TB
    A["Plugin Marketplace"]
    B["Official<br/>Anthropic"]
    C["Community<br/>Marketplace"]
    D["Enterprise<br/>Registry"]

    A --> B
    A --> C
    A --> D

    B -->|Categories| B1["Development"]
    B -->|Categories| B2["DevOps"]
    B -->|Categories| B3["Documentation"]

    C -->|Search| C1["DevOps Automation"]
    C -->|Search| C2["Mobile Dev"]
    C -->|Search| C3["Data Science"]

    D -->|Internal| D1["Company Standards"]
    D -->|Internal| D2["Legacy Systems"]
    D -->|Internal| D3["Compliance"]
```

### 플러그인 설치와 생명 주기

```mermaid
graph LR
    A["Discover"] -->|Browse| B["Marketplace"]
    B -->|Select| C["Plugin Page"]
    C -->|View| D["Components"]
    D -->|Install| E["/plugin install"]
    E -->|Extract| F["Configure"]
    F -->|Activate| G["Use"]
    G -->|Check| H["Update"]
    H -->|Available| G
    G -->|Done| I["Disable"]
    I -->|Later| J["Enable"]
    J -->|Back| G
```

### 플러그인 기능 비교

| 기능 | 슬래시 명령 | 스킬 | 서브에이전트 | 플러그인 |
|---------|---------------|-------|----------|--------|
| **설치** | 직접 복사 | 직접 복사 | 직접 설정 | 명령 한 번 |
| **설정 시간** | 5분 | 10분 | 15분 | 2분 |
| **묶음** | 파일 하나 | 파일 하나 | 파일 하나 | 여러 개 |
| **버전 관리** | 직접 | 직접 | 직접 | 자동 |
| **팀 공유** | 파일 복사 | 파일 복사 | 파일 복사 | 설치 ID |
| **업데이트** | 직접 | 직접 | 직접 | 자동으로 받을 수 있음 |
| **의존성** | 없음 | 없음 | 없음 | 포함 가능 |
| **마켓플레이스** | 아니요 | 아니요 | 아니요 | 예 |
| **배포** | 저장소 | 저장소 | 저장소 | 마켓플레이스 |

### 플러그인 사용 사례

| 사용 사례 | 권장 | 이유 |
|----------|-----------------|-----|
| **팀 온보딩** | ✅ 플러그인 사용 | 즉시 설정, 모든 설정 포함 |
| **프레임워크 설정** | ✅ 플러그인 사용 | 프레임워크별 명령을 묶음 |
| **엔터프라이즈 표준** | ✅ 플러그인 사용 | 중앙 배포, 버전 관리 |
| **빠른 작업 자동화** | ❌ 명령 사용 | 지나치게 복잡함 |
| **단일 분야 전문 지식** | ❌ 스킬 사용 | 너무 무거움, 스킬로 충분 |
| **전문 분석** | ❌ 서브에이전트 사용 | 직접 만들거나 스킬 사용 |
| **실시간 데이터 접근** | ❌ MCP 사용 | 따로 두고 묶지 않음 |

### 플러그인을 만들 때

```mermaid
graph TD
    A["Should I create a plugin?"]
    A -->|Need multiple components| B{"Multiple commands<br/>or subagents<br/>or MCPs?"}
    B -->|Yes| C["✅ Create Plugin"]
    B -->|No| D["Use Individual Feature"]
    A -->|Team workflow| E{"Share with<br/>team?"}
    E -->|Yes| C
    E -->|No| F["Keep as Local Setup"]
    A -->|Complex setup| G{"Needs auto<br/>configuration?"}
    G -->|Yes| C
    G -->|No| D
```

### 플러그인 게시

**게시 단계:**

1. 모든 구성 요소로 플러그인 구조를 만듭니다
2. `.claude-plugin/plugin.json` 매니페스트를 씁니다
3. 문서를 담은 `README.md`를 만듭니다
4. `claude --plugin-dir ./my-plugin`으로 로컬에서 테스트합니다
5. 플러그인 마켓플레이스에 제출합니다
6. 검토와 승인을 받습니다
7. 마켓플레이스에 게시됩니다
8. 사용자가 명령 한 번으로 설치할 수 있습니다

### 플러그인 README 예시

[**07-plugins/**](07-plugins/)의 완성된 플러그인 3개를 참고하세요 — `pr-review/`, `documentation/`, `devops-automation/`에는 각각 그대로 복사해 쓸 수 있는 `README.md`, 매니페스트, 명령, 에이전트, MCP 설정이 들어 있습니다.

### 플러그인과 직접 설정 비교

**직접 설정 (2시간 이상):**
- 슬래시 명령을 하나씩 설치
- 서브에이전트를 하나씩 만들기
- MCP를 따로 설정
- 훅을 직접 설정
- 모든 것을 문서화
- 팀과 공유 (모두 올바르게 설정하길 바람)

**플러그인으로 (2분):**
```bash
claude --plugin-dir ./07-plugins/pr-review
# ✅ 모든 것이 설치되고 설정됨
# ✅ 바로 사용 가능
# ✅ 팀이 똑같은 설정을 재현 가능
```

---

## 비교와 통합

### 기능 비교 매트릭스

| 기능 | 호출 | 지속성 | 범위 | 사용 사례 |
|---------|-----------|------------|-------|----------|
| **슬래시 명령** | 직접 (`/cmd`) | 세션 동안만 | 명령 하나 | 빠른 단축 명령 |
| **서브에이전트** | 자동 위임 | 격리된 컨텍스트 | 전문 작업 | 작업 분배 |
| **메모리** | 자동 로드 | 세션 간 | 사용자·팀 컨텍스트 | 장기 학습 |
| **MCP 프로토콜** | 자동 조회 | 실시간 외부 데이터 | 실시간 데이터 접근 | 동적 정보 |
| **스킬** | 자동 호출 | 파일 시스템 기반 | 재사용 전문 지식 | 자동화된 워크플로 |

### 상호작용 흐름

```mermaid
graph LR
    A["Session Start"] -->|Load| B["Memory (CLAUDE.md)"]
    B -->|Discover| C["Available Skills"]
    C -->|Register| D["Slash Commands"]
    D -->|Connect| E["MCP Servers"]
    E -->|Ready| F["User Interaction"]

    F -->|Type /cmd| G["Slash Command"]
    F -->|Request| H["Skill Auto-Invoke"]
    F -->|Query| I["MCP Data"]
    F -->|Complex task| J["Delegate to Subagent"]

    G -->|Uses| B
    H -->|Uses| B
    I -->|Uses| B
    J -->|Uses| B
```

### 실전 통합 예시: 고객 지원 자동화

#### 구조

```mermaid
graph TB
    User["Customer Email"] -->|Receives| Router["Support Router"]

    Router -->|Analyze| Memory["Memory<br/>Customer history"]
    Router -->|Lookup| MCP1["MCP: Customer DB<br/>Previous tickets"]
    Router -->|Check| MCP2["MCP: Slack<br/>Team status"]

    Router -->|Route Complex| Sub1["Subagent: Tech Support<br/>Context: Technical issues"]
    Router -->|Route Simple| Sub2["Subagent: Billing<br/>Context: Payment issues"]
    Router -->|Route Urgent| Sub3["Subagent: Escalation<br/>Context: Priority handling"]

    Sub1 -->|Format| Skill1["Skill: Response Generator<br/>Brand voice maintained"]
    Sub2 -->|Format| Skill2["Skill: Response Generator"]
    Sub3 -->|Format| Skill3["Skill: Response Generator"]

    Skill1 -->|Generate| Output["Formatted Response"]
    Skill2 -->|Generate| Output
    Skill3 -->|Generate| Output

    Output -->|Post| MCP3["MCP: Slack<br/>Notify team"]
    Output -->|Send| Reply["Customer Reply"]
```

#### 요청 흐름

```markdown
## 고객 지원 요청 흐름

### 1. 이메일 수신
"I'm getting error 500 when trying to upload files. This is blocking my workflow!"

### 2. 메모리 조회
- 지원 표준이 담긴 CLAUDE.md를 불러옴
- 고객 이력 확인: VIP 고객, 이번 달 세 번째 사고

### 3. MCP 조회
- GitHub MCP: 열린 이슈 나열 (관련 버그 보고를 찾음)
- Database MCP: 시스템 상태 확인 (보고된 장애 없음)
- Slack MCP: 엔지니어링 팀이 알고 있는지 확인

### 4. 스킬 탐지와 불러오기
- 요청이 "Technical Support" 스킬과 맞음
- 스킬에서 지원 응답 템플릿을 불러옴

### 5. 서브에이전트 위임
- Tech Support 서브에이전트로 보냄
- 컨텍스트 제공: 고객 이력, 오류 내용, 알려진 문제
- 서브에이전트는 read, bash, grep 도구에 모두 접근 가능

### 6. 서브에이전트 처리
Tech Support 서브에이전트:
- 코드베이스에서 파일 업로드의 500 오류를 찾음
- 커밋 8f4a2c의 최근 변경을 찾음
- 임시 해결 방법 문서를 만듦

### 7. 스킬 실행
Response Generator 스킬:
- Brand Voice 지침을 따름
- 공감하는 어조로 응답을 작성
- 임시 해결 단계를 넣음
- 관련 문서로 링크

### 8. MCP 출력
- #support Slack 채널에 업데이트 게시
- 엔지니어링 팀 태그
- Jira MCP로 티켓 갱신

### 9. 응답
고객이 받는 내용:
- 공감하는 확인 인사
- 원인 설명
- 즉시 쓸 수 있는 임시 해결 방법
- 영구 수정 일정
- 관련 이슈 링크
```

### 전체 기능 오케스트레이션

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as Memory<br/>CLAUDE.md
    participant MCP as MCP Servers
    participant Skills as Skills
    participant SubAgent as Subagents

    User->>Claude: Request: "Build auth system"
    Claude->>Memory: Load project standards
    Memory-->>Claude: Auth standards, team practices
    Claude->>MCP: Query GitHub for similar implementations
    MCP-->>Claude: Code examples, best practices
    Claude->>Skills: Detect matching Skills
    Skills-->>Claude: Security Review Skill + Testing Skill
    Claude->>SubAgent: Delegate implementation
    SubAgent->>SubAgent: Build feature
    Claude->>Skills: Apply Security Review Skill
    Skills-->>Claude: Security checklist results
    Claude->>SubAgent: Delegate testing
    SubAgent-->>Claude: Test results
    Claude->>User: Complete system delivered
```

### 기능별로 쓰는 때

```mermaid
graph TD
    A["New Task"] --> B{Type of Task?}

    B -->|Repeated workflow| C["Slash Command"]
    B -->|Need real-time data| D["MCP Protocol"]
    B -->|Remember for next time| E["Memory"]
    B -->|Specialized subtask| F["Subagent"]
    B -->|Domain-specific work| G["Skill"]

    C --> C1["✅ Team shortcut"]
    D --> D1["✅ Live API access"]
    E --> E1["✅ Persistent context"]
    F --> F1["✅ Parallel execution"]
    G --> G1["✅ Auto-invoked expertise"]
```

### 선택 판단 트리

```mermaid
graph TD
    Start["Need to extend Claude?"]

    Start -->|Quick repeated task| A{"Manual or Auto?"}
    A -->|Manual| B["Slash Command"]
    A -->|Auto| C["Skill"]

    Start -->|Need external data| D{"Real-time?"}
    D -->|Yes| E["MCP Protocol"]
    D -->|No/Cross-session| F["Memory"]

    Start -->|Complex project| G{"Multiple roles?"}
    G -->|Yes| H["Subagents"]
    G -->|No| I["Skills + Memory"]

    Start -->|Long-term context| J["Memory"]
    Start -->|Team workflow| K["Slash Command +<br/>Memory"]
    Start -->|Full automation| L["Skills +<br/>Subagents +<br/>MCP"]
```

---

## 요약표

| 측면 | 슬래시 명령 | 서브에이전트 | 메모리 | MCP | 스킬 | 플러그인 |
|--------|---|---|---|---|---|---|
| **설정 난이도** | 쉬움 | 보통 | 쉬움 | 보통 | 보통 | 쉬움 |
| **학습 곡선** | 낮음 | 보통 | 낮음 | 보통 | 보통 | 낮음 |
| **팀 이점** | 높음 | 높음 | 보통 | 높음 | 높음 | 매우 높음 |
| **자동화 수준** | 낮음 | 높음 | 보통 | 높음 | 높음 | 매우 높음 |
| **컨텍스트 관리** | 세션 하나 | 격리 | 지속 | 실시간 | 지속 | 모든 기능 |
| **유지보수 부담** | 낮음 | 보통 | 낮음 | 보통 | 보통 | 낮음 |
| **확장성** | 좋음 | 매우 좋음 | 좋음 | 매우 좋음 | 매우 좋음 | 매우 좋음 |
| **공유 용이성** | 보통 | 보통 | 좋음 | 좋음 | 좋음 | 매우 좋음 |
| **버전 관리** | 직접 | 직접 | 직접 | 직접 | 직접 | 자동 |
| **설치** | 직접 복사 | 직접 설정 | 해당 없음 | 직접 설정 | 직접 복사 | 명령 한 번 |

---

## 빠른 시작 가이드

### 1주차: 단순하게 시작하기
- 자주 하는 작업용 슬래시 명령 2-3개 만들기
- 설정에서 메모리 켜기
- CLAUDE.md에 팀 표준 적기

### 2주차: 실시간 접근 더하기
- MCP 1개 설정 (GitHub 또는 Database)
- `/mcp`로 설정
- 워크플로에서 실시간 데이터 조회

### 3주차: 작업 나누기
- 특정 역할을 맡을 첫 서브에이전트 만들기
- Claude에게 만들어 달라고 하거나 `.claude/agents/<name>.md` 파일 추가 (`/agents` 마법사는 없어짐)
- 간단한 작업으로 위임 테스트

### 4주차: 모두 자동화하기
- 반복 자동화를 위한 첫 스킬 만들기
- 스킬 마켓플레이스를 쓰거나 직접 만들기
- 모든 기능을 엮어 전체 워크플로 만들기

### 계속
- 매달 메모리를 검토하고 갱신
- 패턴이 보이면 새 스킬 추가
- MCP 조회 최적화
- 서브에이전트 프롬프트 다듬기

---

## 훅

### 개요

훅은 Claude Code 이벤트에 반응해 자동으로 실행되는 이벤트 기반 셸 명령입니다. 사람이 끼어들지 않아도 자동화, 검증, 사용자 정의 워크플로를 실행할 수 있습니다.

### 훅 이벤트

Claude Code는 훅 유형 5가지(command, http, mcp_tool, prompt, agent)에 걸쳐 **훅 이벤트 33개**를 지원합니다.

| 훅 이벤트 | 실행 시점 | 사용 사례 |
|------------|---------|-----------|
| **SessionStart** | 세션 시작·재개·clear·compact | 환경 준비, 초기화 |
| **Setup** | `--init-only`로 실행하거나, `-p`와 함께 `--init` / `--maintenance`로 실행할 때 (일반 시작 때는 아님) | 도구 준비, 의존성 설치 |
| **InstructionsLoaded** | CLAUDE.md나 규칙 파일을 불러올 때 | 검증, 변환, 보강 |
| **UserPromptSubmit** | 사용자가 프롬프트를 제출할 때 | 입력 검증, 프롬프트 필터링 |
| **UserPromptExpansion** | 입력한 슬래시 명령(스킬, 사용자 정의 명령, MCP 프롬프트)이 프롬프트로 펼쳐질 때. 막을 수 있음 | `/command` 직접 사용 차단이나 로깅, 스킬별 컨텍스트 추가 |
| **PreToolUse** | 어떤 도구든 실행되기 전 | 검증, 승인 게이트, 로깅 |
| **PermissionRequest** | 권한 대화 상자가 표시될 때 | 자동 승인·거부 흐름 |
| **PermissionDenied** | auto 모드가 도구 호출을 거부할 때 (사용자가 직접 거부할 때는 실행되지 않음) | 로깅, 분석, 정책 시행 |
| **PostToolUse** | 도구 성공 뒤 | 자동 포맷, 알림, 정리 |
| **PostToolUseFailure** | 도구 실행 실패 | 오류 처리, 로깅 |
| **PostToolBatch** | 도구 사용 묶음이 끝난 뒤 | 종합 보고, 일괄 검증 |
| **Notification** | 알림을 보낼 때 | 경보, 외부 연동 |
| **MessageDisplay** | 어시스턴트 메시지 텍스트가 표시되는 동안 | 표시되는 메시지 텍스트 변환이나 숨김 |
| **SubagentStart** | 서브에이전트 생성 | 컨텍스트 주입, 초기화 |
| **SubagentStop** | 서브에이전트 종료 | 결과 검증, 로깅 |
| **Stop** | Claude가 응답을 마칠 때 | 요약 생성, 정리 작업 |
| **StopFailure** | API 오류로 턴이 끝날 때 | 오류 복구, 로깅 |
| **TeammateIdle** | 에이전트 팀의 팀원이 유휴 상태일 때 | 작업 분배, 조율 |
| **TaskCompleted** | 작업이 완료로 표시될 때. todo 도구를 켰을 때만 실행 — 기본으로는 Claude 3.x 모델, Opus 4–4.7, Sonnet 4–4.6, Haiku 4.5에서만 켜져 있고, `CLAUDE_CODE_ENABLE_TODO_TOOLS=1`로 되살림 (v2.1.233) | 작업 후처리 |
| **TaskCreated** | TaskCreate로 작업이 만들어질 때. todo 도구를 켰을 때만 실행 — 기본으로는 Claude 3.x 모델, Opus 4–4.7, Sonnet 4–4.6, Haiku 4.5에서만 켜져 있고, `CLAUDE_CODE_ENABLE_TODO_TOOLS=1`로 되살림 (v2.1.233) | 작업 추적, 로깅 |
| **ConfigChange** | 설정 파일이 바뀔 때 | 검증, 전파 |
| **CwdChanged** | 작업 디렉터리가 바뀔 때 | 디렉터리별 준비 |
| **DirectoryAdded** | 세션 도중 `/add-dir`이나 SDK `register_repo_root` 제어 요청으로 새 작업 디렉터리가 등록될 때 | 새로 추가된 디렉터리용 도구 준비 |
| **FileChanged** | 감시 중인 파일이 바뀔 때 | 파일 모니터링, 다시 빌드 트리거 |
| **PreCompact** | 컨텍스트 압축 전 | 상태 보존 |
| **PostCompact** | 압축 완료 뒤 | 압축 후 작업 |
| **PreModelSwitch** | 요청된 모델 전환이 적용되기 전 | 모델 변경 허용 여부 판단이나 거부 |
| **PostModelSwitch** | 세션 모델이 바뀐 뒤 | 모델 변경 로깅이나 대응 |
| **WorktreeCreate** | 워크트리를 만들 때 | 기본 git 워크트리 생성을 대체. 워크트리 경로를 출력해야 함 |
| **WorktreeRemove** | 워크트리를 제거할 때 | 정리, 자원 해제 |
| **Elicitation** | MCP 서버가 사용자 입력을 요청할 때 | 입력 검증 |
| **ElicitationResult** | 사용자가 elicitation에 응답할 때 | 응답 처리 |
| **SessionEnd** | 세션이 끝날 때 | 정리, 마지막 로깅 |

### 자주 쓰는 훅

훅은 `~/.claude/settings.json`(사용자 수준)이나 `.claude/settings.json`(프로젝트 수준)에서 설정합니다.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx eslint"
          }
        ]
      }
    ]
  }
}
```

### 훅 환경 변수

- 훅 입력은 stdin으로 JSON(`session_id`, `tool_name`, `tool_input.file_path` 등)이 들어옵니다 — `jq`로 읽습니다
- `$CLAUDE_CODE_SESSION_ID` - 현재 세션 식별자
- `$CLAUDE_PROJECT_DIR` - 프로젝트 디렉터리 경로

### 모범 사례

✅ **권장:**
- 훅은 빠르게 유지합니다 (1초 미만)
- 검증과 자동화에 훅을 씁니다
- 오류를 우아하게 처리합니다
- 절대 경로를 씁니다

❌ **피할 것:**
- 훅을 대화형으로 만들기
- 오래 걸리는 작업에 훅 쓰기
- 자격 증명을 직접 넣기

**참고**: 자세한 예제는 [06-hooks/](06-hooks/)를 보세요

---

## 체크포인트와 되감기

### 개요

체크포인트로 대화 상태를 저장하고 이전 시점으로 되감을 수 있어, 안심하고 실험하며 여러 방법을 탐색할 수 있습니다.

### 핵심 개념

| 개념 | 설명 |
|---------|-------------|
| **체크포인트** | 메시지, 파일, 컨텍스트를 포함한 대화 상태의 스냅샷 |
| **되감기** | 이전 체크포인트로 돌아가고 그 뒤의 변경은 버림 |
| **분기점** | 여러 방법을 탐색하기 시작하는 체크포인트 |

### 체크포인트 열기

체크포인트는 사용자 프롬프트마다 자동으로 만들어집니다. 되감으려면:

```bash
# Esc를 두 번 눌러 체크포인트 브라우저 열기
Esc + Esc

# 또는 /rewind 명령 사용
/rewind
```

체크포인트를 고르면 여섯 가지 옵션 중 하나를 선택합니다.
1. **Restore code and conversation** -- 둘 다 그 시점으로 되돌림
2. **Restore conversation** -- 메시지는 되감고 현재 코드는 유지
3. **Restore code** -- 파일은 되돌리고 대화는 유지
4. **Summarize from here** -- 여기서부터의 대화를 요약으로 압축
5. **Summarize up to here** -- 이 시점 이전의 모든 내용을 요약으로 압축
6. **Never mind** -- 취소

### 사용 사례

| 상황 | 워크플로 |
|----------|----------|
| **여러 방법 탐색** | 저장 → A 시도 → 저장 → 되감기 → B 시도 → 비교 |
| **안전한 리팩터링** | 저장 → 리팩터링 → 테스트 → 실패하면 되감기 |
| **A/B 테스트** | 저장 → 디자인 A → 저장 → 되감기 → 디자인 B → 비교 |
| **실수 복구** | 문제 발견 → 마지막 정상 상태로 되감기 |

### 설정

체크포인트는 기본으로 켜져 있으며 `fileCheckpointingEnabled`로 제어합니다.

```json
{
  "fileCheckpointingEnabled": true
}
```

**참고**: 자세한 예제는 [08-checkpoints/](08-checkpoints/)를 보세요

---

## 고급 기능

### 계획 모드

코딩 전에 자세한 구현 계획을 만듭니다.

**켜기:**
```bash
/plan Implement user authentication system
```

**이점:**
- 예상 시간이 담긴 명확한 로드맵
- 위험 평가
- 체계적인 작업 분해
- 검토하고 고칠 기회

### 확장 사고

복잡한 문제를 위한 깊은 추론입니다.

**켜기:**
- 세션 중에 `Alt+T` (macOS는 `Option+T`)로 토글
- Opus 4.7+, Sonnet 5+, Haiku 5.5, Fable은 늘 적응형 추론을 쓰므로 고정 `MAX_THINKING_TOKENS` 예산이 적용되지 않습니다. 깊이는 `/effort`로 조절합니다

```bash
# 노력 수준으로 추론 깊이 조절
/effort high
```

**이점:**
- 트레이드오프를 꼼꼼히 분석
- 더 나은 구조 결정
- 경계 사례 고려
- 체계적인 평가

### 백그라운드 작업

대화를 막지 않고 오래 걸리는 작업을 실행합니다.

**사용법:**
```bash
User: Run tests in background

Claude: Started task bg-1234

/tasks               # 백그라운드 작업 보기와 관리 (별칭: /bashes)
```

### 권한 모드

Claude가 할 수 있는 일을 제어합니다.

| 모드 | 설명 | 사용 사례 |
|------|-------------|----------|
| **manual** | 민감한 작업에 확인을 요청하는 표준 권한 (v2.1.200부터 Manual로 표시. 설정 값은 여전히 `default`이고 훅과 SDK도 이 값을 보고함) | 일반 개발 |
| **acceptEdits** | 파일 편집을 확인 없이 자동 승인 | 믿을 수 있는 편집 워크플로 |
| **plan** | 분석과 계획만, 파일 수정 없음 | 코드 리뷰, 구조 계획 |
| **auto** | 분류기가 안전한 작업은 승인하고 위험한 작업은 막음. 연속 3번 또는 모두 20번 막힌 뒤에야 확인 요청이 다시 나옴 | 안전을 지키며 자율성 확보 |
| **dontAsk** | 확인이 필요한 것은 모두 자동 거부. 미리 승인된 도구와 읽기만 실행 | 잠긴 CI와 스크립트 |
| **bypassPermissions** | 제한 없는 전체 접근, 안전 검사 없음 | CI/CD 파이프라인, 믿을 수 있는 스크립트 |

**사용법:**
```bash
claude --permission-mode plan          # 읽기 전용 분석
claude --permission-mode acceptEdits   # 편집 자동 승인
claude --permission-mode auto          # 안전한 작업 자동 승인
claude --permission-mode dontAsk       # 미리 승인된 도구만, 나머지는 거부
```

### 헤드리스 모드 (출력 모드)

`-p`(print) 플래그로 대화형 입력 없이 Claude Code를 실행해 자동화와 CI/CD에 씁니다.

**사용법:**
```bash
# 특정 작업 실행 (v2.1.302부터 -p는 Manual 모드로 시작하므로 권한 모드나 --allowedTools를 넘김)
claude -p "Run all tests" --permission-mode auto

# 분석할 입력을 파이프로 전달
cat error.log | claude -p "explain this error"

# CI/CD 연동 (GitHub Actions)
- name: AI Code Review
  run: claude -p "Review PR changes and report issues"

# 스크립트용 JSON 출력
claude -p --output-format json "list all functions in src/"
```

### 예약 작업

`/loop` 명령으로 세션 범위 작업을 반복 일정에 따라 실행합니다. 세션보다 오래가는 일정에는 `/schedule`로 클라우드 예약 작업(예전 routines)을 쓰거나 Desktop 예약 작업을 씁니다.

**사용법:**
```bash
/loop every 30m "Run tests and report failures"
/loop every 2h "Check for dependency updates"
/loop every 1d "Generate daily summary of code changes"
```

예약 작업은 백그라운드에서 실행되고 끝나면 결과를 보고합니다. 지속적인 모니터링, 정기 점검, 자동 유지 보수 워크플로에 유용합니다.

### Chrome 연동

Claude Code는 웹 자동화 작업을 위해 Chrome 브라우저와 연동할 수 있습니다. 개발 워크플로 안에서 바로 웹 페이지 이동, 양식 작성, 스크린샷, 웹사이트 데이터 추출 같은 일을 할 수 있습니다.

### 세션 관리

여러 작업 세션을 관리합니다.

**명령:**
```bash
/resume                # 이전 대화 재개
/rename "Feature"      # 현재 세션에 이름 붙이기
/fork                  # 새 세션으로 포크
claude -c              # 가장 최근 대화 이어 가기
claude -r "Feature"    # 이름이나 ID로 세션 재개
```

### 대화형 기능

**키보드 단축키:**
- `Ctrl + R` - 명령 기록 검색
- `Tab` - 자동 완성
- `↑ / ↓` - 명령 기록
- `Ctrl + L` - 화면 지우기

**여러 줄 입력:** `\` + Enter, Option+Enter(macOS), Shift+Enter, Ctrl+J로 새 줄을 시작합니다. `\end` 같은 종료 기호는 없습니다.

### 설정

전체 설정 예시:

```json
{
  "alwaysThinkingEnabled": true,
  "showThinkingSummaries": true,
  "permissions": {
    "defaultMode": "manual"
  }
}
```

백그라운드 작업용 `settings.json` 블록은 없습니다 — 이 기능은 `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` 환경 변수로, 동시 실행 수는 `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`(기본 `20`)로 제어합니다.

**참고**: 종합 가이드는 [09-advanced-features/](09-advanced-features/)를 보세요

---

## 모델과 추론 노력

Claude Code는 적응형 추론 노력을 지원하는 다음 모델을 지원합니다.

| 모델 | 컨텍스트 윈도 | 노력 수준 | 기본 노력 (Claude Code) |
|-------|----------------|---------------|------------------------------|
| Claude Opus 5.5 | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `medium` — v2.1.280부터 기본 모델이자 기본 Opus 모델 (Claude Code v2.1.280+ 필요) |
| Claude Sonnet 5.5 | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `medium` — v2.1.284부터 Anthropic API의 기본 Sonnet 모델 (Claude Code v2.1.284+ 필요) |
| Claude Haiku 5.5 | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `medium` — v2.1.293부터 Anthropic API의 기본 Haiku 모델 (Claude Code v2.1.293+ 필요) |
| Claude Opus 5 | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `high` — v2.1.219부터 v2.1.280 전까지 기본 Opus 모델 (Claude Code v2.1.219+ 필요) |
| Claude Sonnet 5 | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `high` — v2.1.197부터 v2.1.280 전까지 Pro/Team Standard의 기본 모델 (지금은 Opus 5.5) |
| Claude Opus 4.8 | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `high` (v2.1.154부터) |
| Claude Opus 4.7 (레거시) | 100만 토큰 (네이티브) | `low`, `medium`, `high`, `xhigh`, `max` | `xhigh` (Opus 4.7 출시일 2026-04-16부터) |
| Claude Sonnet 4.6 | 100만 토큰 | `low`, `medium`, `high`, `max` | Pro/Max 구독자는 `high` (v2.1.117에서 `medium`에서 올라감) |
| Claude Haiku 4.5 | 20만 토큰 | — (노력 수준 미지원) | — |

> **참고**: `xhigh`는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7에서 쓸 수 있습니다. `max`는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8/4.7/4.6, Sonnet 4.6에서 동작합니다 (세션 동안만). Haiku 4.5는 노력 수준을 지원하지 않습니다.

> **참고**: v2.1.117에서 Opus 4.7 세션이 `/context`를 네이티브 100만 대신 20만 기준으로 계산하던 버그가 고쳐졌습니다 — Opus 4.7에서 실제로 100만 컨텍스트를 쓰려면 v2.1.117 이상으로 업그레이드합니다. Opus 5와 Opus 4.8도 네이티브 100만 토큰 윈도를 가집니다.

> **참고**: `/cost`와 `/stats`는 v2.1.118에서 `/usage`로 합쳐졌습니다. 이제 `/usage`가 비용·통계 등의 탭을 가진 대표 명령이고, `/cost`와 `/stats`는 해당 탭을 여는 단축 별칭으로 남아 있습니다. v2.1.149부터 비용 보기는 분류별(스킬, 서브에이전트, 플러그인, MCP 서버별 비용)로도 지출을 나눠 보여 줍니다.

## 자료

- [Claude Code 문서](https://code.claude.com/docs/en/overview)
- [Claude Code 변경 기록](https://code.claude.com/docs/en/changelog)
- [MCP GitHub 서버](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---
**최종 수정일**: 2026년 10월 26일
**Claude Code 버전**: 2.1.310
**출처**:
- https://code.claude.com/docs/en/plugins/manifest-reference
- https://code.claude.com/docs/en/plugins/create
- https://code.claude.com/docs/en/plugins/install
- https://code.claude.com/docs/en/settings-reference
- https://code.claude.com/docs/en/interactive-mode
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/tools-reference#task-tool-availability
- https://www.anthropic.com/news/claude-sonnet-5
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/model-config
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/sub-agents
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/remote-control
- https://code.claude.com/docs/en/hooks#userpromptexpansion
- https://code.claude.com/docs/en/headless#auto-approve-tools
- https://code.claude.com/docs/en/permission-modes#which-mode-a-non-interactive-run-starts-in
- https://code.claude.com/docs/en/routines
- https://code.claude.com/docs/en/scheduled-tasks
- https://code.claude.com/docs/en/changelog#2-1-307
- https://code.claude.com/docs/en/skills#work-on-claude-api-projects
**호환 모델**: Claude Fable 5.1, Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
