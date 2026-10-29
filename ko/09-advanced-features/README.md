<!-- i18n-source: 09-advanced-features/README.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 고급 기능

Claude Code의 고급 기능을 다루는 종합 가이드입니다. 계획 모드, 확장 사고, 자동 모드, 백그라운드 태스크, 권한 모드, 출력 모드(비대화형), 세션 관리, 대화형 기능, 채널, 음성 받아쓰기, 원격 제어, 웹 세션, 데스크톱 앱, 태스크 목록, 프롬프트 제안, git 워크트리, 샌드박싱, 관리형 설정, 구성을 설명합니다.

## 목차

1. [개요](#개요)
2. [계획 모드](#계획-모드)
3. [확장 사고](#확장-사고)
4. [자동 모드](#자동-모드)
5. [백그라운드 태스크](#백그라운드-태스크)
6. [Monitor 도구 (이벤트 기반 스트림)](#monitor-도구-이벤트-기반-스트림)
7. [동적 워크플로](#동적-워크플로)
8. [예약 태스크](#예약-태스크)
9. [권한 모드](#권한-모드)
10. [헤드리스 모드](#헤드리스-모드)
11. [세션 관리](#세션-관리)
12. [세션 간 메시지](#세션-간-메시지)
13. [대화형 기능](#대화형-기능)
14. [출력 스타일](#출력-스타일)
15. [상태 줄](#상태-줄)
16. [TUI 모드 (전체 화면)](#tui-모드-전체-화면)
17. [음성 받아쓰기](#음성-받아쓰기)
18. [채널](#채널)
19. [Chrome 연동](#chrome-연동)
20. [원격 제어](#원격-제어)
21. [웹 세션](#웹-세션)
22. [데스크톱 앱](#데스크톱-앱)
23. [태스크 목록](#태스크-목록)
24. [프롬프트 제안](#프롬프트-제안)
25. [Git 워크트리](#git-워크트리)
26. [샌드박싱](#샌드박싱)
27. [관리형 설정 (엔터프라이즈)](#관리형-설정-엔터프라이즈)
28. [구성과 설정](#구성과-설정)
29. [신뢰와 권한 범위](#신뢰와-권한-범위)
30. [에이전트 팀](#에이전트-팀)
31. [모범 사례](#모범-사례)
32. [추가 자료](#추가-자료)

---

## 개요

Claude Code의 고급 기능은 계획, 추론, 자동화, 제어 장치로 핵심 기능을 넓힙니다. 복잡한 개발 작업, 코드 리뷰, 자동화, 여러 세션 관리를 위한 정교한 워크플로를 가능하게 합니다.

**주요 고급 기능:**
- **계획 모드**: 코딩 전에 자세한 구현 계획 작성
- **확장 사고**: 복잡한 문제를 위한 깊은 추론
- **자동 모드**: 백그라운드 안전 분류기가 실행 전에 동작마다 검토
- **백그라운드 태스크**: 대화를 막지 않고 오래 걸리는 작업 실행
- **권한 모드**: Claude가 할 수 있는 일 제어(`manual` — 예전 `default`, `acceptEdits`, `plan`, `auto`, `dontAsk`, `bypassPermissions`)
- **출력 모드(print mode)**: 자동화와 CI/CD를 위해 Claude Code를 비대화형으로 실행(`claude -p`)
- **세션 관리**: 여러 작업 세션 관리
- **대화형 기능**: 단축키, 여러 줄 입력, 명령 기록
- **음성 받아쓰기**: 20개 언어 STT를 지원하는 눌러서 말하기(push-to-talk) 음성 입력
- **채널**: MCP 서버가 실행 중인 세션에 메시지를 보냄(리서치 프리뷰)
- **원격 제어**: Claude.ai나 Claude 앱에서 Claude Code 제어
- **웹 세션**: claude.ai/code에서 브라우저로 Claude Code 실행
- **데스크톱 앱**: 시각적 diff 검토와 여러 세션을 위한 독립 앱
- **태스크 목록**: 컨텍스트 압축 뒤에도 유지되는 태스크 추적
- **프롬프트 제안**: 컨텍스트에 맞춘 똑똑한 명령 제안
- **Git 워크트리**: 병렬 작업을 위한 격리된 워크트리 브랜치
- **샌드박싱**: OS 수준의 파일 시스템·네트워크 격리
- **관리형 설정**: plist, 레지스트리, 관리형 파일을 통한 엔터프라이즈 배포
- **구성**: JSON 설정 파일로 동작을 사용자 지정

---

## 계획 모드

계획 모드는 Claude가 복잡한 작업을 구현하기 전에 깊이 생각해, 사용자가 검토하고 승인할 수 있는 자세한 계획을 만들게 합니다.

### 계획 모드란?

계획 모드는 두 단계로 진행합니다.
1. **계획 단계**: Claude가 작업을 분석하고 자세한 구현 계획을 만듦
2. **구현 단계**: 승인을 받으면 Claude가 계획을 실행

### 계획 모드를 쓸 때

✅ 계획 모드가 알맞은 경우:
- 여러 파일에 걸친 복잡한 리팩터링
- 새 기능 구현
- 아키텍처 변경
- 데이터베이스 마이그레이션
- 대규모 API 재설계

❌ 계획 모드가 필요 없는 경우:
- 간단한 버그 수정
- 서식 변경
- 파일 하나 편집
- 간단한 질문

### 계획 모드 켜기

**슬래시 명령**:
```bash
/plan Implement user authentication system
```

**CLI 플래그**:
```bash
claude --permission-mode plan
```

**기본값으로 설정**:
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**단축키**:
- `Shift + Tab` - 권한 모드 전환(plan 포함)
- `Alt + M` - 권한 모드 전환(Windows 전용, 런타임이 VT 입력 모드를 켜지 않을 때)

### 계획 모드의 이점

- **구조화된 구현 방식**: 구현 단계를 자세히 나눔
- **검토와 승인**: 실행 전에 계획을 승인하거나 고침
- **위험 파악**: 구현 전에 생길 수 있는 문제를 찾음
- **명확한 단계**: 정리된 구현 단계와 마일스톤

### 예시: 기능 구현

```text
User: /plan Add a real-time notification system

Claude: I'll create a plan for implementing real-time notifications.

## Implementation Plan

### Phase 1: Infrastructure Setup
1. Install WebSocket libraries (socket.io)
2. Set up WebSocket server
3. Configure connection management

### Phase 2: Backend Implementation
4. Create notification service
5. Implement notification queue
6. Add database schema for notifications
7. Create API endpoints

### Phase 3: Frontend Integration
8. Set up WebSocket client
9. Create notification UI components
10. Implement notification state management
11. Add notification permissions

### Phase 4: Testing & Deployment
12. Write unit tests
13. Write integration tests
14. Update documentation
15. Deploy to staging

Estimated time: 3-4 hours
Files to modify: 12
New files to create: 8

Ready to proceed? (yes/no/modify plan)

User: yes

Claude: [계획에 따라 구현 시작]
```

### 계획 수정

```text
User: Modify the plan - skip the queue for now, we'll add it later

Claude: Updated plan:
[큐를 뺀 수정 계획 표시]

User: Looks good, proceed

Claude: [수정한 계획 구현]
```

### 계획 모드 설정

계획 모드는 CLI 플래그나 슬래시 명령으로 켭니다.

```bash
# CLI로 계획 모드 켜기
claude --permission-mode plan

# 또는 REPL 안에서 /plan 슬래시 명령 사용
/plan Implement user authentication system
```

**계획용 모델 별칭**: `opusplan` 모델 별칭을 쓰면 계획에는 Opus를, 실행에는 Sonnet을 씁니다.

```bash
claude --model opusplan "design and implement the new API"
```

**외부에서 계획 편집**: `Ctrl+G`를 누르면 현재 계획을 외부 에디터로 열어 자세히 고칠 수 있습니다.

> **v2.1.112 변경 사항**: 계획 파일 이름이 이제 무작위 단어 대신 계획을 만든 프롬프트를 따르므로, 둘러보고 재사용하기 쉽습니다.

> **v2.1.136 변경 사항 — 계획 모드의 쓰기 차단은 무조건 적용**: 이제 `permissions.allow`에 일치하는 `Edit(...)` 규칙이 있어도 계획 모드는 모든 파일 쓰기를 막습니다. 이전에는 허용하는 `Edit(...)` 규칙 때문에 계획 모드에서도 쓰기가 될 수 있었지만, 이 우회로가 막혔습니다. 예전 동작에 기대던 워크플로라면 편집하기 전에 계획 모드에서 나가세요(`Shift+Tab`).

---

## 확장 사고

확장 사고(extended thinking)는 Claude가 해법을 내놓기 전에 복잡한 문제를 더 오래 추론하게 합니다.

### 확장 사고란?

확장 사고는 Claude가 신중하게 단계별로 추론하는 과정입니다.
- 복잡한 문제를 나눔
- 여러 접근 방식을 검토
- 트레이드오프를 평가
- 엣지 케이스까지 추론

### 확장 사고 켜기

**단축키**:
- `Option + T`(macOS) / `Alt + T`(Windows/Linux) - 확장 사고 전환. Opus 5.5(v2.1.280부터 기본 모델), Sonnet 5.5, Haiku 5.5, Fable 모델에서는 사고를 끌 수 없으며, 이 전환 키, `alwaysThinkingEnabled`, `MAX_THINKING_TOKENS=0`이 효과가 없습니다.

**자동 활성화**:
- 모든 모델에서 기본으로 켜짐(Opus 5, Opus 4.8, Opus 4.7, Sonnet 4.6, Haiku 4.5)
- Opus 5 / Opus 4.8: 노력 수준(effort level)을 쓰는 적응형 추론: `low`(○), `medium`(◐), `high`(●), `xhigh`, `max`. 기본값은 Opus 5.5(v2.1.280부터 기본 모델), Sonnet 5.5, Haiku 5.5에서 `medium`, Opus 5(v2.1.219), Sonnet 5, Opus 4.8(v2.1.154), Opus 4.6, Sonnet 4.6에서 `high`, Opus 4.7에서 `xhigh`입니다. `xhigh`는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7에서 쓸 수 있습니다(Opus 4.6 / Sonnet 4.6에서는 `high`로 대체). `max`는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8/4.7/4.6, Sonnet 4.6에서 동작합니다(세션 한정). Haiku 4.5에는 노력 수준이 없습니다. Opus 5, Opus 4.8, Opus 4.7은 기본 컨텍스트 윈도가 100만 토큰입니다(1M 컨텍스트 수정은 v2.1.117에 반영. 그 전에는 `/context`가 Opus 4.7을 200K 윈도로 잘못 계산해 자동 압축이 너무 일찍 일어났습니다). v2.1.129부터 `/context`는 시각화를 UI 안에서만 보여 주고, ASCII 시각화가 대화 컨텍스트로 새지 않으므로(호출당 약 1.6k 토큰 절약) 마음껏 써도 됩니다.
- Opus 4.6 / Sonnet 4.6의 Pro/Max 구독자: v2.1.117에서 기본 노력 수준이 `medium`에서 `high`로 올라갔습니다.
- 그 밖의 모델: 최대 31,999 토큰의 고정 예산

**설정 방법**:
- 전환: `Alt+T` / `Option+T`, 또는 `/config`
- 추론 보기: `Ctrl+O`(상세 모드)
- 노력 수준 설정: `/effort` 명령이나 `--effort` 플래그

**사용자 지정 예산**:
```bash
export MAX_THINKING_TOKENS=1024
```

**노력 수준**(Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6에서 지원. Haiku 4.5는 미지원):
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low (○), medium (◐), high (●), xhigh (Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8/4.7), max 중 선택 — Opus 5와 Opus 4.8의 기본값은 high
```

**CLI 플래그**:
```bash
claude --effort high "complex architectural review"
```

**슬래시 명령**:
```text
/effort high
```

> **참고:** 프롬프트에 "ultrathink"라는 키워드를 넣으면 깊은 추론 모드가 켜집니다. 노력 수준 `low`, `medium`, `high`, `max`는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6에서 지원합니다(Haiku 4.5는 없음). `xhigh`는 Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7에서 쓸 수 있습니다. 기본 노력 수준은 Opus 5.5, Sonnet 5.5, Haiku 5.5에서 `medium`, Opus 5, Sonnet 5, Opus 4.8(과 Opus 4.6 / Sonnet 4.6)에서 `high`, Opus 4.7에서 `xhigh`입니다. 처음 실행할 때 기본 노력 수준을 고정하는 Opus 4.8, Opus 4.7과 달리 Opus 5에는 그런 고정이 없어, 이전에 정한 수준이 그대로 이어집니다. `/effort` 슬라이더에는 **Ultracode** 전환(Tab 또는 `/effort ultracode [on|off]`)도 있는데, 이는 모델 노력 수준이 **아닙니다**. 세션의 노력 수준과 상관없이 Claude가 동적 워크플로를 조율하게 합니다. v2.1.284부터는 더 이상 `xhigh`를 강제하지 않고 수준을 바꿔도 켜진 채로 남습니다. `--effort ultracode`와 Agent SDK의 `effortLevel: "ultracode"`는 여전히 `xhigh`를 설정합니다.

### Opus 5의 안전 분류기 대체 모델

과부하나 사용 불가를 처리하는 [`fallbackModel` 설정](#대체-모델-fallbackmodel)과 별개로, Claude Code는 안전 분류기가 요청에 표시를 하면 **분류별 대체(fallback)**를 적용합니다. Claude Code v2.1.219 이상이 필요합니다.

- **Opus 5**: 사이버 보안으로 표시된 요청은 Opus 4.8로 다시 실행됩니다. 생물학으로 표시된 요청은 거부로 끝나는데, Opus 5는 대체 모델 없이 자체 생물학 분류기를 쓰기 때문입니다. 처음 표시된 요청부터 거부됩니다.
- 공격 보안이나 생물학 분야의 작업(침투 테스트, CTF(Capture the Flag) 실습, 생물학 관련 코드베이스)은 대체가 자주, 흔히 첫 요청부터 일어납니다.

여기서 중요한 이유: Opus 5에서 [보안 리뷰 서브에이전트](../04-subagents/README.md)나 CTF/침투 테스트 작업을 돌리면, 사이버 보안 프롬프트는 아무 말 없이 Opus 4.8(노력 수준 상한과 컨텍스트 윈도가 다름)로 다시 실행되고 생물학 관련 프롬프트는 바로 거부된다고 예상해야 합니다. 이 동작이 방해된다면 다른 모델을 명시적으로 고정하세요.

**전환 제어** — `switchModelsOnFlag` 설정(v2.1.170+, 기본값 `true`)이 대체가 조용히 일어날지 정합니다. `false`로 하면 Claude Code가 멈추고, 모델을 바꿀지 프롬프트를 고칠지 고르게 합니다. `/config`에서 **Switch models when a message is flagged**로 표시됩니다.

```json
{
  "switchModelsOnFlag": false
}
```

### 확장 사고의 이점

- **깊은 추론**: 복잡한 문제를 더 오래 생각함
- **더 나은 아키텍처 결정**: 종합적인 평가로 근거 있는 선택
- **엣지 케이스 고려**: 생길 수 있는 문제와 상황을 끝까지 생각함
- **철저한 분석**: 복잡한 문제 해결에 대한 구조화된 접근

### 예시: 아키텍처 결정

```text
User: ultrathink Should we use microservices or a monolith for our e-commerce platform?

Claude: Let me think through this architectural decision carefully...

<extended_thinking>
## Context Analysis
- Team size: 5 developers
- Expected traffic: 10k daily users initially
- Need to scale to 100k+ users
- Limited DevOps resources
- 6-month timeline to MVP

## Approach 1: Microservices
Pros:
+ Independent scaling
+ Technology flexibility
+ Team independence
+ Fault isolation

Cons:
- Operational complexity
- Network overhead
- Distributed tracing needed
- Requires DevOps expertise
- Slower initial development

## Approach 2: Monolith
Pros:
+ Simpler deployment
+ Faster initial development
+ Easier debugging
+ Lower operational overhead
+ Single codebase

Cons:
- Scaling challenges later
- All-or-nothing deployments
- Technology lock-in
- Potential for tight coupling

## Decision Factors
1. Team size (5 devs) → Favors monolith
2. DevOps resources (limited) → Favors monolith
3. Timeline (6 months) → Favors monolith
4. Future scale (100k users) → Achievable with monolith + caching
5. Complexity budget → Limited

## Recommendation
Start with a **modular monolith**:
- Single deployment unit
- Clear module boundaries
- Clean interfaces between modules
- Prepare for future extraction to microservices if needed

This gives you:
✓ Fast initial development
✓ Low operational complexity
✓ Option to extract services later
✓ Clear architecture

Migration path: When you reach 50k+ users or specific modules need independent scaling, extract those modules into microservices.
</extended_thinking>

**Recommendation**: Implement a modular monolith with clean module boundaries.

This approach balances your current constraints (team size, timeline, DevOps resources) with future scalability needs. You can migrate specific modules to microservices later when there's a clear business need.
```

### 확장 사고 설정

확장 사고는 환경 변수, 단축키, CLI 플래그로 제어합니다.

```bash
# 사고 토큰 예산 설정
export MAX_THINKING_TOKENS=16000

# 노력 수준 설정 (Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6): low (○), medium (◐), high (●), xhigh (Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8/4.7), max 중 선택 — Opus 5와 Opus 4.8의 기본값은 high
export CLAUDE_CODE_EFFORT_LEVEL=high
```

세션 중에는 `Alt+T` / `Option+T`로 전환하고, `/effort`로 노력 수준을 정하거나, `/config`에서 설정합니다.

> **간결한 시스템 프롬프트(v2.1.154):** 간결한(lean) 시스템 프롬프트가 이제 Haiku 4.5, Sonnet 5, Opus 4.7과 그 계열의 이전 모델을 뺀 모든 모델의 **기본값**입니다(Haiku 5.5와 Sonnet 5.5도 사용). Opus 5와 Opus 4.8에서 기본 토큰 부담이 줄어듭니다.

---

## 자동 모드

자동 모드(Auto Mode)는 백그라운드 안전 분류기로 실행 전에 동작마다 검토하는 권한 모드입니다. 위험한 작업은 막으면서 Claude가 자율적으로 일하게 합니다. 모든 요금제에서 쓸 수 있지만 지원 모델이 필요합니다(Anthropic API와 Claude Platform on AWS에서는 Claude Opus 4.6 이상, Sonnet 4.6 이상, Haiku 5.5, Fable 모델. Bedrock, Vertex, Foundry, 로그인한 Claude 앱 게이트웨이 세션에서는 Sonnet 5 이상, Opus 4.7 이상, Haiku 5.5, Fable 모델). Team과 Enterprise에서는 기본으로 켜져 있으며, 관리자가 관리형 설정으로 조직 전체에서 끌 수 있습니다.

### 요구 사항

자동 모드는 계정이 다음 요구 사항을 모두 충족할 때만 쓸 수 있습니다.

- **요금제**: 모든 요금제.
- **조직**: Team과 Enterprise에서는 기본으로 사용 가능. 관리자는 관리형 설정에서 `permissions.disableAutoMode`를 `"disable"`로 설정해 조직 전체에서 끌 수 있습니다.
- **모델**: Anthropic API와 Claude Platform on AWS에서는 Claude Opus 4.6 이상, Sonnet 4.6 이상, Haiku 5.5, Fable 모델. Amazon Bedrock, Google Cloud Agent Platform(Vertex AI), Microsoft Foundry, 로그인한 Claude 앱 게이트웨이 세션에서는 Claude Sonnet 5 이상, Opus 4.7 이상, Haiku 5.5(v2.1.293+), Fable 모델만. 이전 모델(Sonnet 4.5, Opus 4.5, Haiku 4.5, claude-3 모델)은 어느 제공자에서도 지원하지 않습니다.
- **제공자**: Anthropic API, Claude Platform on AWS, Amazon Bedrock, Google Cloud Agent Platform(Vertex AI), Microsoft Foundry, 로그인한 Claude 앱 게이트웨이 세션에서 기본으로 사용 가능. v2.1.158부터 v2.1.206까지는 Anthropic API와 Claude Platform on AWS를 뺀 나머지에서 `CLAUDE_CODE_ENABLE_AUTO_MODE=1`을 설정해야 자동 모드가 켜졌지만, v2.1.207에서 이 요구 사항이 없어졌습니다. 이 변수는 호환성을 위해 아직 받지만 v2.1.207부터 효과가 없습니다.
- **분류기**: 토큰 비용이 추가로 듭니다. 단, Enterprise 요금제와 Claude API 계정에서는 v2.1.278+가 서버 쪽에서 무료로 검사합니다

### 자동 모드 켜기

```bash
# 권한 모드를 설정하지 않았으면 대화형 세션은 자동 모드로 시작 (v2.1.284+)
# 직접 고르려면 (예: 설정의 기본 모드가 다를 때):
claude --permission-mode auto

# 또는 REPL에서 Shift+Tab으로 전환
```

> **v2.1.112 변경 사항**: 자동 모드에 더 이상 `--enable-auto-mode` 플래그가 필요 없습니다. Max 구독자는 Opus 4.7에서 바로 쓸 수 있습니다.

> **v2.1.158 변경 사항**: Opus 4.7/4.8에서 자동 모드를 Bedrock, Vertex, Foundry에서도 쓸 수 있게 되었으며, `CLAUDE_CODE_ENABLE_AUTO_MODE=1`이 있어야 했습니다.
>
> **v2.1.207 변경 사항**: 그 요구 사항이 없어졌습니다. 이제 Bedrock, Vertex AI, Microsoft Foundry, 로그인한 Claude 앱 게이트웨이 세션에서 Claude Sonnet 5, Opus 4.7, Opus 4.8, Fable 5(그리고 v2.1.219부터 Opus 5)로 자동 모드를 기본으로 쓸 수 있으며, 플래그나 환경 변수가 필요 없습니다. 관리자는 관리형 설정의 `disableAutoMode`로 끌 수 있습니다. `CLAUDE_CODE_ENABLE_AUTO_MODE`는 호환성을 위해 아직 받지만 v2.1.207부터 효과가 없습니다.

또는 기본 권한 모드로 설정합니다.

```bash
claude --permission-mode auto
```

설정 파일로 지정:
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 분류기 동작 방식

백그라운드 분류기는 다음 순서로 동작마다 평가합니다.

1. **허용/거부 규칙** -- 명시적 권한 규칙을 먼저 확인
2. **읽기 전용/편집 자동 승인** -- 파일 읽기와 편집은 자동 통과
3. **분류기** -- 백그라운드 분류기가 동작을 검토
4. **대체** -- 연속 3번 또는 총 20번 차단되면 사용자에게 묻는 방식으로 돌아감

### 기본으로 막는 동작

자동 모드는 기본으로 다음을 막습니다.

| 막는 동작 | 예시 |
|-----------|------|
| 셸로 파이프하는 설치 | `curl \| bash` |
| 민감한 데이터 외부 전송 | 네트워크로 API 키, 자격 증명 전송 |
| 운영 배포 | 운영 환경을 대상으로 한 배포 명령 |
| 대량 삭제 | 큰 디렉터리에 `rm -rf` |
| IAM 변경 | 권한과 역할 수정 |
| main 강제 푸시 | `git push --force origin main` |

> **분류기가 판단하는 범위 확대(v2.1.218)**: 분류기는 `rm -rf /`, `rm -rf ~`처럼 파일 시스템 루트나 홈 디렉터리를 대상으로 한 삭제도 판단하며, 명령 치환이나 프로세스 치환 안에 든 삭제도 마찬가지입니다. v2.1.218 전에는 단순한 형태는 승인을 물었고, 치환 형태는 v2.1.208부터 v2.1.217까지 승인을 물었습니다. 백그라운드 `&` 검사와 수상한 Windows 경로 검사도 더 이상 권한 대화 상자를 열지 않고 분류기가 판단합니다. v2.1.281부터는 이렇게 알려진 핵심 경로 삭제가 자동 모드에서 다시 승인을 묻는데, 터미널에서는 2분 카운트다운과 함께, 다른 곳에서는 즉시 거부됩니다. `CLAUDE_CODE_DISABLE_DANGEROUS_RM_TIMEOUT=1`일 때만 분류기가 판단합니다.

### 기본으로 허용하는 동작

| 허용 동작 | 예시 |
|-----------|------|
| 로컬 파일 작업 | 프로젝트 파일 읽기, 쓰기, 편집 |
| 선언된 의존성 설치 | 매니페스트에 있는 `npm install`, `pip install` |
| 읽기 전용 HTTP | 문서를 가져오는 `curl` |
| 현재 브랜치에 푸시 | `git push origin feature-branch` |

### 자동 모드 설정

**기본 규칙을 JSON으로 출력**:
```bash
claude auto-mode defaults
```

**자동 모드 기본 설정 복원**(v2.1.212). 확인 프롬프트가 나오며 `--yes`로 건너뜁니다.
```bash
claude auto-mode reset
claude auto-mode reset --yes
```

**신뢰하는 인프라 설정**: 엔터프라이즈 배포에서는 `autoMode.environment` 관리형 설정으로 합니다. 관리자가 신뢰하는 CI/CD 환경, 배포 대상, 인프라 패턴을 정의할 수 있습니다.

**`/auto-mode-setup`으로 환경 항목 초안 만들기**: 이 안내형 명령은 프로젝트와 최근 세션을 읽고 `autoMode.environment` 항목(때로는 규칙 항목도)을 제안하며, 수락하면 `~/.claude/settings.json`에 씁니다. Pro, Max, Team은 v2.1.228+(네이티브 Windows는 v2.1.233+), Enterprise와 Claude Console API 키는 v2.1.283+가 필요합니다. v2.1.304부터는 Bedrock, Vertex, Foundry, 텔레메트리를 끈 환경에서도 동작합니다.

#### `"$defaults"`로 기본값 확장하기 (v2.1.118)

v2.1.118부터 `autoMode.allow`, `autoMode.soft_deny`, `autoMode.environment`는 `"$defaults"` 토큰을 받습니다. 이 토큰을 쓰면 내장 목록을 대체하지 않고 사용자 규칙을 내장 목록에 **덧붙입니다**. v2.1.118 전에는 사용자가 정의한 배열이 아무 말 없이 내장 목록을 덮어썼습니다.

#### `autoMode.hard_deny`로 무조건 차단하기 (v2.1.136)

`autoMode.hard_deny`(v2.1.136+)는 **추정한 사용자 의도와 상관없이** 특정 종류의 동작을 막는 분류기 규칙 배열입니다. 루트 경로의 `rm -rf`나 보호 브랜치의 `git push --force`처럼 자동 모드에서 절대 실행되면 안 되는 동작에 씁니다. `soft_deny`와 달리 hard-deny 규칙은 분류기가 융통성을 둘 수 없습니다.

```json
{
  "autoMode": {
    "hard_deny": ["Bash(rm -rf /:*)", "Bash(git push --force*)"]
  }
}
```

**이전 (내장 목록 대체 — v2.1.118 전 동작):**

```json
{
  "autoMode": {
    "allow": ["Bash(gh pr list:*)"]
  }
}
```

**이후 (내장 목록 확장 — v2.1.118+):**

```json
{
  "autoMode": {
    "allow": ["$defaults", "Bash(gh pr list:*)"],
    "soft_deny": ["$defaults", "Bash(kubectl delete:*)"],
    "environment": ["$defaults", "trusted-ci.internal"]
  }
}
```

`"$defaults"`를 쓰면 기본 제공 규칙을 유지하면서 그 위에 조직이나 프로젝트 전용 규칙을 더할 수 있습니다.

#### `autoMode.classifyAllShell`로 모든 셸 명령 분류하기 (v2.1.193)

`autoMode.classifyAllShell`(boolean, v2.1.193+)은 **모든** Bash/PowerShell 명령을 자동 모드 분류기로 보냅니다. 세션의 모든 셸 명령을 분류기가 검사하게 하려면 켭니다.

```json
{
  "autoMode": {
    "classifyAllShell": true
  }
}
```

같은 릴리스에서, 자동 모드가 동작을 막으면 **거부 이유**를 보여 줍니다. 대화 기록, 거부 알림, `/permissions` 아래의 최근 거부 목록에서 볼 수 있습니다(v2.1.193+).

#### 의도 기반 내장 보호 (v2.1.183)

사용자가 설정하는 `hard_deny`와 별개로, 자동 모드는 이번 세션에서 사용자가 명시적으로 요청하지 않았다면 다음 파괴적 명령을 기본으로 막습니다.

- `git reset --hard`, `git checkout -- .`, `git clean -fd`, `git stash drop`
- `git commit --amend`(이번 세션에서 에이전트가 만든 커밋이 아닐 때)
- `terraform destroy`, `pulumi destroy`, `cdk destroy`(특정 스택을 요청하지 않았을 때)

추정한 의도에 따라 동작하는 내장 기본 보호이므로, 이것들을 직접 `hard_deny`에 넣을 필요는 없습니다.

### 대체 동작

분류기가 확신하지 못하면 자동 모드는 사용자에게 묻는 방식으로 돌아갑니다.
- 분류기가 **연속 3번** 막은 뒤
- 한 세션에서 분류기가 **총 20번** 막은 뒤

그래서 분류기가 자신 있게 승인하지 못할 때도 사용자는 언제나 통제권을 유지합니다.

### 자동 모드와 비슷한 권한 미리 넣기

백그라운드 분류기 없이 더 간단한 방법을 원하면, `~/.claude/settings.json`에 보수적인 기본 안전 권한 규칙을 넣을 수 있습니다. 스크립트는 읽기 전용과 로컬 조회 규칙으로 시작하고, 편집, 테스트, 로컬 git 쓰기, 패키지 설치, GitHub 쓰기 작업은 원할 때만 추가하게 합니다.

**파일:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 무엇이 추가될지 미리 보기 (변경 없음)
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 보수적인 기본 규칙 적용
python3 09-advanced-features/setup-auto-mode-permissions.py

# 필요할 때만 기능 추가
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

스크립트는 다음 분류에 걸쳐 규칙을 추가합니다.

| 분류 | 예시 |
|------|------|
| 핵심 읽기 전용 도구 | `Read(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)`, `WebFetch(*)` |
| 로컬 조회 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)`, `Bash(cat:*)` |
| 선택 편집 | `Edit(*)` |
| 선택 테스트/빌드 | `Bash(pytest:*)`, `Bash(python3 -m pytest:*)`, `Bash(cargo test:*)` |
| 선택 git 쓰기 | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)`, `Bash(git switch:*)`, `Bash(git stash:*)`, `Bash(git tag:*)` |
| 패키지 관리자 | `Bash(npm ci:*)`, `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(pip3 install:*)` |
| 빌드와 테스트 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 일반 셸 | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

위험한 작업(`rm -rf`, `sudo`, 강제 푸시, `DROP TABLE`, `terraform destroy` 등)은 일부러 뺐습니다. 스크립트는 멱등적이라 두 번 실행해도 규칙이 중복되지 않습니다.

---

## 백그라운드 태스크

백그라운드 태스크는 오래 걸리는 작업을 대화를 막지 않고 실행하게 합니다.

### 백그라운드 태스크란?

백그라운드 태스크는 사용자가 계속 작업하는 동안 비동기로 실행됩니다.
- 긴 테스트 모음
- 빌드 과정
- 데이터베이스 마이그레이션
- 배포 스크립트
- 분석 도구

**기본 사용법:**
```bash
User: Run tests in background

Claude: Started task bg-1234

/tasks    # 백그라운드 작업(셸과 서브에이전트)을 보고 관리. 여기서 태스크 중지
Ctrl+B    # 실행 중인 Bash 명령이나 에이전트를 백그라운드로 보내기
```

### 백그라운드 태스크 시작하기

```text
User: Run the full test suite in the background

Claude: Starting tests in background (task-id: bg-1234)
You can continue working while tests run.

[대화를 계속할 수 있음]

User: Meanwhile, let's refactor the auth module

Claude: [테스트가 백그라운드에서 도는 동안 auth 모듈 작업]

[나중에 테스트가 끝나면]

Claude: 📢 Background task bg-1234 completed:
✅ 245 tests passed
❌ 3 tests failed
View results: /tasks
```

### 백그라운드 태스크 관리

`/tasks`(별칭 `/bashes`)를 실행하면 실행 중인 셸과 서브에이전트가 나열되고, 하나를 열어 출력을 보거나 중지할 수 있습니다. `/task` 하위 명령은 없습니다.

```text
User: /tasks

[백그라운드 작업 화면이 열림: 실행 중인 셸과 서브에이전트,
 끝난 서브에이전트도 포함. 하나를 골라 출력을 보거나,
 여기서 중지.]
```

### 예시: 병렬 개발

```text
User: Run the build in the background

Claude: Starting build... (task-id: bg-5001)

User: Also run the linter in background

Claude: Starting linter... (task-id: bg-5002)

User: While those run, let's implement the new API endpoint

Claude: [빌드와 린터가 도는 동안 API 엔드포인트 구현]

[10분 뒤]

Claude: 📢 Build completed successfully (bg-5001)
📢 Linter found 12 issues (bg-5002)

User: Show me the linter issues

Claude: [bg-5002의 린터 출력 표시]
```

### 설정

백그라운드 태스크용 `settings.json` 블록은 없습니다. 이 기능은 환경 변수로 제어합니다.

```bash
# 백그라운드 Bash/서브에이전트 실행과 Ctrl+B 끄기 (MCP 태스크와 Monitor 도구는 계속 백그라운드에서 실행)
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=1
```

동시 실행 수도 백그라운드 태스크 설정이 아닙니다. 한 번에 몇 개의 에이전트가 도는지는 `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`(기본값 `20`)로 정합니다.

백그라운드 Bash와 PowerShell 명령에는 무인 세션에서만 시간 제한이 있습니다(v2.1.288). `-p` 실행, Agent SDK 세션, CI, 클라우드 세션에서 `run_in_background`로 시작한 명령은 `timeout`(기본 30분, 최대 2시간)이 지나면 멈추고, 멈추면 Claude에게 알립니다. 터미널, 데스크톱 앱, VS Code 세션의 백그라운드 명령에는 시간 제한이 없습니다(v2.1.285–v2.1.287에서는 모든 세션에 제한이 적용되었습니다).

---

## Monitor 도구 (이벤트 기반 스트림)

> **v2.1.98 신규**: Monitor 도구를 쓰면 Claude가 백그라운드 명령의 stdout을 지켜보다가 일치하는 이벤트가 나타나는 순간 반응합니다. 오래 걸리는 프로세스를 기다리기 위한 폴링 루프와 `sleep`을 대신합니다.

Monitor는 stdout에 쓰는 셸 명령이면 무엇이든 붙일 수 있습니다. 명령의 stdout 줄 하나하나가 세션을 깨우는 알림이 됩니다. Claude가 명령을 정하면 하네스가 출력을 흘려보내고 이벤트가 생길 때마다 전달합니다. 바탕이 되는 프로세스를 띄우는 방법은 관련된 [백그라운드 태스크](#백그라운드-태스크) 단락을 참고하세요.

> **기한(v2.1.271)**: 이제 모든 Monitor 감시에는 기한이 있습니다. 최대 30분이고, 단일 프롬프트 `-p` 실행에서는 10분입니다. 기한이 지나면 Claude에게 알려 다시 감시를 걸 수 있게 합니다. 끝나지 않는 감시를 거는 방법은 더 이상 없습니다.

> **사용 조건(v2.1.310)**: Amazon Bedrock, Claude Platform on AWS, Google Cloud Agent Platform, Microsoft Foundry에서, 또는 `DISABLE_TELEMETRY`나 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`를 설정했을 때 Monitor에는 Claude Code v2.1.310 이상이 필요합니다. Windows에서는 Git Bash가 필요합니다.

### 왜 중요한가

`/loop`나 `sleep`으로 폴링하면 바뀐 것이 있든 없든 주기마다 API 왕복 한 번을 씁니다. Monitor는 이벤트가 생길 때까지 조용히 있어, 명령이 조용한 동안 **토큰을 전혀** 쓰지 않습니다. 이벤트가 생기면 Claude가 바로 반응하므로, 다음 폴링 주기까지 기다리며 늦게 알아채는 일도 없습니다. 몇 분 넘게 걸리는 작업이라면 폴링 루프보다 싸고 빠릅니다.

### 자주 쓰는 두 가지 패턴

**스트림 필터**는 오래 실행되는 출처의 연속 출력을 지켜봅니다. 명령은 계속 실행되고, 일치하는 줄마다 이벤트가 됩니다.

```bash
tail -f /var/log/app.log | grep --line-buffered "ERROR"
```

**폴링 후 내보내기 필터**는 출처를 주기적으로 확인하고 무언가 바뀔 때만 내보냅니다. API, 데이터베이스처럼 기본 스트림이 없는 대상에 씁니다.

```bash
last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" || true
  last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
  sleep 30
done
```

### 구체적인 예

"개발 서버를 시작하고 오류를 감시해 줘." Claude는 서버를 백그라운드 태스크로 띄우고 Monitor 필터(`tail -F server.log | grep --line-buffered -E "ERROR|FATAL"`)를 붙인 뒤 세션이 조용해집니다. 로그에 오류 줄이 나타나는 순간 Claude가 깨어나 오류를 읽고, 사용자가 확인하지 않아도 서버를 재시작하거나, 버그를 고치거나, 사용자에게 알리는 식으로 대응할 수 있습니다.

> **주의**: `grep`으로 파이프할 때는 **항상** `grep --line-buffered`를 씁니다. 이 옵션이 없으면 grep이 stdout을 4KB 단위로 버퍼링해, 트래픽이 적은 스트림에서는 이벤트가 몇 분씩 늦어질 수 있습니다. 실제로 Monitor가 고장 나는 가장 흔한 원인입니다. 필터가 조용하면 안 되는데 조용하다면 먼저 `--line-buffered` 플래그를 확인하세요.

---

## 동적 워크플로

> **v2.1.154 신규**

동적 워크플로(dynamic workflows)는 Claude가 수십에서 수백 개의 백그라운드 [서브에이전트](../04-subagents/README.md)를 **결정론적으로** 조율하게 합니다. 팬아웃, 파이프라인, 병렬 단계를 모델의 즉흥에 맡기지 않고 스크립트로 정해 둡니다. 에이전트 하나는 컨텍스트 윈도 하나를 쓰지만, 워크플로는 작업을 여러 에이전트에 나눠 맡기고 결과를 다시 합칩니다.

v2.1.219부터 동적 워크플로의 기본값은 **중간 크기 지침(에이전트 10개 미만 목표)**입니다. v2.1.271부터 Pro 요금제로 로그인했다면 **작은 크기**가 기본입니다. `/config`의 **Dynamic workflow size**에서 다른 크기나 제한 없음을 고르거나, 설정 파일에 `workflowSizeGuideline` 키를 넣습니다. 실행 중인 워크플로의 상태 줄은 현재 크기를 보여 주고, 바꾸려면 `/config`로 가라고 안내합니다.

### 언제 쓸까

- **넓은 범위** — 여러 파일/관점을 병렬로 감사하거나 리뷰할 때.
- **신뢰도** — 서로 독립된 관점을 만들고, 반영하기 전에 결과를 적대적으로 검증할 때.
- **컨텍스트 하나를 넘는 규모** — 컨텍스트 하나에 담을 수 없는 대규모 마이그레이션, 광범위한 일괄 작업, 조사.

이미 이해하고 있는 일회성 작업이라면 여전히 에이전트 하나(또는 직접 편집)가 알맞습니다. 워크플로는 작업이 여러 갈래로 퍼질 때 진가를 발휘합니다.

### 실행하고 보기

- **실행**: 작업용 워크플로를 만들어 달라고 Claude에게 요청합니다(예: "`src/`의 모든 파일을 리뷰하는 워크플로를 돌려 줘"). Claude가 조율 스크립트를 작성해 백그라운드에서 실행합니다.
- **보기**: `/workflows` 명령은 실행 중이거나 끝난 워크플로 실행을 진행 상황과 함께 보여 줍니다.
- **`ultracode`**: `/effort` 슬라이더의 Ultracode 전환(Tab)이나 `/effort ultracode [on|off]`로 켭니다. 그러면 세션의 노력 수준과 상관없이 Claude가 기본으로 동적 워크플로를 조율합니다. v2.1.284부터 이 전환은 노력 수준을 바꾸지 않습니다(예전에는 `xhigh`를 강제했습니다). `--effort ultracode`와 Agent SDK의 `effortLevel: "ultracode"`는 여전히 `xhigh`를 설정합니다. `/effort ultracode`는 그 세션 동안만 유지되며, 모든 세션을 이 상태로 시작하려면 설정에 `ultracode` 키를 넣습니다(`effortLevel`과 `CLAUDE_CODE_EFFORT_LEVEL`은 `ultracode`를 받지 않음). (v2.1.160부터 실행 키워드는 `ultracode`이며, "workflow"라는 단어만으로는 더 이상 실행되지 않습니다.)

워크플로는 서브에이전트 모델 위에 만들어집니다. 개별 에이전트를 정의하고 범위를 정하는 방법은 [서브에이전트](../04-subagents/README.md)를 참고하세요.

---

## 예약 태스크

예약 태스크(Scheduled Tasks)는 프롬프트를 반복 일정에 따라 자동으로 실행하거나 일회성 알림으로 실행하게 합니다. 태스크는 세션 범위라 Claude Code가 켜져 있는 동안 실행되고 세션이 끝나면 지워집니다. v2.1.72+부터 사용할 수 있습니다.

> **"Routines"는 이제 "클라우드 예약 태스크"입니다(2026년 10월)**: Anthropic 문서는 `/schedule`(별칭 `/routines`, 아래 참고)로 만드는 클라우드 태스크인 Routines의 이름을 **cloud scheduled tasks (formerly routines)**로 바꾸었고, 이 페이지의 `/loop`와 cron 태스크는 **session-scoped scheduled tasks**라고 부릅니다. 예전 이름은 `claude.ai/code/routines` URL, `/fire` API, 관리자 전환 **Scheduled tasks (Was Routines)**에 남아 있습니다.

### `/loop` 명령

```bash
# 간격을 명시
/loop 5m check if the deployment finished

# 자연어
/loop check build status every 30 minutes
```

정확한 일정이 필요하면 표준 5필드 cron 표현식도 쓸 수 있습니다.

### 일회성 알림

정해진 시각에 한 번 실행되는 알림을 설정합니다.

```text
remind me at 3pm to push the release branch
in 45 minutes, run the integration tests
```

### 예약 태스크 관리

| 도구 | 설명 |
|------|------|
| `CronCreate` | 새 예약 태스크 만들기 |
| `CronList` | 활성 예약 태스크 모두 나열. v2.1.136부터 출력에 조건과 예약된 프롬프트 본문도 들어 있어, 열어 보지 않고도 각 cron이 무엇을 실행할지 확인할 수 있음 |
| `CronDelete` | 예약 태스크 제거 |

**한도와 동작**:
- 세션당 최대 **예약 태스크 50개**
- 세션 범위 — 세션이 끝나면 지워짐
- 반복 태스크는 **3일** 뒤 자동 만료
- Claude Code가 실행 중일 때만 실행 — 놓친 실행을 나중에 보충하지 않음

### 동작 세부 사항

| 항목 | 내용 |
|------|------|
| **반복 지터** | 간격의 최대 10%(최대 15분) |
| **일회성 지터** | :00/:30 경계에서 최대 90초 |
| **놓친 실행** | 보충 없음 — Claude Code가 실행 중이 아니었으면 건너뜀 |
| **지속성** | 재시작하면 유지되지 않음 |

### 클라우드 예약 태스크

`/schedule`로 Anthropic 인프라에서 실행되는 클라우드 예약 태스크를 만듭니다.

```text
/schedule daily at 9am run the test suite and report failures
```

클라우드 예약 태스크는 재시작해도 유지되며 로컬에서 Claude Code가 실행 중일 필요가 없습니다. 자동 모드를 쓸 수 없는 경우가 아니면 실행마다 자동 모드로 시작하므로, 분류기가 동작을 승인하거나 막습니다. 다만 ask 규칙이 일치하거나 차단이 자동 모드의 반복 차단 한도에 이르면, 실행은 세션에서 멈추고 응답을 기다립니다. 사용자 지정 역할을 쓰는 Enterprise 요금제에서는 예약 태스크 권한이 있는 역할도 필요합니다.

### 예약 태스크 끄기

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

> **API 키 사용 시 `/schedule` 자동 비활성화(v2.1.139)**: `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN`, `apiKeyHelper` 중 하나라도 설정되어 있으면 claude.ai로 로그인해 있어도 클라우드 `/schedule`은 아무 말 없이 쓸 수 없게 됩니다. 같은 조건에서 [원격 제어](#원격-제어-끄기-disableremotecontrol-v21128), claude.ai MCP 커넥터, 알림 설정도 꺼집니다. `/schedule`을 쓰려면 API 키 설정을 지우세요(또는 Pro/Max OAuth로 실행). 로컬 `CronCreate`는 영향받지 않습니다.

### 예시: 배포 감시

```text
/loop 5m check the deployment status of the staging environment.
        If the deploy succeeded, notify me and stop looping.
        If it failed, show the error logs.
```

> **팁**: 예약 태스크는 세션 범위입니다. 재시작해도 유지되는 자동화에는 CI/CD 파이프라인, GitHub Actions, 데스크톱 앱 예약 태스크를 씁니다.

---

## 권한 모드

권한 모드는 Claude가 명시적 승인 없이 할 수 있는 동작을 정합니다.

### 사용할 수 있는 권한 모드

| 모드 | 동작 |
|---|---|
| `manual` | 파일 읽기만 가능. 다른 동작은 모두 물어봄. v2.1.200에서 `default`에서 이름이 바뀌었으며 `default`도 별칭으로 계속 받음 |
| `acceptEdits` | 파일 읽기와 편집 가능. 명령은 물어봄 |
| `plan` | 파일 읽기만 가능(조사 모드, 편집 없음) |
| `auto` | 백그라운드 안전 분류기 검사를 거쳐 모든 동작 가능. 지원 모델(모든 제공자에서 Sonnet 5 이상, Opus 4.7 이상, Haiku 5.5, Fable 모델. Anthropic API와 Claude Platform on AWS에서는 Opus 4.6, Sonnet 4.6도 가능)과 제공자가 필요. 모든 요금제에서 사용 가능. [자동 모드](#자동-모드) 참고 |
| `bypassPermissions` | 권한 검사 없이 모든 동작(위험) |
| `dontAsk` | 미리 승인한 도구만 실행. 나머지는 모두 거부 |

> **참고**: `default` 권한 모드는 v2.1.200에서 **Manual**로 이름이 바뀌었고(CLI, `--help`, VS Code, JetBrains 모두), 켜져 있는 동안 하단에 회색 ⏸ 배지가 표시됩니다(v2.1.203). `--permission-mode manual`과 `--permission-mode default` 모두 동작하며, 설정의 `"defaultMode": "manual"`과 `"defaultMode": "default"`도 마찬가지입니다. 설정 키는 `permissions.defaultMode`입니다. `permissions.mode` 키는 없으므로 아래 예시는 정식 철자를 씁니다.

CLI에서 `Shift+Tab`으로 모드를 순환합니다. 기본값은 `--permission-mode` 플래그나 `permissions.defaultMode` 설정으로 정합니다. 권한 모드를 설정하지 않으면 v2.1.284부터 모든 요금제와 제공자에서 대화형 터미널과 VS Code 세션이 자동 모드로 시작합니다(v2.1.283에서는 Bedrock, Google Cloud Agent Platform, Foundry 같은 서드파티 제공자나 텔레메트리를 끈 경우에만 그랬습니다). `permissions.defaultMode`가 여전히 우선하며, 자동 모드를 쓸 수 없는 세션은 대신 Manual로 시작합니다. v2.1.302부터 권한 모드를 정하지 않은 `claude -p`나 Agent SDK 실행은 Manual 모드(설정값 `default`)로 시작하고, 승인이 필요한 도구 호출을 거부합니다. `--permission-mode`(예: `auto`, `acceptEdits`)나 `--allowedTools`를 넘기세요.

> **계획 모드는 셸 명령을 분류기에 맡김(v2.1.218)**: [자동 모드](#자동-모드)를 쓸 수 있고 `useAutoModeDuringPlan` 설정이 켜져 있으면(기본값), 계획 중의 셸 명령을 사용자에게 묻지 않고 분류기가 검토합니다. 승인된 명령은 실행되고 거부된 명령은 막힙니다. 계획 모드는 여전히 파일 쓰기를 무조건 막습니다.

v2.1.160부터는 `acceptEdits`도 셸 시작 파일(`.zshenv`, `.zlogin`, `.bash_login`, `~/.config/git/`)과 코드를 실행하는 빌드 설정(`.npmrc`, `.yarnrc*`, `bunfig.toml`, `.bazelrc`, `.pre-commit-config.yaml`, `.devcontainer/` 등)을 쓰기 전에는 물어봅니다. 그러지 않으면 의도치 않은 명령 실행으로 이어질 수 있기 때문입니다.

> **`--dangerously-skip-permissions` 적용 경로 확대(v2.1.121, v2.1.126)**: `--dangerously-skip-permissions` CLI 플래그(와 같은 역할의 `bypassPermissions` 모드)는 이제 훨씬 넓은 허용 목록(`.claude/skills/`, `.claude/agents/`, `.claude/commands/`, `.claude/`, `.git/`, `.vscode/`, 셸 설정 파일)에 쓸 때도 묻지 않습니다. 치명적인 삭제 명령(`rm -rf /` 등)은 이 모드에서도 묻습니다. v2.1.281부터 터미널 프롬프트는 2분을 기다린 뒤 다시 쓰라는 힌트와 함께 명령을 거부해 무인 세션이 계속 진행되게 하고, v2.1.309부터 데스크톱 앱과 VS Code는 그 힌트와 함께 바로 거부합니다(`CLAUDE_CODE_DISABLE_DANGEROUS_RM_TIMEOUT=1`이면 타임아웃 없음). 자동 모드도 이런 핵심 경로 삭제를 똑같이 다룹니다. 터미널에서는 2분 프롬프트, 다른 곳에서는 즉시 거부이며, 그 변수를 설정했을 때만 분류기에 보냅니다. v2.1.304부터 `/Volumes/USB`나 WSL의 `/mnt/c/Users` 같은 마운트된 드라이브 경로를 재귀 `rm`으로 지우는 것도 `rm` 허용 규칙이 있어도 같은 처리를 받습니다. 이 프롬프트를 끄려면 `CLAUDE_CODE_DISABLE_MOUNTED_DRIVE_RM_PROMPT=1`로 Claude Code를 시작하세요(설정의 `env` 블록은 무시됨). 이 플래그는 예전보다 날카로운 도구라고 생각하고, 버려도 되는 샌드박스에서만 쓰세요.

> **Windows 셸 감지(v2.1.120, v2.1.126)**: 이제 Git for Windows / Git Bash가 필요 없습니다. Git Bash가 없으면 Claude Code는 셸 도구로 PowerShell을 씁니다. v2.1.126부터 PowerShell 도구가 켜져 있으면 PowerShell이 *주* 셸이 되며, Microsoft Store로 설치했거나, PATH 없이 MSI로 설치했거나, `.NET global tool`로 설치한 PowerShell 7도 감지합니다.

> **Bedrock/Vertex/Foundry의 Windows에서 PowerShell 도구 기본 사용(v2.1.143)**: v2.1.143부터 Bedrock, Vertex, Foundry 사용자는 **Windows에서 PowerShell 도구가 기본으로 켜집니다**. Claude Code는 `-ExecutionPolicy Bypass`로 PowerShell을 호출하므로 시스템 정책이 `Restricted`여도 스크립트가 실행됩니다. 시스템 실행 정책을 따르게 하려면 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`을 설정합니다. PowerShell 도구를 아예 끄려면 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`을 설정합니다.

### 켜는 방법

**단축키**:
```bash
Shift + Tab  # default → acceptEdits → plan 순으로 순환, 쓸 수 있으면 bypassPermissions/auto까지 (dontAsk는 순환에 나오지 않음)
```

**슬래시 명령**:
```bash
/plan                  # 계획 모드 진입
```

**CLI 플래그**:
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**설정**:
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 권한 모드 예시

#### 기본 모드
중요한 동작은 Claude가 확인을 받습니다.

```text
User: Fix the bug in auth.ts

Claude: I need to modify src/auth.ts to fix the bug.
The change will update the password validation logic.

Approve this change? (yes/no/show)
```

#### 계획 모드
실행 전에 구현 계획을 검토합니다.

```text
User: /plan Implement user authentication system

Claude: I'll create a plan for implementing authentication.

## Implementation Plan
[단계와 절차가 담긴 자세한 계획]

Ready to proceed? (yes/no/modify)
```

#### 편집 자동 수락 모드
파일 수정을 자동으로 수락합니다.

```text
User: acceptEdits
User: Fix the bug in auth.ts

Claude: [묻지 않고 변경]
```

### 사용 사례

**코드 리뷰**:
```text
User: claude --permission-mode plan
User: Review this PR and suggest improvements

Claude: [코드를 읽고 피드백을 주지만 수정은 못 함]
```

**페어 프로그래밍**:
```text
User: claude --permission-mode default
User: Let's implement the feature together

Claude: [변경할 때마다 승인을 받음]
```

**자동화 작업**:
```text
User: claude --permission-mode acceptEdits
User: Fix all linting issues in the codebase

Claude: [묻지 않고 파일 편집을 자동 수락]
```

---

## 헤드리스 모드

출력 모드(`claude -p`)를 쓰면 Claude Code가 대화형 입력 없이 실행되어 자동화와 CI/CD에 딱 맞습니다. 예전 `--headless` 플래그를 대신하는 비대화형 모드입니다.

### 출력 모드란?

출력 모드로 할 수 있는 일:
- 자동 스크립트 실행
- CI/CD 연동
- 일괄 처리
- 예약 작업

### 출력 모드(비대화형)로 실행하기

```bash
# 특정 작업 실행
claude -p "Run all tests" --permission-mode auto

# 파이프로 받은 내용 처리
cat error.log | claude -p "Analyze these errors"

# CI/CD 연동 (GitHub Actions)
- name: AI Code Review
  run: claude -p "Review PR"
```

### 출력 모드 사용 예시 더 보기

```bash
# 출력을 받으며 특정 작업 실행
claude -p "Run all tests and generate coverage report" --permission-mode auto

# 구조화된 출력
claude -p --output-format json "Analyze code quality"

# stdin 입력 사용
echo "Analyze code quality" | claude -p "explain this"
```

### 예시: CI/CD 연동

**GitHub Actions**:
```yaml
# .github/workflows/code-review.yml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "Review this PR for:
            - Code quality issues
            - Security vulnerabilities
            - Performance concerns
            - Test coverage
            Output results as JSON" > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```

### 출력 모드 설정

출력 모드(`claude -p`)는 자동화를 위한 여러 플래그를 지원합니다.

```bash
# 자율 턴 수 제한
claude -p --max-turns 5 "refactor this module"

# 구조화된 JSON 출력
claude -p --output-format json "analyze this codebase"

# 스키마 검증 사용
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "find bugs in this code"

# 세션 저장 끄기
claude -p --no-session-persistence "one-off analysis"
```

### 안전 모드 (문제 해결용)

`--safe-mode`(와 `CLAUDE_CODE_SAFE_MODE` 환경 변수, 예: `CLAUDE_CODE_SAFE_MODE=1`)는 **모든 사용자 지정 요소를 끈** 채로 Claude Code를 시작합니다. CLAUDE.md, 플러그인, 스킬, 훅, MCP 서버가 모두 꺼집니다.

```bash
# 모든 사용자 지정 요소를 끄고 실행
claude --safe-mode

# 환경 변수로 같은 효과
CLAUDE_CODE_SAFE_MODE=1 claude
```

문제 해결 도구입니다. 사용자 설정이 문제를 일으킬 때 안전 모드로 실행하면, 문제가 사용자 설정에 있는지 Claude Code 자체에 있는지 가려낼 수 있습니다.

---

## 세션 관리

여러 Claude Code 세션을 효과적으로 관리합니다.

### 세션 관리 명령

| 명령 | 설명 |
|------|------|
| `/resume` | ID나 이름으로 대화 재개 |
| `/rename` | 현재 세션 이름 짓기 |
| `/fork [prompt]` | 대화를 새 독립 백그라운드 세션으로 복사하고 여기서 계속 작업(v2.1.212+) |
| `/subtask <task>` | 대화 전체를 이어받는 포크 서브에이전트를 띄우고 결과를 여기로 보고받음(v2.1.212+) |
| `/branch [name]` | 이 시점의 대화 사본으로 전환하고 원본은 보존 |
| `claude -c` | 가장 최근 대화 이어서 하기 |
| `claude -r "session"` | 이름이나 ID로 세션 재개 |

### 세션 재개하기

**마지막 대화 이어서 하기**:
```bash
claude -c
```

**이름 붙인 세션 재개**:
```bash
claude -r "auth-refactor" "finish this PR"
```

**현재 세션 이름 바꾸기**(REPL 안에서):
```text
/rename auth-refactor
```

> **v2.1.212 변경 사항**: 에이전트 뷰에서 인수 없이 `/resume`을 입력하면 이제 지난 세션 선택기가 열리고(보이는 목록에서 제거한 세션 포함), 고른 세션을 백그라운드 세션으로 재개합니다.

> **v2.1.304 변경 사항**: Pro와 Max 요금제에서 오래 쉬었던 대화를 재개해 5시간 사용 한도를 눈에 띄게 쓸 것 같으면 **Resume this conversation?** 대화 상자가 나타납니다. 그래도 재개하거나, 새 대화를 시작할 수 있습니다(이전 대화를 언급하면 Claude가 찾아볼 수 있음).

### 세션 포크와 분기

대화를 복사하는 명령은 세 가지이며, *사본이 어디서 실행되는지*가 다릅니다.

`/subtask <task>`는 대화 전체를 이어받는 포크 서브에이전트를 띄워, 사용자가 계속 일하는 동안 작업을 맡깁니다. `claude agents`에 따로 한 줄을 차지하며, 끝나면 결과가 대화로 돌아옵니다.

```text
/subtask Investigate why the auth tests are flaky
```

`/fork [prompt]`는 대화를 새 **백그라운드 세션**으로 복사합니다. 사본은 지금까지의 모든 내용으로 시작해 독립적으로 실행되며, 이 대화로는 아무것도 돌아오지 않습니다.

```text
/fork Try the OAuth approach end to end
```

맡기지 않고 직접 사본으로 옮겨 가려면 `/branch [name]`을 씁니다. 원본은 보존되며 `/resume`으로 돌아갈 수 있습니다.

```text
/branch try-oauth-instead
```

> **참고**: **v2.1.212**에서 `/fork`와 `/subtask`의 역할이 서로 바뀌었습니다. v2.1.161 전에는 `/fork`가 `/branch`의 별칭이었고, v2.1.161부터 v2.1.211까지는 지금 `/subtask`가 하는 일, 즉 포크 서브에이전트를 띄웠습니다. 에이전트 뷰를 끄면 `/subtask`는 쓸 수 없고 `/fork`가 포크 서브에이전트 동작을 유지합니다.

또는 CLI에서 포크합니다.
```bash
claude --resume auth-refactor --fork-session "try OAuth instead"
```

### 세션 유지

세션은 자동으로 저장되고 재개할 수 있습니다.

```bash
# 마지막 대화 이어서 하기
claude -c

# 이름이나 ID로 특정 세션 재개
claude -r "auth-refactor"

# 재개하면서 실험용으로 포크
claude --resume auth-refactor --fork-session "alternative approach"
```

### 사용량 한도 뒤 자동 계속 (v2.1.234)

**v2.1.234**부터 claude.ai 사용량 한도에 막힌 세션은 한도가 초기화되면 자동으로 이어집니다. 다시 프롬프트를 보낼 필요가 없습니다. `/config`의 "Continue automatically at usage limit"에서 켜고 끕니다.

### 세션 요약 (v2.1.108)

자리를 비웠다가 세션으로 돌아오면 Claude가 그동안 한 일을 짧게 요약해 보여 줄 수 있습니다. 모든 요금제와 제공자에서 기본으로 켜져 있으며, `/config` → **Session recap**에서 끕니다.

> **OTEL 텔레메트리 — 피드백 설문 다시 켜기(v2.1.136+)**: OpenTelemetry 데이터를 수집하는 조직은 `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL=1`로 Anthropic의 세션 품질 설문을 다시 켤 수 있습니다. 이전에 텔레메트리 파이프라인에서 빠지도록 바뀌었기 때문에 OTEL 배포에서는 기본으로 꺼져 있습니다.

> **OTEL 텔레메트리 — `assistant_response` 로그 이벤트(v2.1.193+)**: Claude Code는 모델 응답 텍스트를 담은 `claude_code.assistant_response` OpenTelemetry 로그 이벤트를 내보내, OTEL 파이프라인이 기존 도구/이벤트 텔레메트리와 함께 Claude의 응답도 수집할 수 있게 합니다.

> **OTEL 텔레메트리 — 캐시와 GenAI 속성(v2.1.300)**: `api_request` 이벤트에 `cache_creation_5m_tokens`/`cache_creation_1h_tokens`가 추가되고, 스팬에 `gen_ai.provider.name` 같은 GenAI 시맨틱 규약 속성이 붙습니다.

> **OTEL 텔레메트리 — `tool_decision` 속성(v2.1.301)**: `tool_decision` 이벤트에 `classifier_verdict`와 `permission_mode`가 추가됩니다.

> **OTEL 텔레메트리 — `safety_event`(v2.1.307)**: 권한 규칙, 자동 모드, 안전 검사, 훅, 샌드박스가 거부하거나, 확인을 요청하거나, 돌려보낸 도구 호출마다 새 `claude_code.safety_event` 로그 이벤트가 발생하며 `category`, `decision`(`accept`/`reject`/`sent_back`), `decision_source` 속성을 담습니다. 명령 텍스트, 파일 경로, 호스트 이름은 담지 않습니다.

> **OTEL 텔레메트리 — 연결 속성(v2.1.310)**: `tool_decision`과 `tool_result` 이벤트에 호출을 API 응답과 연결하는 `request_id`가 추가되고, 서브에이전트 이벤트에는 `parent_agent_id`가 붙습니다.

**요약 동작 제어:**

```bash
/recap                                 # 직접 요약 실행
/config                                # 자동 요약 켜기/끄기
```

또는 환경 변수로:
```bash
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=0 claude   # 요약 끄기
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1 claude   # 요약 강제로 켜기
```

---

## 세션 간 메시지

> **v2.1.224에서 추가**되었고 v2.1.239까지 확장되었습니다. **macOS, Linux, (v2.1.239부터) Windows**에서 쓸 수 있습니다.

예전에는 세션이 서로 고립된 섬이었습니다. 세션 간 메시지(cross-session messaging)를 쓰면 한 Claude Code 세션이 다른 세션(다른 컴퓨터의 세션과 클라우드 세션 포함)과 대화할 수 있습니다.
그래서 질문을 다시 설명하는 대신, 알맞은 컨텍스트를 이미 갖고 있는 세션에 넘길 수 있습니다.

### 세션 찾기

`ListAgents`는 보낼 수 있는 모든 대상을 나열합니다. 직접 띄운 서브에이전트, 이 컴퓨터의 다른 로컬 세션, 클라우드 세션, (원격 제어가 연결되어 있으면) 다른 컴퓨터의 세션입니다. 각 행에는 종류가 표시되며, v2.1.229부터는 `offline`과 `cloud` 표시도 붙어 연결할 수 있는 세션과 잠든 세션을 구분할 수 있습니다.

**각 행의 이름이 주소**입니다. 그 이름으로 보냅니다.

### 메시지 보내기

`SendMessage`는 대상과 메시지를 받습니다.

```text
SendMessage({ to: "<session name>", message: "What did you conclude about the retry logic?" })
```

v2.1.232부터는 이름만 써도 됩니다. 두 행이 정말 같은 이름을 갖고 있지 않다면 구분용 참조를
덧붙이지 않아도 됩니다.

### 세션이 유휴 상태가 될 때까지 기다리기 (`notify_when_idle`, v2.1.236)

메시지를 보낼 세션이 작업 중이라면 보통은 폴링하기보다 끝났을 때 알고 싶을 것입니다.
`SendMessage`는 바로 그 용도로 `notify_when_idle` 입력을 받습니다.

```text
SendMessage({
  to: "auth-refactor",
  message: "ping me when the migration finishes",
  notify_when_idle: true
})
```

**선택 사항이며 한 번만** 동작합니다. 대상 세션이 다음에 유휴 상태가 될 때 알림을 한 번 보내고,
구독은 끝납니다. 폴링 루프도 없고, 세션이 다시 바빠졌다가 유휴 상태가 되어도 알림을 반복하지 않습니다.

v2.1.239의 관련 변경 두 가지: 이제 `ListAgents`가 활성 팀원과 함께 **세션 자신의 이름**도 보여 주어
(그래서 세션이 다른 세션에게 자신을 부르는 법을 알려 줄 수 있음), 세션 간 메시지를
**Windows**에서도 쓸 수 있게 되었습니다.

### `@` 멘션 축약형 (v2.1.232)

도구를 직접 호출하는 대신, 프롬프트에서 세션을 `@`로 멘션해
메시지를 보낼 수 있습니다.

```text
@auth-refactor did the migration tests pass?
```

### 받는 메시지 제어: `crossSessionInbound`

받는 메시지는 `crossSessionInbound` 설정(v2.1.224+)으로 정합니다.

| 값 | 동작 |
|---|---|
| `"accept"` | 받은 메시지를 이 세션의 Claude에게 전달 |
| `"hold"` | 메시지가 왔다는 알림만 보이고 전달하지 않음 |
| `"refuse"` | 받은 메시지를 버림 |

값은 `accept < hold < refuse`의 단계를 이루며, **프로젝트와 로컬 설정은 사용자 범위 값보다
엄격할 때만 적용**됩니다. 프로젝트는 받는 메시지를 더 엄격하게 할 수는 있어도 느슨하게 할 수는 없습니다.
v2.1.232부터 이 설정은 `/config`에도 "Messages from your other sessions" 행으로 나옵니다.

### 범위와 한계

- 같은 컴퓨터의 로컬 세션과 클라우드 세션.
- 다른 컴퓨터의 원격 제어 세션. 이름으로 지정(v2.1.225).
- 클라우드 세션은 메시지를 **받지만** 아직 로컬 세션에 답장을 보낼 수는 없습니다.
  답은 그 세션의 대화 기록에서 읽습니다.
- macOS와 Linux는 v2.1.224부터, Windows는 v2.1.239부터.

---

## 대화형 기능

### 단축키

Claude Code는 효율을 높이는 단축키를 지원합니다. 공식 문서의 전체 레퍼런스입니다.

| 단축키 | 설명 |
|--------|------|
| `Ctrl+C` | 현재 입력/생성 취소 |
| `Ctrl+D` | Claude Code 종료 |
| `Ctrl+G` | 외부 에디터에서 계획 편집 |
| `Ctrl+L` | 화면 다시 그리기(다시 그리기만 함 — 두 번 눌러 `/clear`하는 단축키는 v2.1.238에서 제거) |
| `Ctrl+O` | 상세 출력 전환(추론 보기) |
| `Ctrl+R` | 기록 역방향 검색. 기본으로 **모든 프로젝트의 모든 프롬프트**를 검색(v2.1.129+). 선택기 안에서 `Ctrl+S`를 누르면 현재 프로젝트로 좁힘. 이전 버전은 현재 프로젝트만 기본으로 검색 |
| `Ctrl+T` | 태스크 목록 보기 전환 |
| `Ctrl+B` | 실행 중인 태스크를 백그라운드로 |
| `Esc+Esc` | 코드/대화 되감기 |
| `Shift+Tab` / `Alt+M` | 권한 모드 전환 |
| `Option+P` / `Alt+P` | 모델 바꾸기 |
| `Option+T` / `Alt+T` | 확장 사고 전환 |
| `Option+O` / `Alt+O` | 빠른 모드 전환(`/fast`) |
| `Ctrl+X` `Ctrl+K` | 모든 백그라운드 서브에이전트 중지 |
| `Ctrl+Enter` (또는 `Ctrl+X` `Ctrl+S`) | 지금 보내기 — 대기 중인 메시지를 한꺼번에 보냄. v2.1.281부터는 턴을 취소하지 않고 실행 중인 도구와 서브에이전트를 백그라운드로 보냄. 보낸 메시지와 대기 메시지는 모델이 받을 때까지 회색으로 표시(v2.1.275) |
| `Ctrl+S` | 현재 프롬프트를 임시 보관. 다시 누르면 복원 |
| `Ctrl+_` | 프롬프트 입력의 마지막 편집 취소 |
| `:` | 단어 처음에 `:`를 입력하면 이모지 단축 코드 자동 완성이 열림. 예: `:heart:`(v2.1.217+) |

**줄 편집(표준 readline 단축키):**

| 단축키 | 동작 |
|--------|------|
| `Ctrl + A` | 줄 처음으로 이동 |
| `Ctrl + E` | 줄 끝으로 이동 |
| `Ctrl + K` | 줄 끝까지 잘라내기 |
| `Ctrl + U` | 줄 처음까지 잘라내기 |
| `Ctrl + W` | 앞 단어 지우기 |
| `Ctrl + Y` | 붙여 넣기(yank) |
| `Tab` | 자동 완성 |
| `↑ / ↓` | 명령 기록 |

### 접근성

화면 낭독기 모드(v2.1.208+)는 CLI를 화면 낭독기에 맞춘 일반 텍스트 렌더링 모드로 바꿉니다. CLI 플래그, 환경 변수, 또는 사용자 설정(`~/.claude/settings.json`), 관리형 설정, `--settings`로 넘긴 파일의 `axScreenReader` 키로 켭니다. v2.1.305부터 Claude Code는 프로젝트와 로컬 설정에 있는 이 키와 `env` 블록의 `CLAUDE_AX_*` 변수를 무시합니다.

```bash
claude --ax-screen-reader
```

```bash
export CLAUDE_AX_SCREEN_READER=1
```

```json
{
  "axScreenReader": true
}
```

### 키 바인딩 사용자 지정

`/keybindings`를 실행하면 `~/.claude/keybindings.json`이 편집용으로 열려 단축키를 직접 만들 수 있습니다(v2.1.18+).

**설정 형식**:

```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```

기본 단축키를 해제하려면 바인딩을 `null`로 설정합니다.

### 사용할 수 있는 컨텍스트

키 바인딩은 특정 UI 컨텍스트 범위에서 동작합니다.

| 컨텍스트 | 주요 동작 |
|----------|-----------|
| **Chat** | `submit`, `cancel`, `cycleMode`, `modelPicker`, `thinkingToggle`, `undo`, `externalEditor`, `stash`, `imagePaste` |
| **Confirmation** | `yes`, `no`, `previous`, `next`, `nextField`, `cycleMode`, `toggleExplanation` |
| **Global** | `interrupt`, `exit`, `toggleTodos`, `toggleTranscript` |
| **Autocomplete** | `accept`, `dismiss`, `next`, `previous` |
| **HistorySearch** | `search`, `previous`, `next` |
| **Settings** | 설정 화면 탐색 |
| **Tabs** | 탭 전환과 관리 |
| **Help** | 도움말 패널 탐색 |

`Transcript`, `Task`, `ThemePicker`, `Attachments`, `Footer`, `MessageSelector`, `DiffDialog`, `ModelPicker`, `Select`를 포함해 컨텍스트는 모두 18개입니다.

### 코드(chord) 지원

키 바인딩은 여러 키를 이어 누르는 코드 시퀀스를 지원합니다.

```text
"ctrl+k ctrl+s"   → 두 키 시퀀스: ctrl+k를 누른 뒤 ctrl+s
"ctrl+shift+p"    → 수정 키를 동시에 누름
```

**키 입력 문법**:
- **수정 키**: `ctrl`(또는 `control`), `alt`(또는 `opt`, `option`, `meta`), `shift`, `cmd`(또는 `command`, `super`, `win`). `cmd`는 `meta`의 별칭이 아닌 별도 수정 키이며, Super 키를 보고하는 터미널에서만 감지됩니다
- **대문자는 Shift 포함**: `K`는 `shift+k`와 같음
- **특수 키**: `escape`, `enter`, `return`, `tab`, `space`, `backspace`, `delete`, 화살표 키

### 예약된 키와 충돌하는 키

| 키 | 상태 | 참고 |
|----|------|------|
| `Ctrl+C` | 예약됨 | 다시 바인딩할 수 없음(중단) |
| `Ctrl+D` | 예약됨 | 다시 바인딩할 수 없음(종료) |
| `Ctrl+B` | 터미널 충돌 | tmux 접두 키 |
| `Ctrl+A` | 터미널 충돌 | GNU Screen 접두 키 |
| `Ctrl+Z` | 터미널 충돌 | 프로세스 일시 중지 |

> **팁**: 단축키가 동작하지 않으면 터미널 에뮬레이터나 멀티플렉서와 충돌하는지 확인하세요.

### 탭 자동 완성

Claude Code는 똑똑한 탭 자동 완성을 제공합니다.

```text
User: /rew<TAB>
→ /rewind

User: /plu<TAB>
→ /plugin

User: /plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```

### 명령 기록

이전 명령을 불러옵니다.

```text
User: <↑>  # 이전 명령
User: <↓>  # 다음 명령
User: Ctrl+R  # 기록 검색

(reverse-i-search)`test': run all tests
```

### 여러 줄 입력

복잡한 질문은 `\` + Enter, Option+Enter(macOS), Shift+Enter, Ctrl+J로 새 줄을 시작합니다. `\end` 같은 종료 표시는 없으며, Enter를 누르면 보냅니다.

```bash
User: Long complex prompt \
spanning multiple lines
```

**예시:**

```text
User: Implement a user authentication system \
with the following requirements: \
- JWT tokens \
- Email verification \
- Password reset \
- 2FA support

Claude: [여러 줄 요청을 처리]
```

### 인라인 편집

보내기 전에 명령을 고칩니다.

```text
User: Deploy to prodcution<Backspace><Backspace>uction

[보내기 전에 그 자리에서 편집]
```

### Vim 모드

텍스트 편집에 Vi/Vim 키 바인딩을 씁니다.

**켜기**:
- `/config`("Editor / Vim mode" 전환)나 `~/.claude/settings.json`의 `editorMode: "vim"`으로 켭니다. 별도 `/vim` 슬래시 명령은 제거되었으며([issue #43370](https://github.com/anthropics/claude-code/issues/43370) 참고), 이제 vim 모드는 설정으로 켭니다.
- `Esc`로 NORMAL, `i/a/o`로 INSERT, `v`로 VISUAL, `V`로 VISUAL-LINE 모드 전환(v2.1.118+)

**이동 키**:
- `h` / `l` - 왼쪽/오른쪽 이동
- `j` / `k` - 아래/위 이동
- `w` / `b` / `e` - 단어 단위 이동
- `0` / `$` - 줄 처음/끝으로 이동
- `gg` / `G` - 텍스트 처음/끝으로 이동
- `%` - 짝이 맞는 괄호로 이동. `d`, `c`, `y`와 함께 사용(v2.1.301)

**텍스트 객체**:
- `iw` / `aw` - 단어 안/둘레
- `i"` / `a"` - 따옴표 문자열 안/둘레
- `i(` / `a(` - 괄호 안/둘레

**비주얼 모드(v2.1.118+)**:

| 키 | 모드 | 동작 |
|----|------|------|
| `v` | Visual | 글자 단위 선택, 화면에 표시. 이동 키로 범위 확장 |
| `V` | Visual-line | 줄 단위 선택. 항상 줄 전체를 선택 |
| `y` | Yank | 현재 비주얼 선택 복사 |
| `d` / `x` | Delete | 현재 비주얼 선택 삭제 |
| `c` | Change | 선택을 지우고 INSERT 모드로 |
| `Esc` | Exit | NORMAL 모드로 돌아감 |

비주얼 선택은 입력란에 강조 표시되므로, 연산을 적용하기 전에 무엇이 복사·삭제·변경될지 정확히 볼 수 있습니다.

### Bash 모드

`!` 접두사로 셸 명령을 바로 실행합니다.

```bash
! npm test
! git status
! cat src/index.js
```

컨텍스트를 바꾸지 않고 명령을 빠르게 실행할 때 씁니다.

**v2.1.193부터:** bash 모드(`!`)에서 파일 경로가 실시간으로 자동 완성되므로, 프롬프트를 떠나지 않고 셸 명령을 입력하면서 경로를 완성할 수 있습니다.

**v2.1.186부터:** `!` 명령의 출력이 이제 자동으로 Claude에게 전달되고 Claude가 응답합니다. 출력을 컨텍스트에만 넣고 응답은 받지 않던 예전 동작을 유지하려면 `settings.json`에 `"respondToBashCommands": false`를 설정합니다.

---

## 출력 스타일

출력 스타일(output styles)은 Claude가 아는 것이 아니라 **어떻게** 응답하는지를 바꿉니다. 시스템 프롬프트를 바꿔 역할, 어조, 기본 응답 형식을 정합니다. 매 턴 같은 말투를 다시 요청하고 있거나, Claude가 소프트웨어 엔지니어가 아닌 다른 역할을 맡기를 원할 때 씁니다.

프로젝트나 코드베이스에 관한 지침은 대신 [CLAUDE.md](../02-memory/)를 씁니다. 트레이드오프가 다른 별개의 장치입니다.

### 내장 스타일

| 스타일 | 동작 |
|--------|------|
| **Default** | 소프트웨어 엔지니어링 작업을 효율적으로 끝내도록 맞춘 표준 시스템 프롬프트 |
| **Proactive** | 일상적인 결정에서 멈추지 않고 합리적으로 가정하며 바로 실행. 자동 모드보다 강한 자율 실행 지침이지만 권한 모드는 **바꾸지 않으므로** 권한 프롬프트는 계속 보임 |
| **Explanatory** | 단계 사이에 교육용 "Insights"를 넣어 구현 선택과 코드베이스 패턴을 설명 |
| **Learning** | 함께 해 보며 배우는 협업 방식. Claude가 통찰을 공유하고, 사용자가 작고 중요한 부분을 직접 구현하도록 `TODO(human)` 표시도 남김 |
| **Concise** (v2.1.237) | 결과부터 말하고 서론과 진행 설명을 생략. 철저함은 그대로이며 답을 둘러싼 말만 줄임. `/config` → Output style에서 고르거나 `"outputStyle": "Concise"`로 설정 |

### 스타일 고르기

`/config`에서 **Output style**을 고르거나 `/output-style <style>`로 바로 바꿉니다. 인수 없이 실행하면 쓸 수 있는 스타일을 나열하고 현재 스타일을 표시합니다. 어느 쪽이든 선택은 `.claude/settings.local.json`에 저장됩니다. 메뉴 없이 정하려면 설정을 직접 편집합니다.

```json
{
  "outputStyle": "Explanatory"
}
```

> **참고**: 별도 `/output-style` 명령은 v2.1.73에서 지원 중단되고 v2.1.91에서 제거되었지만 **v2.1.269에서 돌아왔으며** v2.1.278에서도 동작합니다. 헤드리스와 원격 제어 세션에서도 동작합니다. `/config`와 `outputStyle` 설정도 계속 쓸 수 있습니다.

출력 스타일은 시스템 프롬프트의 일부이고, Claude Code는 시스템 프롬프트를 세션 시작 때 한 번 읽습니다. 그래서 변경은 `/clear` 뒤나 새 세션에서 적용됩니다.

### 사용자 지정 출력 스타일

사용자 지정 스타일은 프런트매터가 있는 마크다운 파일로, 세 수준 중 한 곳에 저장합니다.

- 사용자: `~/.claude/output-styles/`
- 프로젝트: `.claude/output-styles/`
- 관리형 정책: 관리형 설정 디렉터리 안의 `.claude/output-styles/`

프로젝트 스타일은 작업 디렉터리와 저장소 루트 사이의 모든 `.claude/output-styles/`에서 불러옵니다. v2.1.178부터 중첩 디렉터리에서 같은 스타일 이름을 정의하면 작업 디렉터리에 가장 가까운 것이 이깁니다.

```markdown
---
name: Diagrams first
description: 모든 설명을 다이어그램으로 시작합니다
keep-coding-instructions: true
---

코드, 아키텍처, 데이터 흐름을 설명할 때는 구조를 보여 주는 Mermaid 다이어그램으로
시작하고, 그다음 글로 설명합니다.
```

| 프런트매터 | 용도 | 기본값 |
|------------|------|--------|
| `name` | 파일 이름과 다를 때의 스타일 이름 | 파일 이름을 따름 |
| `description` | `/config` 선택기에 표시 | 없음 |
| `keep-coding-instructions` | Claude Code의 내장 소프트웨어 엔지니어링 지침 유지 | `false` |
| `force-for-plugin` | 플러그인 스타일 전용: 플러그인이 켜져 있으면 사용자의 `outputStyle`을 무시하고 자동 적용 | `false` |

Claude의 소통 방식만 바꾸고 코딩 방식은 그대로 두려면 **`keep-coding-instructions: true`를 설정**합니다. 글쓰기 도우미나 데이터 분석가처럼 Claude가 소프트웨어 엔지니어링을 전혀 하지 않을 때는 뺍니다.

### 적용 범위와 비용

출력 스타일은 **메인 대화에만** 적용됩니다. 서브에이전트는 자체 시스템 프롬프트로 실행되므로 스타일이 서브에이전트의 응답을 바꾸지 않습니다. 단, 포크는 부모의 시스템 프롬프트 전체를 이어받으므로 예외입니다.

지침을 더하면 입력 토큰이 늘지만, 첫 요청 뒤에는 프롬프트 캐싱이 대부분을 흡수합니다. Explanatory와 Learning은 원래 응답이 길어서 출력 토큰이 늘어납니다.

### 비교

| 기능 | 동작 방식 | 쓸 때 |
|------|-----------|-------|
| 출력 스타일 | 시스템 프롬프트를 바꿈 | 매 턴 다른 역할, 어조, 형식을 원할 때 |
| [CLAUDE.md](../02-memory/) | 시스템 프롬프트 뒤에 사용자 메시지를 더함 | Claude가 프로젝트 규칙을 항상 알아야 할 때 |
| `--append-system-prompt` | 아무것도 빼지 않고 시스템 프롬프트 뒤에 덧붙임 | 한 번 호출할 때만 덧붙일 때 |
| [서브에이전트](../04-subagents/) | 자체 시스템 프롬프트, 모델, 도구로 실행 | 범위가 따로 정해진 도우미가 필요할 때 |
| [스킬](../03-skills/) | 호출될 때 작업 전용 지침을 불러옴 | 재사용할 워크플로가 있을 때 |

---

## 상태 줄

상태 줄(status line)은 출력이 세션 아래쪽에 표시되는 사용자 지정 명령입니다. `/statusline`으로 설정하거나 직접 지정합니다.

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh",
    "padding": 0
  }
}
```

`padding`의 기본값은 `0`입니다. Claude Code가 명령의 stdin으로 JSON 객체를 넘기므로, 무엇을 보여 줄지는 스크립트가 정합니다.

### 사용할 수 있는 입력 필드

| 묶음 | 필드 |
|------|------|
| 세션 | `session_id`, `session_name`, `prompt_id`, `transcript_path`, `cwd`, `version` |
| 모델 | `model.id`, `output_style.name`, `effort.level`, `fast_mode`, `thinking.enabled` |
| 에이전트 | `agent.name`, `vim.mode` |
| 비용 | `cost.total_cost_usd`, `cost.total_duration_ms`, `cost.total_api_duration_ms`, `cost.total_lines_added` |
| 컨텍스트 | `context_window.context_window_size`, `.current_usage`, `.remaining_percentage`, `.total_input_tokens`, `.used_percentage` |
| 한도 | `rate_limits.five_hour.used_percentage`, `.resets_at` |
| 저장소 | `pr.number`, `pr.review_state`, `workspace.project_dir`, `workspace.added_dirs`, `workspace.git_worktree`, `workspace.repo.host` |
| 워크트리 | `worktree.name`, `.branch`, `.path`, `.original_branch`, `.original_cwd` |

### 예시

```bash
#!/bin/bash
# ~/.claude/statusline.sh — 모델, 컨텍스트 사용량, 비용
input=$(cat)
model=$(echo "$input" | jq -r '.model.id')
used=$(echo "$input" | jq -r '.context_window.used_percentage')
cost=$(echo "$input" | jq -r '.cost.total_cost_usd')
printf '%s | ctx %.0f%% | $%.2f' "$model" "$used" "$cost"
```

> **참고**: `statusLine`에는 작업 공간 신뢰가 필요합니다. 상태 줄 스크립트는 환경에서 `COLUMNS`와 `LINES`도 받으므로(v2.1.153+) 출력을 터미널 크기에 맞출 수 있습니다.

---

## TUI 모드 (전체 화면)

> **v2.1.110 신규**

TUI(Text User Interface) 모드는 Claude Code를 깜빡임 없는 전체 화면으로 렌더링합니다. tmux나 iTerm2 분할 창 같은 터미널 멀티플렉서에 알맞습니다.

### TUI 모드 켜기

`/tui` 명령으로 렌더러를 바꾸거나, 실행 전에 `CLAUDE_CODE_NO_FLICKER=1`을 설정합니다.

```bash
/tui fullscreen               # 세션 안에서 전체 화면으로 전환
/tui default                  # 기존 렌더러로 돌아가기
CLAUDE_CODE_NO_FLICKER=1 claude  # 바로 전체 화면으로 시작
```

### 설정

| 설정 | 설명 | 기본값 |
|------|------|--------|
| `autoScrollEnabled` | 최신 메시지로 자동 스크롤 | `true` |

`/config`나 `settings.json`에서 자동 스크롤을 끕니다.

```json
{
  "autoScrollEnabled": false
}
```

### 포커스 보기

`/focus` 명령은 포커스 보기를 전환합니다. 가장 관련 있는 출력만 보여 주는 방해 없는 화면입니다. 이제 `Ctrl+O`는 일반 대화 기록과 상세 대화 기록 사이만 전환합니다(포커스 보기는 `/focus`).

---

## 음성 받아쓰기

음성 받아쓰기는 Claude Code에 눌러서 말하기(push-to-talk) 음성 입력을 제공해, 프롬프트를 입력하는 대신 말로 할 수 있게 합니다.

### 음성 받아쓰기 켜기

```text
/voice
```

### 기능

| 기능 | 설명 |
|------|------|
| **눌러서 말하기** | 키를 누르고 있으면 녹음, 떼면 보냄 |
| **20개 언어** | 음성 인식(STT)이 20개 언어 지원 |
| **사용자 지정 키 바인딩** | `/keybindings`로 눌러서 말하기 키 설정 |
| **계정 요구 사항** | STT 처리에 Claude.ai 계정 필요 |

### 설정

키 바인딩 파일(`/keybindings`)에서 눌러서 말하기 키를 바꿉니다. 음성 받아쓰기는 음성 인식 처리에 Claude.ai 계정을 씁니다.

---

## 채널

채널(Channels)은 외부 서비스의 이벤트를 MCP 서버를 통해 실행 중인 Claude Code 세션으로 보내는 리서치 프리뷰 기능입니다. Telegram, Discord, iMessage, 임의의 웹훅을 출처로 쓸 수 있어, Claude가 폴링 없이 실시간 알림에 반응할 수 있습니다.

> **인증(v2.1.128+)**: 이제 `--channels`는 Pro/Max OAuth **와** API 키(콘솔) 인증 모두에서 동작합니다. 이전 릴리스는 OAuth가 필요했습니다.

### 채널 구독

```bash
# 시작할 때 채널 플러그인 구독
claude --channels plugin:telegram@claude-plugins-official

# 여러 출처 구독 (공백으로 구분)
claude --channels plugin:discord@claude-plugins-official plugin:telegram@claude-plugins-official
```

### 지원하는 연동

| 연동 | 설명 |
|------|------|
| **Discord** | 세션에서 Discord 메시지를 받고 응답 |
| **Telegram** | 세션에서 Telegram 메시지를 받고 응답 |
| **iMessage** | 세션에서 iMessage 알림 받기 |
| **Webhooks** | 임의의 웹훅 출처에서 이벤트 받기 |

### 설정

시작할 때 `--channels` 플래그로 채널을 설정합니다. 엔터프라이즈 배포에서는 관리형 설정으로 허용할 채널 플러그인을 정합니다.

```json
{
  "channelsEnabled": true,
  "allowedChannelPlugins": [
    { "marketplace": "claude-plugins-official", "plugin": "discord" },
    { "marketplace": "claude-plugins-official", "plugin": "telegram" }
  ]
}
```

`allowedChannelPlugins` 관리형 설정은 조직 전체에서 허용할 채널 플러그인을 정합니다. Anthropic이 관리하는 허용 목록을 대체하며 `channelsEnabled: true`가 필요합니다.

### 동작 방식

1. MCP 서버가 외부 서비스에 연결하는 채널 플러그인 역할을 함
2. 들어오는 메시지와 이벤트를 활성 Claude Code 세션으로 보냄
3. Claude는 세션 컨텍스트 안에서 메시지를 읽고 응답할 수 있음
4. 채널 플러그인은 Anthropic이 관리하는 허용 목록에 있거나, 관리자가 정했다면 조직의 `allowedChannelPlugins` 목록에 있어야 함
5. 폴링 필요 없음 — 이벤트가 실시간으로 전달됨

---

## Chrome 연동

Chrome 연동은 Claude Code를 Chrome이나 Microsoft Edge 브라우저에 연결해 실시간 웹 자동화와 디버깅을 합니다. v2.0.73+부터 쓸 수 있는 베타 기능입니다(Claude in Chrome 확장 v1.0.36+ 필요).

### Chrome 연동 켜기

**시작할 때**:

```bash
claude --chrome      # Chrome 연결 켜기
claude --no-chrome   # Chrome 연결 끄기
```

**세션 안에서**:

```text
/chrome
```

"Enabled by default"를 고르면 앞으로 모든 세션에서 Chrome 연동이 켜집니다. Claude Code는 브라우저의 로그인 상태를 공유하므로 인증이 필요한 웹 앱과도 상호 작용할 수 있습니다.

> **참고**: v2.1.290부터 프로젝트 설정 파일로는 Chrome 연동을 켤 수 없습니다. `--chrome`, `/chrome`, 사용자 설정을 쓰세요.

### 기능

| 기능 | 설명 |
|------|------|
| **실시간 디버깅** | 콘솔 로그 읽기, DOM 요소 검사, JavaScript 실시간 디버깅 |
| **디자인 검증** | 렌더링된 페이지를 디자인 시안과 비교 |
| **폼 검증** | 폼 제출, 입력 검증, 오류 처리 테스트 |
| **웹 앱 테스트** | 인증이 필요한 앱(Gmail, Google Docs, Notion 등)과 상호 작용 |
| **데이터 추출** | 웹 페이지의 내용을 수집하고 처리 |
| **세션 녹화** | 브라우저 상호 작용을 GIF 파일로 녹화 |

### 사이트별 권한

Chrome 확장이 사이트별 접근을 관리합니다. 확장 팝업에서 언제든 특정 사이트의 접근을 허용하거나 취소합니다. Claude Code는 사용자가 명시적으로 허용한 사이트와만 상호 작용합니다.

### 동작 방식

Claude Code는 보이는 창에서 브라우저를 제어하므로 동작을 실시간으로 지켜볼 수 있습니다. 브라우저가 로그인 페이지나 CAPTCHA를 만나면 Claude가 멈추고, 사용자가 직접 처리할 때까지 기다린 뒤 계속합니다.

### 알려진 한계

- **브라우저 지원**: Chrome과 Edge. Brave, Arc, Vivaldi, Opera 같은 다른 Chromium 기반 브라우저에도 연결됨
- **WSL**: Windows Subsystem for Linux에서는 사용 불가
- **서드파티 제공자**: Bedrock, Vertex, Foundry API 제공자에서는 지원하지 않음
- **서비스 워커 유휴**: 긴 세션 동안 Chrome 확장 서비스 워커가 유휴 상태가 될 수 있음

> **팁**: Chrome 연동은 베타 기능입니다. 이후 릴리스에서 지원 브라우저가 늘어날 수 있습니다.

---

## 원격 제어

원격 제어(Remote Control)를 쓰면 로컬에서 실행 중인 Claude Code 세션을 휴대폰, 태블릿, 아무 브라우저에서 이어 갈 수 있습니다. 로컬 세션은 계속 자신의 컴퓨터에서 실행되며, 클라우드로 옮겨 가는 것은 없습니다. Pro, Max, Team, Enterprise 요금제에서 쓸 수 있습니다(v2.1.51+). Enterprise 사용자 지정 역할로 접근을 관리한다면, 클라우드 세션 권한과 **Claude Code remote control** 권한을 모두 가진 역할이 필요합니다.

원격 제어는 **더 이상 리서치 프리뷰가 아닙니다**. 2026년 34주 차에 그 표시가 빠졌습니다. 이제 `claude remote-control`을 실행 중인 모든 컴퓨터가 Claude 앱 Code 탭에 **기기 카드**로 나타나므로, 그 컴퓨터에서 먼저 세션을 시작하고 연결할 필요 없이 휴대폰에서 바로 그 컴퓨터의 세션을 시작할 수 있습니다.

### 원격 제어 시작하기

**CLI에서**:

```bash
# 기본 세션 이름으로 시작
claude remote-control

# 사용자 지정 이름으로 시작
claude remote-control --name "Auth Refactor"
```

**세션 안에서**:

```text
/remote-control
/remote-control "Auth Refactor"
```

**사용할 수 있는 플래그**:

| 플래그 | 설명 |
|--------|------|
| `--name "title"` | 알아보기 쉬운 사용자 지정 세션 제목 |
| `--verbose` | 자세한 연결 로그 표시 |

> **참고(v2.1.300)**: `claude remote-control`은 더 이상 `--sandbox`나 `--no-sandbox`를 받지 않습니다(원래 효과가 없었고, 이제 넘기면 오류로 종료). 원격 제어 세션을 샌드박스로 실행하려면 설정 파일에 `sandbox.enabled`를 설정합니다.

### 세션에 연결하기

다른 기기에서 연결하는 방법은 세 가지입니다.

1. **세션 URL** — 세션이 시작될 때 터미널에 출력됨. 아무 브라우저에서 엶
2. **QR 코드** — 시작한 뒤 `spacebar`를 누르면 스캔할 수 있는 QR 코드 표시
3. **이름으로 찾기** — claude.ai/code나 Claude 모바일 앱(iOS/Android)에서 세션 둘러보기

### 보안

- 컴퓨터에 **들어오는 포트를 열지 않음**
- TLS를 쓰는 **나가는 HTTPS만** 사용
- **범위가 제한된 자격 증명** — 수명이 짧고 범위가 좁은 토큰 여러 개
- **세션 격리** — 원격 세션은 서로 독립적

### 원격 제어와 웹의 Claude Code 비교

| 항목 | 원격 제어 | 웹의 Claude Code |
|------|-----------|------------------|
| **실행 위치** | 사용자 컴퓨터 | Anthropic 클라우드 |
| **로컬 도구** | 로컬 MCP 서버, 파일, CLI에 모두 접근 | 로컬 의존성 없음 |
| **사용 사례** | 다른 기기에서 로컬 작업 이어 가기 | 아무 브라우저에서 새로 시작 |

### 한계

- 대화형 Claude Code 프로세스 하나당 원격 세션 하나. 서버 모드(`claude remote-control`)는 여러 개를 동시에 처리(`--capacity`, 기본값 32)
- 호스트 컴퓨터에서 터미널이 열려 있어야 함
- 네트워크에 연결할 수 없으면 서버 모드는 약 10분 뒤 종료. 대화형 세션은 계속 재시도하다가 네트워크가 돌아오면 다시 연결

### 사용 사례

- 자리를 비운 동안 모바일 기기나 태블릿에서 Claude Code 제어
- 로컬 도구 실행은 유지하면서 더 풍부한 claude.ai UI 사용
- 이동 중에도 전체 로컬 개발 환경으로 빠르게 코드 리뷰

### 푸시 알림 (v2.1.110)

원격 제어가 켜져 있고 `/config`에서 "Push when Claude decides"를 켜면, 긴 작업이 끝나거나 입력이 필요할 때처럼 Claude가 휴대폰에 모바일 푸시 알림을 보낼 수 있습니다.

켜는 방법:
1. 원격 제어 시작: `/remote-control` 또는 `claude --rc`
2. `/config`를 열고 **Push when Claude decides** 켜기

푸시 알림에는 Claude 구독과 Claude 모바일 앱이 필요합니다.

### 원격 제어 끄기 (`disableRemoteControl`, v2.1.128+)

Team이나 Enterprise 요금제의 관리자는 `disableRemoteControl` 설정으로 원격 제어를 완전히 막을 수 있습니다. `true`면 `claude remote-control`과 `/remote-control` 모두 시작을 거부합니다.

```json
{
  "disableRemoteControl": true
}
```

이 설정은 **관리형/정책** 범위(예: macOS의 `/Library/Application Support/ClaudeCode/managed-settings.json`)에서 적용되므로 개별 사용자가 재정의할 수 없습니다. 조직 전체에서 로컬 실행만 허용해야 할 때 유용합니다.

> **API 키 사용 시 원격 제어 자동 비활성화(v2.1.139)**: 다음 중 하나라도 설정되어 있으면, claude.ai로 함께 로그인해 있어도 원격 제어가 **아무 말 없이 꺼집니다**.
>
> - `ANTHROPIC_API_KEY`
> - `ANTHROPIC_AUTH_TOKEN`
> - `apiKeyHelper` (settings.json)
>
> 같은 조건에서 [`/schedule`](#예약-태스크), claude.ai MCP 커넥터, 알림 설정도 꺼집니다. claude.ai와 연결된 이 네 가지 기능은 모두 OAuth 로그인이 활성 자격 증명이어야 동작합니다. 이 기능들을 쓰려면 API 키 설정을 지우세요(또는 Pro/Max OAuth로 실행).

---

## 웹 세션

웹 세션을 쓰면 claude.ai/code에서 브라우저로 Claude Code를 바로 실행하거나, CLI에서 웹 세션을 만들 수 있습니다.

### 웹 세션 만들기

```bash
# CLI에서 새 웹 세션 만들기
claude --cloud "implement the new API endpoints"   # --remote는 지원 중단된 별칭
```

claude.ai에서 Claude Code 세션이 시작되어 아무 브라우저에서나 접근할 수 있습니다.

### 웹 세션을 로컬에서 재개하기

웹에서 시작한 세션을 로컬에서 이어 가려면:

```bash
# 로컬 터미널에서 웹 세션 재개 — 웹 세션 선택기가 열림
claude --teleport
```

또는 대화형 REPL 안에서:

```text
/teleport
```

`/tp`는 `/teleport`의 별칭입니다. 둘 다 claude.ai 구독이 필요합니다. 클라우드 세션에는
로컬에서 이어 가는 방법을 알려 주는 `/teleport` 힌트가 표시됩니다(v2.1.223).

> **변경 기록 기준**: v2.1.223 변경 기록에는 알고 있는 세션으로 바로 이동하는 인수 형태
> `claude --teleport <session id>`가 나옵니다. CLI 레퍼런스에는 인수 없는 선택기 형태만
> 문서화되어 있으므로, 세션 ID를 이미 알고 있지 않다면 `claude --teleport`를 쓰세요.

### 사용 사례

- 한 컴퓨터에서 작업을 시작하고 다른 컴퓨터에서 이어 가기
- 팀원과 세션 URL 공유
- 웹 UI로 시각적 diff를 검토한 뒤 터미널로 옮겨 실행

---

## 데스크톱 앱

Claude Code 데스크톱 앱은 시각적 diff 검토, 병렬 세션, 내장 커넥터를 갖춘 독립 애플리케이션입니다. macOS와 Windows에서 쓸 수 있습니다(Pro, Max, Team, Enterprise 요금제).

### 설치

플랫폼에 맞는 앱을 [claude.ai](https://claude.ai)에서 내려받습니다.
- **macOS**: 유니버설 빌드(Apple Silicon과 Intel)
- **Windows**: x64와 ARM64 설치 파일

설정 방법은 [Desktop Quickstart](https://code.claude.com/docs/en/desktop-quickstart)를 참고하세요.

### CLI에서 넘기기

현재 CLI 세션을 데스크톱 앱으로 넘깁니다.

```text
/desktop
```

### 핵심 기능

| 기능 | 설명 |
|------|------|
| **Diff 보기** | 파일별 시각적 검토와 인라인 댓글. Claude가 댓글을 읽고 수정 |
| **앱 미리 보기** | 개발 서버를 자동으로 시작하고 내장 브라우저로 실시간 확인 |
| **PR 모니터링** | **Auto-fix**는 실패한 CI 검사와 리뷰 댓글을 Claude가 고치게 하고, **Merge when ready**는 GitHub 자동 병합을 켬(베이스 브랜치가 병합 큐를 쓰지 않으면 로컬 세션에서는 squash). 둘 다 세션 **Changes** 카드의 풀 리퀘스트 **⋮** 메뉴에 있음 |
| **병렬 세션** | 사이드바에 여러 세션, Git 워크트리로 자동 격리 |
| **예약 태스크** | 앱이 열려 있는 동안 실행되는 반복 태스크(매시, 매일, 평일, 매주) |
| **풍부한 렌더링** | 코드, 마크다운, 다이어그램을 구문 강조와 함께 렌더링. GitHub 마크다운 태스크 목록 체크박스(`- [ ]` / `- [x]`)를 체크박스로 표시(v2.1.149+) |

### 앱 미리 보기 설정

`.claude/launch.json`에서 개발 서버 동작을 설정합니다.

```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "my-app",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["run", "dev"],
      "port": 3000
    }
  ]
}
```

### 커넥터

외부 서비스를 연결해 컨텍스트를 풍부하게 합니다.

| 커넥터 | 기능 |
|--------|------|
| **GitHub** | PR 모니터링, 이슈 추적, 코드 리뷰 |
| **Slack** | 알림, 채널 컨텍스트 |
| **Linear** | 이슈 추적, 스프린트 관리 |
| **Notion** | 문서, 지식 베이스 접근 |
| **Asana** | 태스크 관리, 프로젝트 추적 |
| **Calendar** | 일정 파악, 회의 컨텍스트 |

> **참고**: 원격(클라우드) 세션에서는 커넥터를 쓸 수 없습니다.

### 원격 세션과 SSH 세션

- **원격 세션**: Anthropic 클라우드 인프라에서 실행되며 앱을 닫아도 계속됨. claude.ai/code나 Claude 모바일 앱에서 접근
- **SSH 세션**: SSH로 원격 컴퓨터에 연결해 원격 파일 시스템과 도구에 모두 접근. 원격 컴퓨터에 Claude Code가 설치되어 있어야 함

### 데스크톱의 권한 모드

데스크톱 앱은 CLI와 같은 권한 모드를 지원합니다.

| 모드 | 동작 |
|------|------|
| **Manual**(기본값) | 모든 편집과 명령을 검토하고 승인 |
| **Accept edits** | 파일 편집은 자동 승인, 명령은 수동 승인 |
| **Plan** | 변경 전에 접근 방식 검토 |
| **Auto** | 일상적인 프롬프트 없이 실행. 백그라운드 분류기가 먼저 동작을 검사 |
| **Bypass permissions** | 자동 실행(샌드박스 전용, 관리자 제어) |

### 엔터프라이즈 기능

- **관리 콘솔**: 조직의 Code 탭 접근과 권한 설정 제어
- **MDM 배포**: macOS는 MDM, Windows는 MSIX로 배포
- **SSO 연동**: 조직 구성원에게 SSO(Single Sign-On) 요구
- **관리형 설정**: 팀 설정과 사용 가능한 모델을 중앙에서 관리

---

## 태스크 목록

태스크 목록 기능은 컨텍스트 압축(대화 기록을 컨텍스트 윈도에 맞게 줄이는 것) 뒤에도 남는 영구 태스크 추적을 제공합니다.

### 태스크 목록 전환

세션 중 `Ctrl+T`를 누르면 태스크 목록 보기를 켜고 끕니다.

### 영구 태스크

태스크는 컨텍스트 압축 뒤에도 유지되므로, 대화 컨텍스트가 줄어도 오래 걸리는 작업 항목을 잃지 않습니다. 여러 단계로 된 복잡한 구현에 특히 유용합니다.

### 이름 붙인 태스크 디렉터리

`CLAUDE_CODE_TASK_LIST_ID` 환경 변수로 세션끼리 공유하는 이름 붙인 태스크 디렉터리를 만듭니다.

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

여러 세션이 같은 태스크 목록을 공유하므로 팀 워크플로나 여러 세션에 걸친 프로젝트에 유용합니다.

---

## 프롬프트 제안

프롬프트 제안은 git 기록과 현재 대화 컨텍스트를 바탕으로 회색 예시 명령을 보여 줍니다.

### 동작 방식

- 입력 프롬프트 아래에 회색 텍스트로 제안이 나타남
- `Tab`을 누르면 제안을 수락
- `Enter`를 누르면 수락하고 바로 제출
- 제안은 git 기록과 대화 상태를 바탕으로 컨텍스트에 맞게 나옴

### 프롬프트 제안 끄기

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git 워크트리

Git 워크트리를 쓰면 격리된 워크트리에서 Claude Code를 시작해, stash나 브랜치 전환 없이 여러 브랜치에서 병렬로 작업할 수 있습니다.

### 워크트리에서 시작하기

```bash
# 격리된 워크트리에서 Claude Code 시작
claude --worktree
# 또는
claude -w
```

### 워크트리 위치

워크트리는 다음 위치에 만들어집니다.
```text
<repo>/.claude/worktrees/<name>
```

### 모노레포를 위한 스파스 체크아웃

`worktree.sparsePaths` 설정으로 모노레포에서 스파스 체크아웃을 해 디스크 사용량과 클론 시간을 줄입니다.

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### 베이스 브랜치 기준 (`worktree.baseRef`)

**`worktree.baseRef`**(v2.1.133에서 추가)는 `claude --worktree`가 `origin/<default>`에서 브랜치를 만들지 로컬 `HEAD`에서 만들지 정합니다.

- `"fresh"`(기본값) — 푸시하지 않은 로컬 커밋을 무시하고 `origin/<default-branch>`에서 브랜치를 만듭니다. **v2.1.128에서 바뀐 동작을 되돌린 것**이라, v2.1.128 뒤로 로컬 HEAD 기준 브랜치에 기대던 사용자는 다시 직접 켜야 합니다.
- `"head"` — 푸시하지 않은 커밋을 유지한 채 로컬 `HEAD`에서 브랜치를 만듭니다.

`~/.claude/settings.json`에 설정합니다.

```json
{ "worktree": { "baseRef": "head" } }
```

### 백그라운드 세션 격리 (`worktree.bgIsolation`)

**`worktree.bgIsolation`**(v2.1.143에서 추가)은 백그라운드 세션(`/bg`, `claude --bg`, 에이전트 뷰 등)이 자체 워크트리를 가질지, 포그라운드 작업 사본을 직접 편집할지 정합니다.

- *(기본값)* — 백그라운드 세션이 `--worktree`처럼 `<repo>/.claude/worktrees/` 아래에 격리된 워크트리를 만듭니다.
- `"none"` — 백그라운드 세션이 현재 작업 사본을 직접 편집합니다. 워크트리가 비현실적이거나(예: 무거운 네이티브 빌드 산출물) 백그라운드 에이전트가 포그라운드 세션과 편집을 맞춰야 할 때 씁니다.

```json
{ "worktree": { "bgIsolation": "none" } }
```

트레이드오프: `"none"`은 워크트리 격리라는 안전망을 없앱니다. 백그라운드와 포그라운드 세션이 동시에 편집하면 작업 사본에서 병합 충돌이 생길 수 있습니다.

### 워크트리 도구와 훅

| 항목 | 설명 |
|------|------|
| `EnterWorktree` | 워크트리에 들어가는 도구. v2.1.157부터 세션 도중 Claude가 관리하는 워크트리 사이를 오갈 수 있음 |
| `ExitWorktree` | 현재 워크트리에서 나와 정리하는 도구 |
| `WorktreeCreate` | 워크트리를 만들 때 발생하는 훅 이벤트 |
| `WorktreeRemove` | 워크트리를 제거할 때 발생하는 훅 이벤트 |

v2.1.157부터 Claude가 관리하는 워크트리는 에이전트가 끝나면 잠기지 않은 채로 남으므로, `git worktree remove`/`prune`으로 정리할 수 있습니다.

### 자동 정리

대화형 워크트리 세션을 끝낼 때 워크트리에 변경이 없으면 Claude가 워크트리와 브랜치를 자동으로 지웁니다(이름 붙인 세션은 먼저 물어봄). 변경이나 새 커밋이 있거나, 새 커밋이 있는 다른 브랜치가 체크아웃되어 있으면(v2.1.304부터) 유지할지 지울지 묻습니다. 비대화형 `-p` 실행은 워크트리를 그대로 둡니다.

### 사용 사례

- main 브랜치는 건드리지 않고 기능 브랜치에서 작업
- 작업 디렉터리에 영향 없이 격리된 환경에서 테스트 실행
- 버려도 되는 환경에서 실험적 변경 시도
- 모노레포에서 특정 패키지만 스파스 체크아웃해 빠르게 시작

---

## 샌드박싱

샌드박싱은 Claude Code가 실행하는 Bash 명령에 OS 수준의 파일 시스템·네트워크 격리를 제공합니다. 권한 규칙을 보완하는 추가 보안 계층입니다.

### 샌드박싱 켜기

**슬래시 명령**(이 프로젝트의 `.claude/settings.local.json`에 선택을 저장):
```text
/sandbox
```

**설정 파일**(모든 프로젝트): `~/.claude/settings.json`에 `"sandbox": { "enabled": true }`를 설정합니다.

**세션 단위**로, 설정 파일을 쓰지 않고:
```bash
claude --settings '{"sandbox": {"enabled": true}}'
```

> `claude --sandbox` / `--no-sandbox` 플래그는 없습니다. v2.1.300부터 `claude remote-control`에 이 플래그를 넘기면 오류로 종료합니다.

### 설정 항목

| 설정 | 설명 |
|------|------|
| `sandbox.enabled` | 샌드박싱 켜기/끄기 |
| `sandbox.failIfUnavailable` | 샌드박싱을 켤 수 없으면 실패 |
| `sandbox.filesystem.allowWrite` | 쓰기를 허용할 경로 |
| `sandbox.filesystem.allowRead` | 읽기를 허용할 경로 |
| `sandbox.filesystem.denyRead` | 읽기를 거부할 경로 |
| `sandbox.network.allowedDomains` | Bash로 실행한 프로세스가 접속할 수 있는 도메인(`*.` 와일드카드 지원) |
| `sandbox.network.deniedDomains` | `allowedDomains` 와일드카드가 허용하더라도 막을 도메인(v2.1.113+) |
| `sandbox.network.strictAllowlist` | (v2.1.219) 샌드박스 명령이 허용 목록에 없는 호스트에 접속하면 묻지 않고 거부 |
| `sandbox.enableWeakerNetworkIsolation` | macOS에서 더 약한 네트워크 격리 사용 |
| `sandbox.bwrapPath` | (v2.1.133+, Linux/WSL) `bubblewrap` 바이너리 경로. 기본값: `$PATH`에서 찾음 |
| `sandbox.socatPath` | (v2.1.133+, Linux/WSL) `socat` 바이너리 경로. 기본값: `$PATH`에서 찾음 |
| `sandbox.credentials` | (v2.1.187+) 샌드박스 명령이 자격 증명 파일과 비밀 환경 변수를 읽지 못하게 막음 |
| `sandbox.allowAppleEvents` | (v2.1.181+, macOS) 샌드박스 명령이 Apple Events를 보내도록 허용(선택) |
| `sandbox.filesystem.disabled` | (v2.1.216+) 네트워크 격리는 유지한 채 파일 시스템 격리만 건너뜀. 파일 샌드박싱이 도구를 깨뜨리지만 네트워크 송신 제어는 유지해야 할 때 유용. 사용자 설정, 관리형 설정, `--settings`에서만 적용되며 프로젝트 설정으로는 지정 불가 |
| `sandbox.excludedCommands` | `"docker compose *"`처럼 `Bash(...)` 규칙 문법으로 쓴 명령 패턴. 샌드박스 밖에서 실행되고 일반 권한 흐름을 거침. v2.1.309부터 샌드박스가 켜져 있지만 실행할 수 없을 때 샌드박스 쓰기 목록 경로에 쓰기 전에 물어봄 |
| `sandbox.allowNestedUserNamespaces` | (v2.1.305+, Linux/WSL2) 샌드박스 명령이 사용자 네임스페이스를 만들도록 허용(Chromium/Electron 샌드박스, 중첩 bubblewrap, `unshare`). 보안 경계를 끄는 것이며 사용자 설정, 관리형 설정, `--settings`에서만 적용. 루트리스 Podman에는 대신 `excludedCommands`를 사용 |

**Linux/WSL 바이너리 경로**(v2.1.133+) — 표준이 아닌 설치 위치를 Claude Code에 알려 줍니다.

```json
{
  "sandbox": {
    "bwrapPath": "/opt/bubblewrap/bin/bwrap",
    "socatPath": "/opt/socat/bin/socat"
  }
}
```

넓은 와일드카드를 `deniedDomains`로 재정의하는 예시(v2.1.113+):

```json
{
  "sandbox": {
    "network": {
      "allowedDomains": ["*.example.com"],
      "deniedDomains": ["evil.example.com"]
    }
  }
}
```

와일드카드는 `example.com`의 모든 것을 통과시키지만, `deniedDomains`가 이름을 지정한 호스트는 여전히 막습니다.

> **참고**(v2.1.243): 샌드박스 Bash 도구의 권한 프롬프트는 **더 이상 허용된 네트워크 호스트를 나열하지 않습니다**. Claude가 그냥 요청을 시도하고, 새 호스트가 나올 때마다 사용자가 승인합니다. 그러니 프롬프트가 허용 목록을 미리 보여 주리라 기대하지 마세요. 같은 릴리스에서 막힌 명령이 `0`으로 끝나도 네트워크 위반 세부 정보를 버리지 않게 바뀌어, 겉보기에 조용한 성공도 무엇이 막혔는지 알려 줍니다.

### 자격 증명 가리기 (v2.1.221, v2.1.224)

> **변경 기록 기준**: 이 `sandbox.credentials` 옵션은 v2.1.221과 v2.1.224 변경 기록 항목에서
> 가져왔으며, 설정 레퍼런스에는 아직 자세히 나와 있지 않습니다.

v2.1.221 전에는 `sandbox.credentials`가 자격 증명 파일을 `deny`할 수만 있어서, 자격 증명이 필요한
샌드박스 명령은 그냥 실패했습니다. `mode: "mask"`는 비밀 값을 노출하지 않고도 명령이 동작하게 합니다.
샌드박스 프로세스는 파일의 **대체(sentinel)** 사본을 읽고, 샌드박스 프록시가 네트워크로 나갈 때
실제 값으로 바꿔 넣습니다.

```json
{
  "sandbox": {
    "network": { "tlsTerminate": true },
    "credentials": {
      "files": [
        { "path": "~/.aws/credentials", "mode": "mask" }
      ]
    }
  }
}
```

| 기능 | 도입 | 하는 일 |
|---|---|---|
| 자격 증명 **파일**의 `mode: "mask"` | v2.1.221 | 샌드박스 명령은 대체 값을 읽고, 프록시가 나갈 때 실제 값으로 바꿈. **Linux와 WSL 전용** — macOS에서는 파일 가리기가 `deny`로 대체됨 |
| `extract` / `onExtractNoMatch` | v2.1.224 | 구조화된 환경 변수 값에서 변수 전체가 아니라 필드 하나만 가리고, 패턴이 맞지 않을 때의 동작을 정함 |
| `maskClaims`를 쓴 `decode: "jwt"` | v2.1.224 | JWT를 디코드해 지정한 클레임만 가리고 나머지는 읽을 수 있게 둠 |
| `awsPairs` / `sigv4` | v2.1.224 | 실제 액세스 키로 바꾼 뒤 프록시에서 AWS SigV4 요청에 다시 서명 |

**놓치기 쉬운 제약 두 가지:**

- 모든 가리기에는 `network.tlsTerminate`가 필요합니다. 값을 바꾸려면 프록시가 요청 내부를
  볼 수 있어야 합니다.
- 이 옵션은 사용자 설정, 관리형 설정, `--settings`에서**만** 적용됩니다.
  프로젝트 설정으로는 가리기를 켜거나 가릴 대상을 바꿀 수 없습니다.

### 설정 예시

```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    }
  }
}
```

### 동작 방식

- Bash 명령은 파일 시스템 접근이 제한된 샌드박스 환경에서 실행
- 네트워크 접근을 격리해 의도치 않은 외부 연결을 막을 수 있음
- 권한 규칙과 함께 동작해 다층 방어
- macOS에서는 Seatbelt가 샌드박스 프록시로 가는 연결 외의 모든 네트워크 연결을 막음. `sandbox.enableWeakerNetworkIsolation`은 이 격리를 **약화**하며, MITM `httpProxyPort` 프록시 뒤의 Go CLI 같은 도구를 위한 것

### 사용 사례

- 신뢰할 수 없거나 생성된 코드를 안전하게 실행
- 프로젝트 밖 파일이 실수로 수정되는 것을 방지
- 자동화 작업 중 네트워크 접근 제한

---

## 관리형 설정 (엔터프라이즈)

관리형 설정(Managed Settings)을 쓰면 엔터프라이즈 관리자가 플랫폼 고유의 관리 도구로 조직 전체에 Claude Code 설정을 배포할 수 있습니다.

### 배포 방법

| 플랫폼 | 방법 | 도입 |
|--------|------|------|
| macOS | 관리형 plist 파일(MDM) | v2.1.51+ |
| Windows | Windows 레지스트리 | v2.1.51+ |
| 크로스 플랫폼 | 관리형 설정 파일 | v2.1.51+ |
| 크로스 플랫폼 | 관리형 드롭인(`managed-settings.d/` 디렉터리) | v2.1.83+ |

### 관리형 드롭인

v2.1.83부터 관리자는 여러 관리형 설정 파일을 `managed-settings.d/` 디렉터리에 배포할 수 있습니다. 파일은 알파벳 순으로 합쳐지므로 팀별로 설정을 모듈화할 수 있습니다. 이 디렉터리는 시스템 디렉터리(macOS는 `/Library/Application Support/ClaudeCode/`, Linux는 `/etc/claude-code/`, Windows는 `C:\Program Files\ClaudeCode\`)의 `managed-settings.json` 옆에 둡니다.

```text
/etc/claude-code/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 사용할 수 있는 관리형 설정

| 설정 | 설명 |
|------|------|
| `permissions.disableBypassPermissionsMode` | `"disable"`로 설정하면 사용자가 bypass permissions를 켤 수 없음 |
| `availableModels` | 사용자가 고를 수 있는 모델 제한 |
| `enforceAvailableModels` | (v2.1.175) `true`면 `availableModels` 허용 목록이 **기본** 모델까지 *함께* 제한. 설정된 기본 모델이 목록에 없으면 Claude Code는 허용된 첫 모델로 대체. 사용자와 프로젝트 설정은 더 이상 관리형 `availableModels` 목록을 넓힐 수 없음 |
| `allowedChannelPlugins` | 허용할 채널 플러그인 제어 |
| `autoMode.environment` | 자동 모드용 신뢰 인프라 설정 |
| `workflowSizeGuideline` | (v2.1.219) 권고용 [동적 워크플로 크기 지침](#동적-워크플로) 설정. 관리형 설정뿐 아니라 어느 설정 파일에서든 읽으며, 어디서든 설정하면 `/config`의 **Dynamic workflow size** 행이 숨겨짐 |
| `wslInheritsWindowsSettings` | Windows/WSL 전용(v2.1.118+): `true`면 WSL 안의 Claude Code가 Windows 호스트의 관리형 설정을 이어받아, 레지스트리/MDM으로 배포한 엔터프라이즈 정책이 Windows와 WSL 셸에 똑같이 적용 |
| `parentSettingsBehavior` | (v2.1.133+, 관리자 계층) SDK의 `managedSettings`가 부모 프로세스 설정과 합쳐지는 방식. `"first-wins"`는 기존 우선순위 유지(충돌 시 먼저 나온 설정이 이김), `"merge"`는 값을 깊게 병합 |
| 사용자 지정 정책 | 조직 전용 권한과 도구 정책 |

### 예시: macOS Plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>permissions</key>
  <dict>
    <key>disableBypassPermissionsMode</key>
    <string>disable</string>
  </dict>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```

---

## 구성과 설정

### 설정 파일 위치

1. **사용자 설정**: `~/.claude/settings.json`
2. **프로젝트 설정**: `.claude/settings.json`(커밋함)
3. **로컬 프로젝트 설정**: `.claude/settings.local.json`(커밋하지 않음)
4. **관리형 설정**: 시스템 디렉터리의 `managed-settings.json`([관리형 설정](#관리형-설정-엔터프라이즈) 참고)

### 전체 설정 예시

**핵심 고급 기능 설정**(`.claude/settings.json`):

```json
{
  "permissions": {
    "defaultMode": "manual"
  },
  "hooks": {
    "PreToolUse": [
      { "matcher": "Edit", "hooks": [{ "type": "command", "command": "~/.claude/hooks/eslint-fix.sh" }] }
    ],
    "PostToolUse": [
      { "matcher": "Write", "hooks": [{ "type": "command", "command": "~/.claude/hooks/security-scan.sh" }] }
    ]
  }
}
```

훅 스크립트는 도구 입력을 stdin의 JSON으로 받습니다(예: `tool_input.file_path`). `${file_path}` 같은 치환은 없습니다. MCP 서버는 설정 키가 아니므로 프로젝트 루트의 `.mcp.json`에 정의합니다.

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": { "Authorization": "Bearer ${GITHUB_PERSONAL_ACCESS_TOKEN}" }
    }
  }
}
```

**확장 설정 예시:**

```json
{
  "permissions": {
    "defaultMode": "manual",
    "allow": ["Bash(git log:*)", "Read"],
    "deny": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": [{ "type": "command", "command": "~/.claude/hooks/eslint-fix.sh" }] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": [{ "type": "command", "command": "~/.claude/hooks/security-scan.sh" }] }],
    "Stop": [{ "hooks": [{ "type": "command", "command": "~/.claude/hooks/notify.sh" }] }]
  }
}
```

이 설정의 GitHub MCP 서버는 위에서처럼 `.mcp.json`에 넣습니다.

### 사용자별 추가 설정

다음 키는 `~/.claude/settings.json`(또는 프로젝트 `.claude/settings.json`)에 넣으며, 개별 사용자의 대화형 동작을 정합니다.

| 설정 | 설명 |
|------|------|
| `askUserQuestionTimeout` | 답하지 않은 `AskUserQuestion` 대화 상자를 일정 시간 뒤 자동으로 넘김. **v2.1.200**부터 대화 상자가 기본으로 자동 진행되지 않으므로, 시간 제한 자동 진행을 다시 원하면 설정 |
| `enableArtifact` | 사용자별 Artifact 도구 켜기/끄기(v2.1.196) |
| `crossSessionInbound` | (v2.1.224) 받은 [세션 간 메시지](#세션-간-메시지) 처리 방식 — `"accept"`, `"hold"`, `"refuse"`. 프로젝트와 로컬 값은 `accept < hold < refuse` 단계에서 *더 엄격할* 때만 적용. v2.1.232부터 `/config`에 "Messages from your other sessions"로 표시 |
| `dialogExpiry` | (v2.1.224) 답하지 않은 대화 상자가 열려 있는 시간. 기본값 `"5m"`. `"60s"`, `"5m"`, `"10m"`, `"never"`를 받음. `CLAUDE_CODE_USER_DIALOG_TIMEOUT_MS`가 우선. v2.1.232부터 `/config`에 "Dialog expiry"로 표시 |
| `modelPicker` | (v2.1.243) `/model` 선택기에 나열할 모델을 원하는 순서와 이름으로 정함. 설정 계층 사이에서 **병합하지 않고 대체하는** 몇 안 되는 설정이라 가장 가까운 범위의 값이 그대로 이김 |
| `promptCacheTtl` | (v2.1.243) 메인 대화의 프롬프트 캐시 수명 선택 |
| `subagentPromptCacheTtl` | (v2.1.243) 서브에이전트와 메인 대화 밖 요청에 대한 같은 선택 |
| `modelPricing` | (v2.1.243) **관리형 설정.** 조직의 계약 단가를 넣어 `/cost`, 상태 줄, 텔레메트리가 정가 대신 그 단가로 보고하게 함 |
| `keybindingFlavor` | **v2.1.261부터 지원 중단되었고 효과 없음.** 프롬프트의 단어 편집 키는 Bash처럼 항상 readline 규칙을 따름: `Ctrl+W`는 공백까지 지우고, `Alt+F`와 `Alt+D`는 단어 끝에서 멈추며, 문장 부호가 단어를 나눔. 키는 아직 받으므로 이를 설정한 설정 파일도 유효함(v2.1.238–v2.1.260에서는 `"classic"`과 `"readline"` 중 선택) |
| `spellcheck` | (v2.1.235) `PATH`에 있는 `aspell`, `hunspell`, `ispell` 중 하나를 그 순서로 찾아 프롬프트 입력의 틀린 단어에 밑줄 표시. 객체 값(`{"enabled": true, "language": "en_GB"}`)이며 기본으로 꺼짐. **사용자 설정, `--settings` 플래그, 관리형 설정에서만 읽음**: 프로젝트 `.claude/settings.json`이나 `.claude/settings.local.json`의 `spellcheck` 블록은 무시 |
| `bashOutputMaxChars` | (v2.1.261) **성공한** Bash나 PowerShell 명령 출력 중 Claude가 바로 받을 글자 수, 최대 128K. 넘으면 Claude Code가 출력을 파일로 저장하고 Claude는 짧은 미리 보기와 경로를 받음. 설정하면 `BASH_MAX_OUTPUT_LENGTH`를 무시 |
| `taskOutputMaxChars` | (v2.1.261) **v2.1.278에서 대체되어 효과 없음.** 예전에는 `TaskOutput` 도구로 **백그라운드 태스크** 출력을 읽을 때 Claude가 바로 받는 글자 수를 제한했음. v2.1.278에서 `TaskOutput` 도구가 제거되어 이제 Claude는 `Read`로 백그라운드 태스크 출력 파일을 읽으므로, 이 키와 `TASK_MAX_OUTPUT_LENGTH` 모두 효과가 없음. 키는 아직 받으므로 이를 설정한 설정 파일도 유효함 |
| `maxEffortLevel` | (v2.1.267) Bedrock, Vertex, Foundry를 포함한 모든 제공자에서 Claude Code가 쓸 수 있는 노력 수준의 상한. 최상위나 `modelSettings` 아래 모델별로 설정. 사용자는 여전히 더 낮은 수준을 고를 수 있음 |
| `bashEditDiffEnabled` | (v2.1.269) Bash 도구 결과에 명령이 바꾼 파일의 diff를 포함 |
| `syncClaudeAiSkills` / `syncClaudeAiPlugins` | (v2.1.275) `false`로 설정하면 claude.ai 계정에서 켠 스킬이나 플러그인을 터미널 세션으로 동기화하지 않음 |

### 대체 모델 (`fallbackModel`)

`fallbackModel` 설정으로 주 모델이 과부하이거나 쓸 수 없을 때 차례로 시도할 대체 모델을 **최대 세 개**까지 정할 수 있습니다.

```json
{
  "fallbackModel": ["claude-opus-4-8", "claude-sonnet-4-6", "claude-haiku-4-5"]
}
```

**v2.1.166**부터 `--fallback-model` 플래그는 헤드리스뿐 아니라 대화형 세션에도 적용됩니다. 대체할 때 Claude Code는 예상치 못한 재시도 불가 오류를 한 번 다시 시도합니다. 인증, 속도 제한, 요청 크기, 전송 오류는 여전히 바로 실패합니다.

### 환경 변수

환경 변수로 설정을 재정의합니다.
```bash
# 모델 선택
export ANTHROPIC_MODEL=claude-opus-4-8
export ANTHROPIC_DEFAULT_MODEL=claude-opus-4-8   # (v2.1.236) 새 세션이 시작할 모델. ANTHROPIC_MODEL과 달리 /model 선택이 이를 재정의하며, 그 선택은 재시작 뒤에도 유지됨
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-8
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API 설정
export ANTHROPIC_API_KEY=sk-ant-...

# 사고 설정
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high   # low, medium, high, xhigh (Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8/4.7), max 중 선택 — Opus 5와 Opus 4.8의 기본값은 high (Opus 5.5, Sonnet 5.5, Haiku 5.5, Opus 5, Sonnet 5, Opus 4.8, Opus 4.7, Opus 4.6, Sonnet 4.6에서 지원)

# 기능 켜기/끄기
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=1    # 자동 메모리 끄기 (자동 메모리 파일을 만들거나 불러오지 않음)
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=0          # 태스크 도구가 기본값. 0이면 예전 TodoWrite 도구로 돌아감
export CLAUDE_CODE_SIMPLE=true              # --bare 플래그가 설정

# MCP 설정
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# 프롬프트 캐싱
export ENABLE_PROMPT_CACHING_1H=1      # 모든 요청에 1시간 캐시 TTL 요청 (기본값: 구독은 메인 대화 1시간, API 키 / 클라우드 제공자는 5분)

# 태스크 관리
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# 에이전트 팀 (실험 기능)
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# 서브에이전트와 플러그인 설정
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=1

# 하위 프로세스와 스트리밍
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000

# 출력과 패키지 관리자 (v2.1.129+)
export CLAUDE_CODE_FORCE_SYNC_OUTPUT=1                      # 자동 감지가 놓치는 터미널(Emacs eat 등)에 동기 출력 강제
export CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE=1            # Homebrew/WinGet 설치의 백그라운드 업그레이드 켜기
export CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1         # ANTHROPIC_BASE_URL을 설정했을 때 /v1/models 게이트웨이 탐색 사용

# Windows PowerShell 도구 (v2.1.143+) — Windows의 Bedrock/Vertex/Foundry에서 기본으로 켜짐
export CLAUDE_CODE_USE_POWERSHELL_TOOL=0                    # PowerShell 도구를 완전히 끔
export CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1    # `-ExecutionPolicy Bypass` 대신 시스템 ExecutionPolicy를 따름

# 워크로드 ID 페더레이션 (v2.1.141+)
export ANTHROPIC_WORKSPACE_ID=ws_abc123                     # 규칙이 여러 작업 공간을 포함할 때 페더레이션 토큰을 특정 작업 공간으로 제한

# Stop 훅 안전 한도 (v2.1.143+)
export CLAUDE_CODE_STOP_HOOK_BLOCK_CAP=8                    # 경고와 함께 세션이 끝나기 전까지 허용하는 Stop 훅 연속 차단 횟수. 0이면 한도 없음.

# 세션 전체 호출 한도 (v2.1.212)
export CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION=200         # 검색이 폭주하지 않도록 세션당 WebSearch 도구 호출 수 제한. 기본값 200.
export CLAUDE_CODE_WEB_SEARCH_REFILLS_PER_HOUR=100       # (v2.1.290) 그 예산의 충전 속도(시간당 호출 수). 대화형 기본값 100, -p에서는 0(충전 없음).

# 접근성 (v2.1.208)
export CLAUDE_AX_SCREEN_READER=1                            # 일반 텍스트 화면 낭독기 렌더링 모드 켜기. --ax-screen-reader나 사용자/관리형 설정의 "axScreenReader": true와 같은 효과 (v2.1.305부터 프로젝트/로컬 설정에서는 무시).

# 새 변수 (v2.1.221–v2.1.234) — 변경 기록 기준. CLI 레퍼런스에는 환경 변수 단락이 없음
export CLAUDE_CODE_ENABLE_TODO_TOOLS=1                      # (v2.1.233) 할 일/태스크 추적 도구(TaskCreate/Get/Update/List, TodoWrite) 되살리기. 이 도구는 Claude 3.x 모델, Opus 4부터 4.7, Sonnet 4부터 4.6, Haiku 4.5에서만 기본 제공 (v2.1.268)
export CLAUDE_CODE_WEBFETCH_CACHE_TTL_MS=900000             # (v2.1.233) WebFetch URL 캐시 TTL. 기본값 15분.
export CLAUDE_CODE_TOOL_MEMORY_LIMIT=2G                     # (v2.1.233, Linux) Bash 명령에 적용하는 선택형 메모리 cgroup
export ANTHROPIC_BEDROCK_REGION_PREFIX=us                   # (v2.1.224) 특정 Bedrock 교차 리전 추론 프로필을 우선
export CLAUDE_CODE_DISABLE_UNKNOWN_MODEL_WINDOW_ENFORCEMENT=1  # (v2.1.223) 알 수 없는 모델 ID에서 v2.1.223 이전의 자동 압축 동작으로 복원
export CLAUDE_CODE_WORKFLOW_PREFIX_STAGGER_MS=0             # (v2.1.229) 동적 워크플로 팬아웃에서 접두 시차 두기를 끔
export CLAUDE_CODE_USER_DIALOG_TIMEOUT_MS=300000            # (v2.1.224) dialogExpiry 설정을 재정의
export CLAUDE_CODE_PROJECT_DIR_NAME=my-app                  # (v2.1.234) 세션마다 자체 설정 디렉터리를 주는 호스트를 위한, 프로젝트별 대화 기록 디렉터리의 짧은 이름
export CLAUDE_CODE_GOAL_CHECKIN_MINUTES=30                  # (v2.1.234) /goal이 활성일 때 Claude가 확인하기 전까지 백그라운드 태스크가 멈춰 있어도 되는 시간(분). 0이면 확인 안 함.

# 새 변수 (v2.1.268-v2.1.278) - 변경 기록 기준
export CLAUDE_CODE_MCP_STARTUP_WAIT_MS=5000                 # (v2.1.274) 첫 비대화형 턴이 아직 연결 중인 MCP 서버를 기다리는 시간. 0 = 기다리지 않음. 단, --allowedTools나 mcp_tool 훅이 지정한 서버는 예외 (여전히 최대 2초, v2.1.284+).
export CLAUDE_CODE_WEBFETCH_DEADLINE_MS=300000              # (v2.1.268) WebFetch의 새 300초 기한을 재정의. 0이면 기한 없음.
export CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS=32        # (v2.1.269) Workflow 도구의 실행당 동시 에이전트 한도를 높임. 1-256.
export CLAUDE_CODE_AUTO_MODE_SERVER=0                       # (v2.1.278. v2.1.281부터 Anthropic API 직접 사용 시) 서버 쪽 자동 모드 분류기 검토를 쓰지 않음

# 새 변수 (v2.1.299-v2.1.300)
export CLAUDE_CODE_DISABLE_FILE_MENTIONS=1                  # (v2.1.299) @ 파일/디렉터리 멘션을 펼치지 않음. Claude는 경로를 받아 파일 도구로 읽음. 프로젝트/로컬 설정에서는 무시.
export CLAUDE_CODE_IDLE_COMPACT_MIN_TOKENS=200000           # (v2.1.299, v2.1.290+ 필요) 유휴 압축이 적용되는 컨텍스트 크기. 기본값 200000, 최소 100000. "idleCompaction": false면 끔.
export CLAUDE_CODE_GZIP_REQUEST_BODIES=0                    # (v2.1.300) api.anthropic.com으로 보내는 큰 요청 본문의 gzip을 끔 (TLS 검사 프록시가 잘못 처리할 때 사용).
export CLAUDE_CODE_ARTIFACT_PUBLISH_EDITS=0                 # (v2.1.300) 게시 중 편집 대신, 파일 도구로 아티팩트 파일을 편집한 뒤 별도 단계로 게시.

# 새 변수 (v2.1.301-v2.1.302)
export CLAUDE_CODE_AUTO_MODE_MODEL=claude-sonnet-4-6  # 클라이언트 쪽 자동 모드 분류기 모델 (v2.1.302). Opus 5, Sonnet 5.5, Opus 5.5나 정책이 제외한 모델이면 무시
export CLAUDE_CODE_RETRY_WATCHDOG_MAX_USAGE_LIMIT_WAIT_MS=3600000  # 먼 사용량 한도 초기화를 기다리지 않음 (v2.1.301)

# 새 변수 (v2.1.304-v2.1.310) — 실행하는 셸에서 설정
export CLAUDE_CODE_AUTO_COMPACT_WINDOW=200k                 # (v2.1.304) 자동 압축 윈도, 100000-1000000 토큰. k/M 접미사는 v2.1.304+ 필요, 숫자만 쓰면 토큰으로 읽음
export CLAUDE_CODE_DISABLE_MOUNTED_DRIVE_RM_PROMPT=1        # (v2.1.304) /Volumes/USB나 /mnt/c 같은 마운트 드라이브의 재귀 rm 프롬프트를 끔. 설정 env 블록에서는 무시
export CLAUDE_CODE_WARN_DISK_FREE_GIB=10                    # (v2.1.304) 작업 디렉터리 파일 시스템의 남은 디스크 공간이 N GiB 아래로 떨어지면 Claude에게 경고
export CLAUDE_CODE_DISABLE_MCP_TASK_BACKGROUND=1            # (v2.1.304) 대화형 세션에서 Claude가 MCP 태스크 결과를 기다리게 함
export CLAUDE_CODE_RETRY_WATCHDOG_USAGE_LIMIT_RECHECK_MS=600000  # (v2.1.305) CLAUDE_CODE_RETRY_WATCHDOG와 함께, 이 주기(60000-10800000 ms)로 사용량 한도를 다시 확인해 한도가 풀리면 일찍 재개
export CLAUDE_CODE_DISABLE_ULTRAREVIEW_TRUST_CHECK=1        # (v2.1.309) 물어볼 수 없는 신뢰하지 않은 폴더(CI)에서 `claude ultrareview` 실행 허용. 체크아웃을 신뢰할 때만 사용
```

> **v2.1.223 — `CLAUDE_CODE_DISABLE_1M_CONTEXT` 적용 범위 확대**: 이 변수는 이제 정해진 모델 ID 목록만이
> 아니라, 기본 1M 토큰 윈도를 가진 **모든** Claude 모델을 자동 압축으로 200K로 묶어 둡니다.

> **v2.1.108**: `ENABLE_PROMPT_CACHING_1H=1` — 메인 대화와 다른 요청에 1시간 프롬프트 캐시 TTL을 요청합니다. 설정하지 않으면 메인 대화는 요금제 사용량 안의 Claude 구독에서 1시간, API 키나 사용량 크레딧이나 클라우드 제공자에서 5분입니다. `FORCE_PROMPT_CACHING_5M=1`은 5분을 강제합니다. (v2.1.129에서 1시간 TTL이 아무 말 없이 5분으로 낮아지던 회귀를 고쳤습니다.)

> **v2.1.129**: `CLAUDE_CODE_FORCE_SYNC_OUTPUT=1`은 기능 자동 감지에 실패하는 터미널(예: Emacs `eat`)에 동기 출력을 강제합니다. `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE=1`은 원래 자동 업데이트되지 않는 Homebrew/WinGet 설치에서 백그라운드 업그레이드를 켭니다.

### 설정 관리 명령

```text
User: /config
[대화형 설정 메뉴가 열림]
```

`/config` 명령은 다음 같은 설정을 바꾸는 대화형 메뉴를 제공합니다.
- 확장 사고 켜기/끄기
- 상세 출력
- 권한 모드
- 모델 선택
- 동적 워크플로 크기(v2.1.219) — 설정 파일에 `workflowSizeGuideline`이 있으면 숨겨짐. [동적 워크플로](#동적-워크플로) 참고
- 유휴 압축(v2.1.302)

대화형 메뉴에서 Enter나 Space로 선택한 설정을 바꾸고, Esc로 저장하고 닫습니다(v2.1.183+).

메뉴를 열지 않고 프롬프트에서 바로 설정할 수도 있습니다.

```bash
/config thinking=false      # 설정 하나를 바로 지정 (v2.1.181+)
/config --help              # 쓸 수 있는 축약 키 나열 (v2.1.183+)
```

`key=value` 축약형은 대화형 세션, `-p`, 원격 제어에서 동작합니다.

### 프로젝트별 설정

프로젝트에 `.claude/settings.json`을 만듭니다.

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": [{ "type": "command", "command": "npm test && npm run lint" }] }]
  },
  "permissions": {
    "defaultMode": "manual"
  }
}
```

프로젝트의 MCP 서버는 프로젝트 루트의 `.mcp.json`에 넣습니다.

```json
{
  "mcpServers": {
    "project-db": {
      "command": "mcp-postgres",
      "env": {
        "DATABASE_URL": "${PROJECT_DB_URL}"
      }
    }
  }
}
```

`PROJECT_DB_URL`에는 읽기 전용 데이터베이스 사용자를 씁니다. [MCP → 데이터베이스 예제](../05-mcp/README.md)의 읽기 전용 설정을 참고하세요.

---

## 신뢰와 권한 범위

> **변경 기록 기준(v2.1.222, v2.1.232)**: 이 강화 사항은 변경 기록에서 가져왔으며,
> 설정 레퍼런스에는 아직 자세히 나와 있지 않습니다.

최근 릴리스에 되풀이되는 주제가 있습니다. 보안과 관련된 설정은 이제 클론한 저장소가
넓힐 수 없습니다. 알아 둘 변경은 세 가지입니다.

**중첩된 저장소는 따로 신뢰를 확인해야 합니다(v2.1.232).** 신뢰한 상위 디렉터리 안의 git 저장소는
더 이상 그 신뢰를 이어받지 않습니다. `~/work/monorepo`를 신뢰했고 그 안에 벤더링된 서브모듈이 있다면,
Claude Code가 처음 그 서브모듈 안에서 작업할 때 따로 신뢰할지 묻습니다.

**`sandbox.ripgrep`은 사용자 범위 전용입니다(v2.1.232).** 샌드박스가 쓰는 ripgrep 바이너리를
지정하는 이 설정은 사용자 설정, 관리형 설정, `--settings`에서만 적용됩니다.
프로젝트 설정으로는 더 이상 샌드박스가 다른 바이너리를 쓰게 할 수 없습니다.

**원격 제어 자동 시작은 사용자 범위 전용입니다(v2.1.222).** 저장소의 로컬 설정으로는 원격 제어
자동 시작을 켤 수 없으며, 사용자 범위에서 `/config`로만 켤 수 있습니다.

기억할 패턴: 어떤 설정이 커밋된 파일로 내 컴퓨터에서 Claude Code가 할 수 있는 일을 넓히게
해 준다면, 이제 그 설정은 사용자 범위 전용이라고 생각하세요.

---

## 에이전트 팀

에이전트 팀(Agent Teams)은 여러 Claude Code 인스턴스가 한 작업에 협업하게 하는 실험 기능입니다. 기본으로 꺼져 있습니다.

### 에이전트 팀 켜기

환경 변수나 설정으로 켭니다.

```bash
# 환경 변수
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

또는 설정 JSON에 추가합니다.

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### 에이전트 팀 동작 방식

- **팀 리더**가 전체 작업을 조율하고 하위 작업을 팀원에게 맡김
- **팀원**은 각자 자신의 컨텍스트 윈도로 독립적으로 작업
- **공유 태스크 목록**으로 팀원끼리 스스로 조율
- 서브에이전트 정의(`.claude/agents/`나 `--agents` 플래그)로 팀원의 역할과 전문 분야를 정함

### 표시 모드

에이전트 팀은 `--teammate-mode` 플래그로 정하는 표시 모드를 지원합니다.

| 모드 | 설명 |
|------|------|
| `in-process`(기본값) | 팀원이 같은 터미널 프로세스 안에서 실행 |
| `tmux` | 팀원마다 전용 분할 창(tmux나 iTerm2 필요) |
| `auto` | 가장 알맞은 표시 모드를 자동 선택 |

```bash
# 팀원 표시에 tmux 분할 창 사용
claude --teammate-mode tmux

# in-process 모드를 명시적으로 사용
claude --teammate-mode in-process
```

### 사용 사례

- 팀원마다 다른 모듈을 맡는 대규모 리팩터링
- 코드 리뷰와 구현을 병렬로
- 코드베이스 전반에 걸친 여러 파일 변경을 조율

> **참고**: 에이전트 팀은 실험 기능이라 이후 릴리스에서 바뀔 수 있습니다. 전체 레퍼런스는 [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams)를 참고하세요.

---

## 모범 사례

### 계획 모드
- ✅ 여러 단계로 된 복잡한 작업에 사용
- ✅ 승인 전에 계획 검토
- ✅ 필요하면 계획 수정
- ❌ 간단한 작업에는 쓰지 않기

### 확장 사고
- ✅ 아키텍처 결정에 사용
- ✅ 복잡한 문제 해결에 사용
- ✅ 사고 과정 검토
- ❌ 간단한 질문에는 쓰지 않기

### 백그라운드 태스크
- ✅ 오래 걸리는 작업에 사용
- ✅ 태스크 진행 상황 확인
- ✅ 태스크 실패를 매끄럽게 처리
- ❌ 동시에 너무 많은 태스크를 시작하지 않기

### 권한
- ✅ 코드 리뷰(읽기 전용)에는 `plan`
- ✅ 대화형 개발에는 `default`
- ✅ 자동화 워크플로에는 `acceptEdits`
- ✅ 안전장치가 있는 자율 작업에는 `auto`
- ❌ 꼭 필요하지 않다면 `bypassPermissions`를 쓰지 않기

### 세션
- ✅ 작업마다 세션을 따로 사용
- ✅ 중요한 세션 상태 저장
- ✅ 오래된 세션 정리
- ❌ 관련 없는 작업을 한 세션에 섞지 않기

---

## 추가 자료

Claude Code와 관련 기능에 대해 더 알아보기:

- [Official Interactive Mode Documentation](https://code.claude.com/docs/en/interactive-mode)
- [Official Headless Mode Documentation](https://code.claude.com/docs/en/headless)
- [CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [체크포인트 가이드](../08-checkpoints/) - 세션 관리와 되감기
- [슬래시 명령](../01-slash-commands/) - 명령 레퍼런스
- [메모리 가이드](../02-memory/) - 영구 컨텍스트
- [스킬 가이드](../03-skills/) - 자율 기능
- [서브에이전트 가이드](../04-subagents/) - 작업 위임 실행
- [MCP 가이드](../05-mcp/) - 외부 데이터 접근
- [훅 가이드](../06-hooks/) - 이벤트 기반 자동화
- [플러그인 가이드](../07-plugins/) - 묶음 확장
- [Official Scheduled Tasks Documentation](https://code.claude.com/docs/en/scheduled-tasks)
- [Official Chrome Integration Documentation](https://code.claude.com/docs/en/chrome)
- [Official Remote Control Documentation](https://code.claude.com/docs/en/remote-control)
- [Official Keybindings Documentation](https://code.claude.com/docs/en/keybindings)
- [Official Desktop App Documentation](https://code.claude.com/docs/en/desktop)
- [Official Agent Teams Documentation](https://code.claude.com/docs/en/agent-teams)

---

**최종 업데이트**: 2026년 10월 26일
**Claude Code 버전**: 2.1.310
**출처**:
- https://code.claude.com/docs/en/output-styles
- https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md
- https://code.claude.com/docs/en/permissions
- https://code.claude.com/docs/en/workflows#set-a-size-guideline
- https://code.claude.com/docs/en/tools-reference#task-tool-availability
- https://code.claude.com/docs/en/auto-mode-classifier-billing
- https://code.claude.com/docs/en/env-vars
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/sandboxing
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/cli-reference
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/settings.md
- https://code.claude.com/docs/en/settings-reference
- https://code.claude.com/docs/en/whats-new/2026-w34
- https://code.claude.com/docs/en/model-config#adjust-effort-level
- https://code.claude.com/docs/en/interactive-mode
- https://code.claude.com/docs/en/keybindings#modifiers
- https://code.claude.com/docs/en/model-config#extended-thinking
- https://github.com/anthropics/claude-code/releases/tag/v2.1.281
- https://github.com/anthropics/claude-code/releases/tag/v2.1.283
- https://code.claude.com/docs/en/permission-modes#critical-paths
- https://code.claude.com/docs/en/settings-reference#ultracode
- https://code.claude.com/docs/en/permission-modes#which-mode-a-session-starts-in
- https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode
- https://github.com/anthropics/claude-code/releases/tag/v2.1.284
- https://github.com/anthropics/claude-code/releases/tag/v2.1.285
- https://code.claude.com/docs/en/tools-reference
- https://code.claude.com/docs/en/chrome
- https://github.com/anthropics/claude-code/releases/tag/v2.1.288
- https://github.com/anthropics/claude-code/releases/tag/v2.1.290
- https://github.com/anthropics/claude-code/releases/tag/v2.1.293
- https://code.claude.com/docs/en/remote-control
- https://code.claude.com/docs/en/changelog#2-1-299
- https://code.claude.com/docs/en/changelog#2-1-300
- https://code.claude.com/docs/en/settings-reference#sandbox-enabled
- https://code.claude.com/docs/en/prompt-caching
- https://code.claude.com/docs/en/agent-teams
- https://code.claude.com/docs/en/monitoring-usage
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/managed-settings
- https://code.claude.com/docs/en/channels
- https://code.claude.com/docs/en/desktop
- https://code.claude.com/docs/en/fullscreen
- https://code.claude.com/docs/en/changelog#2-1-301
- https://code.claude.com/docs/en/changelog#2-1-302
- https://code.claude.com/docs/en/permission-modes#which-mode-a-non-interactive-run-starts-in
- https://code.claude.com/docs/en/permission-modes#choose-the-classifier-model
- https://code.claude.com/docs/en/changelog#2-1-309
- https://code.claude.com/docs/en/permission-modes#critical-path-removals-in-each-permission-mode
- https://code.claude.com/docs/en/permission-modes#the-mounted-drive-prompt
- https://code.claude.com/docs/en/permission-modes#time-limits-and-denials-in-auto-and-bypasspermissions-modes
- https://code.claude.com/docs/en/changelog#2-1-307
- https://code.claude.com/docs/en/settings-reference#axscreenreader
- https://code.claude.com/docs/en/routines#where-the-name-routines-still-appears
- https://code.claude.com/docs/en/scheduled-tasks
- https://code.claude.com/docs/en/desktop#monitor-pull-request-status
- https://code.claude.com/docs/en/changelog#2-1-281
- https://code.claude.com/docs/en/mcp#example-query-your-postgresql-database
- https://code.claude.com/docs/en/routines#permission-prompts-during-a-run
- https://code.claude.com/docs/en/changelog#2-1-310
- https://code.claude.com/docs/en/tools-reference#monitor-tool
- https://code.claude.com/docs/en/monitoring-usage#safety-event
- https://code.claude.com/docs/en/monitoring-usage#standard-attributes
- https://code.claude.com/docs/en/monitoring-usage#tool-decision-event
- https://code.claude.com/docs/en/changelog#2-1-304
- https://code.claude.com/docs/en/sessions#the-resume-this-conversation-dialog
- https://code.claude.com/docs/en/worktrees#clean-up-worktrees
- https://code.claude.com/docs/en/sandboxing#run-commands-outside-the-sandbox-with-excludedcommands
- https://code.claude.com/docs/en/settings-reference#sandbox-allownestedusernamespaces
- https://code.claude.com/docs/en/env-vars#variables
- https://code.claude.com/docs/en/model-config#values-and-warnings-for-claude_code_auto_compact_window
- https://code.claude.com/docs/en/auto-mode-config#generate-environment-entries-with-auto-mode-setup
- https://code.claude.com/docs/en/remote-control#requirements
- https://code.claude.com/docs/en/cli-reference#cli-flags
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
