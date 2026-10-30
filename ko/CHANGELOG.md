<!-- i18n-source: CHANGELOG.md -->
<!-- i18n-source-sha: 28149a8 -->
<!-- i18n-date: 2026-10-30 -->

# 변경 기록

> 한국어판 참고: 이 문서는 원문 `CHANGELOG.md`의 `28149a8` 시점 내용(최신 항목 v2.1.245)까지 옮긴 것입니다. 이후 항목은 [원문](../CHANGELOG.md)을 확인하세요. 항목에 나오는 파일 경로는 원문 저장소 기준입니다.

## [v2.1.245] — 2026-08-25

### Claude Code v2.1.245 기준 문서 동기화

Claude Code가 v2.1.235(2026-08-19)에서 **v2.1.245**(2026-08-25)로 올라갔습니다. 릴리스 10개 분량입니다. 이 항목은 그 동기화를 기록합니다.

> **참고**: 이 변경 기록에는 v2.1.233과 v2.1.235 동기화 항목이 없습니다(커밋 `deb7e5c`, `da6e09e`). 두 커밋은 변경 기록 항목 없이 저장소에 들어갔습니다. 날짜를 추정해 뒤늦게 채우지 않고, 빠졌다는 사실을 여기에 기록합니다.

### 수정

- **버전 최신성 표기가 낡았고 서로 모순됨** — 루트 `README.md`가 한 곳에서는 `v2.1.235`, 다른 곳에서는 `v2.1.220 (July 2026)`이라고 했고, 이 변경 기록의 맨 위 항목도 여전히 v2.1.220을 현재 Claude Code 릴리스라고 했습니다. 이제 모두 v2.1.245로 표기합니다.
- **훅 기본 시간 제한**을 `command`, `http`, `mcp_tool`은 60초에서 **600초**로 바로잡았고(`prompt`는 30초, `agent`는 60초), 이벤트별 재정의 값도 문서화했습니다.
- **스킬 우선순위**를 **enterprise > personal > project**로 바로잡았습니다. 레슨에서 project와 personal이 뒤바뀌어 레슨 자체의 퀴즈와 모순되었습니다.
- **메모리 파일은 재정의되지 않고 이어 붙여짐** — `02-memory/README.md`에 남아 있던 "overrides root CLAUDE.md" 문구 두 개를 바꿨습니다.
- **`SECURITY.md`**가 더 이상 "1.x releases" 지원을 안내하지 않습니다.
- **`STYLE_GUIDE.md`**가 더 이상 예시로 낡은 꼬리말을 가르치지 않습니다.
- 채점용 자료(`lesson-quiz`, `self-assessment` 스킬)를 채점 대상 레슨과 다시 맞췄습니다. 훅 이벤트 31개, 훅 유형 5가지, 되감기 옵션 6가지, 권한 모드 6가지, 가져오기(import) 깊이 4.

### 추가

- **WebSocket(`ws`) MCP 전송 방식** — 네 번째 전송 방식으로, `.mcp.json`이나 `claude mcp add-json`으로 설정합니다.
- **`Concise` 출력 스타일** — 다섯 번째 내장 스타일입니다(v2.1.237).
- **새 설정 키** — `modelPicker`, `promptCacheTtl`, `subagentPromptCacheTtl`, `modelPricing`, `keybindingFlavor`.
- **`ANTHROPIC_DEFAULT_MODEL`** 환경 변수(v2.1.236).
- **`/design`** — 디자인 캔버스 리서치 프리뷰.
- 세션 간 `SendMessage`의 **`notify_when_idle`**(v2.1.236).
- **플러그인 매니페스트 필드** — `workflows`, `channels`, `dependencies`와 관련 CLI 추가 사항.

### 변경

- **원격 제어(Remote Control)**가 리서치 프리뷰를 벗어났습니다. `claude remote-control`을 실행 중인 컴퓨터가 이제 Claude 앱에 기기 카드로 나타납니다.
- **`Ctrl+L`**은 화면을 지우지 않고 다시 그립니다(v2.1.238).
- **플러그인 `commands/`**를 레거시로 표시했습니다. 현재 안내는 `skills/`입니다.

## [v2.1.220-r2] — 2026-08-04

### Claude Code v2.1.220 기준 정확성 점검 (업스트림 버전 변경 없음)

v2.1.220(2026-07-25)이 여전히 현재 Claude Code 릴리스이므로, 이 항목은 버전 동기화가 아니라 **내부 수정**을 기록합니다. 공식 문서와 대조한 감사에서 빠진 업스트림 기능은 없었습니다. v2.1.218~v2.1.220의 모든 기능이 이미 문서에 있었습니다. 대신 망가진 예제 코드, 파일마다 다른 개수와 이름, 메타데이터 어긋남을 찾았습니다.

### 수정

- **`06-hooks/pre-commit.sh`가 실제로 커밋을 막은 적이 없음** — 스크립트가 네 곳에서 "Commit blocked."를 출력한 뒤 `exit 1`을 했습니다. 이 저장소의 종료 코드 표(`06-hooks/README.md`)에 따르면 exit 1은 *차단하지 않는* 오류라서 커밋은 그대로 진행되었습니다. 차단하는 것은 exit 2뿐입니다. 이제 네 곳 모두 `exit 2`를 하고 이유를 stderr에 씁니다. Claude Code는 차단 이유를 stderr에서 읽습니다.
- **`06-hooks/dependency-check.sh`가 실행된 적이 없음** — 대상을 `$1`에서 읽었지만 훅은 JSON을 stdin으로 받으므로 경로가 늘 비어 있었고 스크립트는 곧바로 끝났습니다. 이제 `format-code.sh`처럼 stdin에서 `file_path`를 파싱합니다.
- **`claude_concepts_guide.md`에서 중첩 서브에이전트 깊이가 5로 적혀 있음** — "up to 5 levels deep as of v2.1.172"라고 했는데, 이미 다른 네 파일에 문서화된 기본 깊이 3(v2.1.219)과 모순되었습니다. 현재 동작을 먼저 쓰고, `04-subagents/README.md`와 같은 세 시기 이력 메모를 붙이도록 고쳐 썼습니다.
- **다섯 파일에서 훅 이벤트 수가 29개로 적혀 있음** — `README.md`, `CATALOG.md`, `QUICK_REFERENCE.md`(4곳), `INDEX.md`, `resources.md`가 모두 29개라고 했습니다. 공식 훅 목록과 이름을 하나씩 대조해 확인한 정확한 수는 **31개**입니다. 이전 동기화에서는 두 파일만 고치고 나머지를 놓쳤습니다. `README.md`의 이벤트 목록은 25개, `INDEX.md`는 29개 이름만 나열했는데, 이제 둘 다 31개를 모두 나열합니다.
- **`/less-permission-prompts`는 실제 명령이 아님** — 내장 명령은 `/fewer-permission-prompts`입니다. `CATALOG.md`는 한 파일 안에서 *두 이름을 모두* 썼습니다.
- **`/fork`와 `/subtask`의 의미가 뒤바뀌어 문서화됨** — 두 명령은 v2.1.212에서 역할을 맞바꿨습니다. 이제 `/fork`는 대화를 독립된 새 백그라운드 세션으로 복사하고, 포크된 서브에이전트 동작은 `/subtask`로 옮겨 갔는데 `/subtask`는 저장소에 아예 없었습니다. 네 파일에서 바로잡고 추가했습니다.
- **`08-checkpoints/README.md`가 `cleanupPeriodDays`를 유일한 체크포인트 설정이라고 함** — `fileCheckpointingEnabled`(기본값 `true`)도 있고, `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING`과 체크포인트 100개 보존 한도도 있습니다. 저장소의 `config-examples.json`은 없다고 한 그 키를 이미 쓰고 있었습니다.
- **`03-skills/refactor/SKILL.md`가 `refactor/` 폴더에 있으면서 `name: code-refactor`를 선언함** — 지난 동기화에서 `doc-generator`에 대해 고친 것과 같은 결함입니다. 영어판에서 이름을 바꾸고 `ja/`, `zh/`, `uk/`, `vi/`에도 반영했습니다.
- **`03-skills/README.md`가 스킬 우선순위를 서로 모순되게 두 번 설명함** — 한 곳에서는 "enterprise > personal > project", 다른 곳에서는 "project wins by default"라고 했습니다. 이제 일관되게 **enterprise > project > personal**입니다.
- **`claude-md` 스킬이 AGENTS.md를 잘못 설명함** — 에이전트 정의 형식이라고 했습니다. 실제로는 여러 도구가 함께 쓰는 프로젝트 컨텍스트 파일이며, Claude Code는 **직접 읽지 않습니다**. `@AGENTS.md`로 가져오거나 심볼릭 링크로 연결해야 합니다.
- **`02-memory/directory-api-CLAUDE.md`가 루트 CLAUDE.md를 "재정의한다"고 함** — 메모리 파일은 이어 붙여지며, 재정의되지 않습니다.
- **`INDEX.md`가 `auto`와 `dontAsk`를 거꾸로 설명함** — `dontAsk`를 허용적인 모드("accept all except risky")로 설명했지만, 실제로는 가장 제한적인 모드입니다. 미리 승인되지 않은 것은 모두 자동으로 거부합니다. 공식 정의에 맞게 바로잡았습니다.
- **`permissions.mode`는 설정 키가 아님** — `09-advanced-features/README.md`와 `claude_concepts_guide.md`의 설정 예시 네 개가 이 키를 썼습니다(이미 대체된 값 `default`와 함께). 이제 모두 `permissions.defaultMode: "manual"`을 씁니다.
- **`09-advanced-features/config-examples.json`에 Opus 5가 없음** — v2.1.219부터 Opus 5가 기본 Opus인데도, "가장 성능이 높은" 프로필 세 개가 여전히 `claude-opus-4-8`을 썼습니다.
- **`05-mcp/database-mcp.json`에 자격 증명이 하드코딩됨** — `postgresql://user:pass@localhost/mydb`로, 모듈 자체의 "자격 증명을 하드코딩하지 말라"는 규칙과 모순되었습니다. 이제 `${DATABASE_URL}`입니다.
- **`05-mcp/README.md`가 MCP 범위는 설명하면서 `--scope` 플래그는 설명하지 않음** — 독자가 실제로 범위를 고를 방법이 없었습니다. 예제와 함께 추가했습니다.
- **`/output-style`이 지원 중단(deprecated)으로 나와 있음** — 실제로는 v2.1.91에서 *제거*되었습니다. 출력 스타일은 `/config`나 `outputStyle` 설정으로 계속 쓸 수 있습니다.
- **훅 `permissionDecision`에 `defer`가 빠짐** — 허용되는 값은 `allow`, `deny`, `ask`, `defer`이며, 우선순위는 `deny` > `defer` > `ask` > `allow`입니다.
- **컨텍스트 추적 훅이 128k 윈도를 가정함** — 현재 Claude 모델 중 그런 모델은 없습니다. 기본값을 1M으로 올리고 Haiku 4.5의 200k를 따로 적었습니다.
- **`claude_concepts_guide.md`의 로컬 메모리 경로가 틀림** — `.claude/local/CLAUDE.md`를 `./CLAUDE.local.md`로 바로잡았습니다.
- **`03-skills/doc-generator/SKILL.md`의 코드 펜스가 깨짐** — 바깥쪽 ```` ```markdown ```` 블록이 일찍 닫히고 닫히지 않은 펜스가 남아, 예제 절반이 실제 마크다운으로 렌더링되었습니다. 이제 바깥 펜스에 백틱 네 개를 씁니다. `check_markdown_rendering.py`가 README 파일만 검사하기 때문에 어느 검증 도구도 이를 잡지 못했습니다.
- **명령 템플릿 세 개의 스킬 이름이 잘못됨** — `Documentation Refactor`, `Setup CI/CD Pipeline`, `Expand Unit Tests`는 올바른 `name:` 값이 아니어서(소문자와 하이픈만 허용), README 안내대로 `.claude/skills/`에 복사하면 실패했습니다. 표준이 아닌 `tags:` 필드도 지웠습니다.
- **플러그인 에이전트 아홉 개가 도구 이름을 소문자로 씀**(`read, grep, bash`) — `04-subagents/` 템플릿은 정식 표기 `Read, Grep, Bash`를 썼습니다. 하나로 맞췄습니다.
- **다섯 파일의 "Claude Code 1.0+" 요구 사항** — 2.1+로 올렸습니다.
- **잘못 센 목록** — `CATALOG.md`의 요약표는 합계가 맞지 않았고 *게다가* 예제 수 세 개가 틀렸습니다(서브에이전트 11→9, MCP 8→4, 훅 8→11). `INDEX.md`는 플러그인(27→39), 스킬(21→23), 훅(9→12), 서브에이전트(9→10), 고급 기능(3→4)을 적게 적었고, 훅 스크립트 세 개와 `setup-auto-mode-permissions.py`를 빠뜨렸으며, 훅 이벤트 세 개의 이름을 잘못 붙였습니다. 모두 파일 트리에서 다시 셌습니다.
- **`CONTRIBUTING.md`가 "all four checks"라고 함** — 검사는 다섯 가지입니다.
- **서브에이전트 템플릿 두 개가 문서화되지 않음** — `clean-code-reviewer.md`와 `performance-optimizer.md`는 디스크에 있었지만 `04-subagents/README.md`의 예제 목록과 파일 트리 어디에도 없었습니다.
- **`resources.md`의 Mermaid 다이어그램이 가이드에 없는 색상표를 씀** — `STYLE_GUIDE.md`의 다섯 색상으로 다시 맞추고 필수 `stroke`/`color`를 붙였습니다.
- **메타데이터 꼬리말** — 61개 파일이 예전의 `Last Updated`만 있는 꼬리말을 쓰거나 꼬리말이 아예 없었습니다. 이제 모두 전체 블록을 갖췄습니다. 모듈 README 네 개에 `Compatible Models` 줄이 없었고, `02-memory/README.md`는 ISO 형식 날짜로 v2.1.217에 머물러 있었습니다. 대상 마크다운 파일 87개가 모두 **2.1.220**으로 표기됩니다.
- **작은 수정** — `session-end.sh`에 명시적 `exit 0`을 넣었습니다. 스크립트 네 개에서 실제 문법이 아닌 `# Hook: Event:Matcher` 축약 표기를 고쳐 썼습니다. MCP 예제 설정 네 개에 `"type": "stdio"`를 넣었습니다. 한 곳에서는 PR 42개, 다른 곳에서는 47개라고 한 실습 예제를 맞췄습니다.

## [v2.1.220] — 2026-07-29

### Claude Code v2.1.220에 동기화

튜토리얼 범위를 v2.1.217 기준(2026-07-22 동기화)에서 v2.1.220으로 올립니다. 연속된 릴리스 세 개(v2.1.218, v2.1.219, v2.1.220)이며 빠진 버전은 없습니다. 이번 동기화의 대부분은 v2.1.219 때문입니다. v2.1.219는 Claude Opus 5를 추가했고, 바로 이전 동기화에서 문서화한 중첩 서브에이전트 기본값을 뒤집었습니다.

### 수정

- **중첩 서브에이전트 기본값이 다시 뒤집힘 (v2.1.219)** — `04-subagents/README.md`, `CATALOG.md`, `10-cli/README.md`, `QUICK_REFERENCE.md`의 다섯 곳에서 중첩 생성이 기본적으로 꺼져 있다고 했습니다. 이는 v2.1.217~v2.1.218에서만 맞았습니다. v2.1.219는 기본값을 **깊이 3**으로 정하며, 이제 `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH=1`은 중첩을 켜는 것이 아니라 *끕니다*. 다섯 곳 모두 현재 동작을 먼저 쓰고, 똑같은 세 시기 이력 메모를 붙였습니다(v2.1.172~v2.1.216은 기본으로 최대 5단계까지 중첩되고 바꿀 방법이 없었음, v2.1.217은 깊이 1에서 중첩을 선택형으로 바꿈, v2.1.219는 기본값을 3으로 정함). 다음에 또 뒤집혀도 문서가 크게 틀어지지 않게 하기 위해서입니다.
- **Opus 4.8을 기본 Opus 모델로 적음** — `10-cli/README.md`에 "Opus 4.8 remains the default on Max, Team Premium, Enterprise pay-as-you-go, and the Claude API"라고 적혀 있었습니다. v2.1.219부터 기본값은 **Claude Opus 5**(`claude-opus-5`, 1M 컨텍스트, 기본 effort `high`)인데, 저장소의 어떤 모델 표에도 없었습니다. `10-cli`와 `claude_concepts_guide.md`의 모델 표, 18개 `Compatible Models` 꼬리말 전부, 그리고 effort 수준을 나열한 약 20곳에 추가했습니다. Microsoft Foundry에서는 `opus` 별칭이 여전히 Opus 4.6을 가리킵니다.
- **빠른 모드 모델 목록** — 이제 `/fast`는 **Opus 5와 Opus 4.8**에 적용되고, Opus 4.7은 빠졌습니다(v2.1.219). `10-cli/README.md`에서 Opus 4.6에 빠른 모드를 켜는 방법을 현재형으로 안내한 문장도 바로잡았습니다.
- **auto 모드 사용 조건의 자기모순** — `09-advanced-features/README.md`가 "available on all plans"와 "Team, Enterprise, or API" / "Anthropic API only"를 함께 말했습니다. 문서화된 네 가지 요구 사항(플랜 / 조직 / 모델 / 제공자)으로 고쳐 썼습니다. auto 모드는 **모든 플랜**에서 쓸 수 있으며, Team/Enterprise에서는 기본으로 켜져 있고 관리자가 `permissions.disableAutoMode`로 *끌 수* 있습니다. 이전 문서는 Owner가 *켜는* 방식으로 설명했습니다.
- **낡은 auto 모드 활성화 안내** — `CATALOG.md`가 여전히 서드파티 제공자에서 `CLAUDE_CODE_ENABLE_AUTO_MODE=1`을 설정하라고 했습니다. 이 요구 사항은 v2.1.207에서 없어졌습니다. 이 변수는 호환성을 위해 받아들이지만 아무 효과가 없습니다.
- **훅 이벤트 수 불일치** — `LEARNING-ROADMAP.md`와 `claude_concepts_guide.md`는 29개, `06-hooks/README.md`는 30개(지금은 31개)라고 했습니다. 둘 다 31개로 맞췄습니다. `claude_concepts_guide.md`의 이벤트 이름 표에는 새 `DirectoryAdded`와 함께 `MessageDisplay`(v2.1.152부터 있던 누락)도 빠져 있었습니다. 이제 이벤트 목록이 `06-hooks`와 같습니다.
- **`doc-generator` 스킬 이름 불일치** — `03-skills/doc-generator/SKILL.md`가 `doc-generator/` 폴더에 있으면서 `name: api-documentation-generator`를 선언해, 모든 카탈로그·색인 항목과 어긋났습니다. 영어판에서 이름을 바꾸고 `ja/`, `zh/`, `uk/`에도 반영했습니다.
- **낡은 "New Features (May 2026)" 제목** — `CATALOG.md`와 `resources.md`에서 다시 낡을 일이 없도록 날짜 없는 제목으로 바꾸고, `CATALOG.md`의 탐색 앵커도 맞췄습니다.
- **예전 `docs.anthropic.com` 링크** — `01-slash-commands/README.md`, `05-mcp/README.md`, `10-cli/README.md`에 남아 있던 꼬리말 링크 세 개를 `code.claude.com`으로 옮겼습니다.
- **내장 스킬을 가리는 스킬 예제** — `03-skills/README.md`가 맞춤 스킬 예제로 `name: deep-research`를 썼는데, 이 이름은 이제 내장 `/deep-research`를 가립니다. `topic-research`로 바꾸고 `vi/`, `ja/`, `uk/`에도 반영했습니다.

### 추가

- **`DirectoryAdded` 훅 (v2.1.219)** — 세션 중에 `/add-dir`이나 SDK `register_repo_root` 제어 요청이 새 작업 디렉터리를 등록한 뒤 발생합니다. `06-hooks/README.md` 이벤트 표에 추가했습니다(30 → 31개).
- **`/deep-research`는 명시적으로만 호출 (v2.1.218)** — Claude가 더 이상 스스로 실행하지 않습니다. 번들 스킬 표에 추가했습니다.
- **`/code-review`가 백그라운드 서브에이전트로 실행 (v2.1.218)** — 리뷰 작업이 더 이상 대화를 채우지 않으며, 이어서 쌓인 슬래시 명령도 계속 리뷰 대상입니다. `03-skills/README.md`와 `CATALOG.md`에 문서화했습니다.
- **포크 스킬이 기본적으로 백그라운드에서 실행 (v2.1.218)** — `context: fork` 스킬은 기본값이 `background: true`이며, 끄려면 `false`로 설정합니다. `background` 프런트매터 행, 이에 맞는 YAML 예시 줄, 기본값을 명시하는 문장을 추가했습니다. `background: true`가 백그라운드를 *강제하는* `04-subagents`와 모순되어 보이지 않게 하기 위해서입니다.
- **에이전트 프런트매터 훅에 작업 공간 신뢰가 필요 (v2.1.218)** — 프로젝트 서브에이전트의 훅은 에이전트 파일이 있는 폴더가 작업 공간 신뢰를 받은 뒤에만 실행됩니다.
- **동적 워크플로 규모 기준 (v2.1.219)** — 워크플로의 기본값은 이제 중간 기준(에이전트 15개 미만 목표)이며, `/config`의 **Dynamic workflow size**나 새 `workflowSizeGuideline` 설정 키로 고릅니다.
- **`sandbox.network.strictAllowlist` (v2.1.219)** — 샌드박스 명령에서 허용 목록에 없는 호스트를 묻지 않고 거부합니다.
- **MCP 오류 표시 (v2.1.219)** — 연결에 실패하면 `claude mcp list`와 `/mcp`가 HTTP 상태와 오류 메시지를 보여 줍니다. 설정 값 앞뒤의 보이지 않는 공백도 경고하며, 헤드리스 실행에서는 stream-json init 이벤트에 `mcp_server_errors`가 나옵니다. `05-mcp/README.md`에 문제 해결 안내로 추가했습니다.
- **stream-json의 중첩 서브에이전트 전달 (v2.1.219)** — `--forward-subagent-text`를 설정하면 깊이 2 이상의 서브에이전트도 나타나며, 자신을 생성한 `Agent` `tool_use` id로 구분됩니다.
- **프런트매터 불리언이 더 많은 값을 받음 (v2.1.218)** — `true`/`false`와 함께 `yes`/`no`, `on`/`off`, `1`/`0`을 대소문자 구분 없이 받습니다.
- **에이전트 이름에 `:` 불가 (v2.1.218)** — 플러그인 네임스페이스용으로 예약되었습니다.
- **auto 모드와 plan 모드가 분류기에 더 많이 맡김 (v2.1.218)** — `rm -rf /`와 `rm -rf ~`(명령·프로세스 치환 안에 있는 경우 포함)는 이제 권한 대화 상자를 여는 대신 분류기가 판정합니다. auto와 함께 쓰는 plan 모드는 정적 분석기가 읽기 전용임을 증명하지 못한 Bash 명령에 대해 더 이상 묻지 않습니다(`useAutoModeDuringPlan`, 기본으로 켜짐).
- **Opus 5 안전 분류기 대체 동작 (v2.1.219)** — 사이버보안으로 표시된 요청은 Opus 4.8에서 다시 실행되고, 생물학으로 표시된 요청은 거절로 끝납니다. Opus 5는 자체 생물학 분류기를 쓰며 대체 모델이 없기 때문입니다. 침투 테스트·CTF 작업에서 자주 발생하므로, 이 저장소의 보안 리뷰 서브에이전트와 플러그인 템플릿을 쓰는 독자와 관련이 있습니다.

## [v2.1.217] — 2026-07-22

### Claude Code v2.1.217에 동기화

튜토리얼 범위를 v2.1.212 기준(2026-07-18 동기화)에서 v2.1.217로 올립니다(업스트림에서 v2.1.213은 건너뛰었습니다. 변경 기록과 GitHub 릴리스 모두 2.1.212 → 2.1.214로 넘어갑니다). 저장소 내부 정확성 감사도 함께 했습니다.

### 수정

- **중첩 서브에이전트 생성 설명이 뒤집힘 (v2.1.217)** — `04-subagents/README.md`와 `CATALOG.md`가 "subagents can spawn their own subagents, nested up to 5 levels deep"를 현재 동작으로 설명했습니다(v2.1.172~v2.1.216에서만 맞음). v2.1.217부터는 기본으로 꺼져 있고, `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH`로 켭니다. 두 파일 모두 현재 동작을 먼저 쓰고 이력 메모를 남겼습니다.
- **낡은 `--enable-auto-mode` 플래그** — v2.1.111에서 제거되었지만(auto 모드가 기본 `Shift+Tab` 순환에 들어갔고, 지금은 `--permission-mode auto`로 시작함), `10-cli/README.md`는 네 곳에서 살아 있는 플래그처럼 안내했고 `QUICK_REFERENCE.md`는 세 곳에서 썼습니다. 일곱 곳 모두 `--permission-mode auto`로 바꿨습니다.
- **가져오기 깊이 모순("5 levels"와 "4 hops")** — 2026-07-18 동기화에서 두 곳을 고친 뒤에도 `02-memory/README.md`의 모범 사례 부분에 "5 levels"가 두 곳 남아 있었습니다. 두 곳 모두 파일의 나머지 부분과 같은 "4 hops"로 바꿨습니다.
- **Sonnet 5가 빠진 모델 표** — `claude_concepts_guide.md`의 "Models & Reasoning Effort" 표에 Claude Sonnet 5가 없었습니다. 같은 파일의 꼬리말은 이미 호환 모델로 적고 있었습니다. Pro/Team Standard/Enterprise의 기본 행으로 추가했습니다.
- **`config-examples.json`의 지어낸 스키마** — 예제 설정 11개 모두 실제 설정 스키마에 없는 필드(`mode: "unrestricted"/"confirm"`, `planning.*`, `extendedThinking.*`, `headless.*`, `checkpoints.autoCheckpoint`)와 낡은 모델 ID `claude-opus-4-7`을 썼습니다. 실제 `settings.json` 키(`permissions.defaultMode`, `permissions.allow`/`deny`, `env`, `hooks`, `fileCheckpointingEnabled`, `sandbox.*`)와 현재 모델 ID로 고쳐 썼습니다.
- **`brand-voice` 스킬과 README 예제의 불일치** — 실제 `03-skills/brand-voice/SKILL.md`(`name: brand-voice-consistency`, 사용자 호출 가능)가 `03-skills/README.md`의 "Example 4" 실습(`name: brand-voice`, `user-invocable: false`)과 어긋났습니다. 스킬 파일을 README의 배경 지식 방식에 맞췄습니다.
- **CLAUDE.md 길이 안내 충돌** — `03-skills/claude-md/SKILL.md`("<300 lines, ideally <100")와 `02-memory/README.md`("<500 lines")가 달랐습니다. 두 파일 모두 "몇백 줄 이내로, 짧을수록 좋다"로 통일했습니다(공식적으로 정한 한도는 없으며, 이전 두 숫자 모두 편집자 의견이었습니다).

### 추가

- **새 서브에이전트 한도 (v2.1.217)** — 위의 중첩 깊이 제어와 함께, 동시에 실행되는 서브에이전트 수 한도(기본 20, `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`)가 생겼습니다. `04-subagents/README.md`, `10-cli/README.md`, `QUICK_REFERENCE.md`에 문서화했습니다.
- **`--max-budget-usd`가 백그라운드 서브에이전트도 멈춤 (v2.1.217)** — 이전에는 비용 한도가 새 생성만 막았지만, 이제 이미 실행 중인 백그라운드 서브에이전트도 멈춥니다. `10-cli/README.md`에 문서화했습니다.
- **훅 `if:` 글롭 범위 축소 (v2.1.214)** — 한 단계짜리 `dir/**` 훅 `if:` 조건은 이제 `<cwd>/dir`에만 일치합니다(깊이와 상관없이 일치시키려면 `**/dir/**`). deny/ask 권한 규칙은 영향을 받지 않습니다. `06-hooks/README.md`에 문서화했습니다.
- **SessionStart `"fork"` 출처 (v2.1.214)** — 포크된 세션은 이제 출처를 `"resume"` 대신 `"fork"`로 보고합니다. `06-hooks/README.md`에 문서화했습니다.
- **`sandbox.filesystem.disabled` (v2.1.216)** — 네트워크 송신 제어는 유지하면서 파일 시스템 격리를 건너뜁니다. `09-advanced-features/README.md`와 `10-cli/README.md`에 문서화했습니다.
- **`/rewind`의 심볼릭 링크·하드 링크 보호 (v2.1.216)** — 추적 경로에 있는 심볼릭 링크나 하드 링크를 거쳐 파일을 복원하거나 지우지 않으며, 건너뛴 경로 수를 알려 줍니다. `08-checkpoints/README.md`에 문서화했습니다.
- **자동 메모리 `modified` 타임스탬프와 막지 않는 `/memory` 편집기** — 메모리 파일 프런트매터에 ISO 형식 `modified` 타임스탬프가 생겼고(v2.1.214), `/memory`의 GUI 편집기가 열려 있어도 세션이 막히지 않습니다(v2.1.216). `02-memory/README.md`에 문서화했습니다.
- **CLI·설정 묶음 (v2.1.214~v2.1.217)** — `emojiCompletionEnabled`, `FORCE_HYPERLINK=0`, `CLAUDE_CODE_OTEL_CONTENT_MAX_LENGTH`, 2MiB로 제한된 `--settings` 파일, 권한 강화(Docker/Podman 데몬 우회 플래그, `file -m`/`--magic-file`/`-f`/`--files-from`, 10,000자 이상 명령)를 `10-cli/README.md`에 문서화했습니다. `/context`의 컨텍스트 윈도 초과 경고와 `/compact` 실패가 이제 오류로 표시되는 점(v2.1.216)을 `01-slash-commands/README.md`에 문서화했습니다.
- **`/verify`와 `/code-review`는 명시적으로만 호출 (v2.1.215)** — `03-skills/README.md`와 `QUICK_REFERENCE.md`에 보충 메모를 넣었습니다. 저장소에서 자동 호출된다고 주장한 적은 없습니다.

### 참고

- `QUICK_REFERENCE.md`의 `--enable-auto-mode` / `--permission-mode auto` 불일치는, 실제 CLI 레퍼런스에서 전자가 v2.1.111에 제거되었음을 확인한 뒤 후자로 정리했습니다.
- 번역판(`vi`/`ja`/`zh`/`uk`)에서도 위의 P0/P1 내용을 확인했습니다. 번역된 `04-subagents/README.md`와 `CATALOG.md` 대부분은 중첩 생성 기능보다 이전 것이거나 해당 단락이 없어서 고칠 필요가 없었습니다. `10-cli/README.md`의 `--max-budget-usd` 설명 보충은 네 언어 모두에 반영했습니다.

## [v2.1.212] — 2026-07-18

### Claude Code v2.1.212에 동기화

튜토리얼 범위를 v2.1.206 기준(2026-07-11 동기화)에서 v2.1.212로 올립니다. 버전 차이와 무관한 결함을 찾아낸 저장소 내부 정확성 감사도 함께 했습니다.

### 수정

- **동작하지 않는 `#` 메모리 단축키 제거** — `02-memory/README.md`가 `#` 접두사로 메모리를 빠르게 추가하는 방법을 두 곳("Method 3"과 "Example 4" 실습)에서 동작하는 기능처럼 설명했습니다. 같은 파일 맨 위에서 세 줄 아래 있는 명령 표는 이미 `#`을 **Discontinued**로 표시하고 있어 정면으로 모순되었습니다. 두 단락을 지우거나 고쳐 써서 `/memory`와 대화로 메모리를 요청하는 방법을 안내하게 했습니다.
- **Bedrock/Vertex/Foundry에서 auto 모드가 켜는 방식에서 끄는 방식으로 바뀜 (v2.1.207)** — 이제 이 제공자들(그리고 로그인한 Claude 앱 게이트웨이 세션)에서 Sonnet 5, Opus 4.7/4.8, Fable 5에 대해 auto 모드를 기본으로 쓸 수 있습니다. v2.1.207부터 `CLAUDE_CODE_ENABLE_AUTO_MODE` 활성화 플래그는 효과가 없습니다. `09-advanced-features/README.md`와 `10-cli/README.md`에서 고쳤습니다.
- **`auto` 권한 모드를 "Research Preview"로 잘못 표시** — 현재 공식 문서는 `auto`를 프리뷰 기능이 아닌 GA(모든 플랜에서 쓸 수 있고, 모델과 제공자 조건만 있음)로 안내합니다. `09-advanced-features/README.md`, `CATALOG.md`, `README.md`에서 바로잡았고, `README.md`의 권한 모드 요약에 빠져 있던 `auto` 행도 넣었습니다.
- **`/fork` / `/branch` 이력 바로잡음** — 저장소는 "`/fork` renamed to `/branch` in v2.1.77 (alias retained)"라고 했습니다. 현재 문서에 따르면 둘은 서로 다른 현재 명령입니다. `/fork`는 대화를 물려받는 백그라운드 서브에이전트를 만들고, `/branch`는 그 자리에서 사본으로 전환합니다. 둘이 별칭으로 묶인 한 명령이었던 것은 v2.1.77부터 v2.1.161까지뿐입니다. `01-slash-commands/README.md`와 `09-advanced-features/README.md`에서 고쳤습니다.
- **`effort` 프런트매터 열거값 보완** — `04-subagents/README.md`는 4단계(`low`/`medium`/`high`/`max`)만 나열해, 이 열거값을 문서화한 다른 모든 파일에 있는 `xhigh`가 빠졌습니다.
- **번들 스킬 수 맞춤** — `CATALOG.md`의 요약표는 "9 bundled"라고 했지만 같은 파일의 상세 표는 10행이었습니다. 10개(전체 16개)로 바로잡았습니다.
- **`INDEX.md` 기능별 구성 매트릭스 전체 재계산** — 머리글은 "16 files", 스킬 행은 같은 분류에 대해 "28"이라고 했지만, 둘 다 문서화된 스킬 6개의 실제 파일 21개와 맞지 않았습니다. 플러그인 행도 합계는 36인데 40으로 표시했습니다. 모든 행을 손으로 다시 셌습니다. 스킬 행은 이제 `5 | 9 | 7`(**21**), 플러그인 행은 `11 | 9 | 3 | 3 | 3 | 3 | 7`(**39**)이며, 모든 행의 합계가 각 칸의 합과 같습니다.
- **낡은 `2.1.160` 꼬리말 묶음 갱신** — 다섯 파일(`08-checkpoints/checkpoint-examples.md`, `09-advanced-features/planning-mode-examples.md`, `07-plugins/*/README.md` 예제 패키지 파일 세 개)이 여전히 2026년 6월 꼬리말이었고, 버전별 내용 어긋남은 찾지 못했습니다. 2.1.212로 올렸습니다.
- **`QUICK_REFERENCE.md` 전체 재동기화** — `2.1.160`/6월 2일에 머물러 52개 버전 뒤처져 있었습니다. 권한 모드 블록을 `manual`(예전 `default`)로 갱신하고, Compatible Models에 Sonnet 5를 넣고, "New Features (May 2026)" 단락의 제목을 바꾸고 내용을 새로 했습니다.
- **`02-memory/README.md`의 Memory Hierarchy 단락 재작성** — 지어낸 8단계 엄격한 우선순위 모델을 확인된 구조로 바꿨습니다. CLAUDE.md 파일과 규칙은 재정의로 골라지는 것이 아니라 컨텍스트에 이어 붙여지며, `managed-settings.d/`는 CLAUDE.md가 아닌 `settings.json`의 방식입니다. Memory Architecture 다이어그램도 바로잡았습니다. 이전에는 claude.ai의 24시간 종합 주기와 Claude Code의 연속 자동 메모리를 뒤섞었습니다.

### 추가

- **서브에이전트 출력 검사 (v2.1.210)** — Claude Code는 서브에이전트 보고서에서 프롬프트 인젝션 패턴(가짜 시스템 태그, 지어낸 대화 턴, 권한 우회 언급)을 찾아 무력화합니다. `04-subagents/README.md`에 새 하위 단락으로 문서화했습니다.
- **세션 전체 생성 한도 (v2.1.212)** — WebSearch 호출(`CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION`)과 서브에이전트 생성(`CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION`, `/clear`로 초기화)에 세션당 기본 200개 한도가 생겼습니다. `09-advanced-features/README.md`, `04-subagents/README.md`, `10-cli/README.md`에 문서화했습니다.
- **오래 걸리는 MCP 도구의 자동 백그라운드 전환 (v2.1.212)** — 2분을 넘는 MCP 도구 호출은 이제 세션을 막지 않고 자동으로 백그라운드로 넘어갑니다. `CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS`로 설정합니다. `05-mcp/README.md`에 문서화했습니다.
- **`claude auto-mode reset`과 `/resume` 선택기 (v2.1.212)** — 새 CLI 하위 명령은 기본 auto 모드 설정을 복원합니다. 인수 없는 `/resume`은 이제 지난 세션(지운 세션 포함) 선택기를 열고 백그라운드 세션으로 재개합니다. `09-advanced-features/README.md`, `01-slash-commands/README.md`, `10-cli/README.md`에 문서화했습니다.
- **화면 낭독기 모드 (v2.1.208)** — `--ax-screen-reader`, `CLAUDE_AX_SCREEN_READER=1`, `"axScreenReader": true`로 켜는 일반 텍스트 렌더링입니다. `09-advanced-features/README.md`와 `10-cli/README.md`에 문서화했습니다.
- **Task 도구 `mode` 매개변수 지원 중단 (v2.1.212)** — `04-subagents/README.md`에 메모했습니다. 서브에이전트는 이제 기본으로 부모 세션의 권한 모드를 물려받으며, Task 도구의 호출별 `mode` 매개변수는 무시됩니다.

### 알려진 빈틈 (이번 동기화에서는 미룸)

- `03-skills/.claude/skills/blog-draft/`는 gitignore된 로컬 테스트용 임시 폴더(`03-skills/.gitignore`의 `# Local skill testing`)이며, `03-skills/blog-draft/`의 추적되는 중복이 아닙니다. 조치할 필요가 없음을 확인했습니다.

## [v2.1.160] — 2026-06-02

### Claude Code v2.1.160에 동기화

튜토리얼 범위를 Claude Code v2.1.160 릴리스로 올립니다. 그 사이의 v2.1.156 동기화(Claude Opus 4.8, #129)는 문서에 반영했지만 변경 기록에 따로 적지 않았습니다. 이 항목은 거기서 이어서 v2.1.157~v2.1.160 차이를 다룹니다. 이 범위에는 호환성을 깨는 변경이 없습니다. 추가된 것은 새 CLI·기능 몇 가지와 늘 하는 꼬리말 갱신입니다. 서드파티 제공자의 auto 모드는 새 기본값이 아니라 **선택해서 켜는** 기능입니다.

### 추가

- **`claude plugin init <name>` (v2.1.157)** — `.claude/skills`에 새 플러그인의 뼈대를 바로 만듭니다. 거기에 둔 플러그인은 이제 마켓플레이스 없이 자동으로 로드됩니다. `10-cli/README.md`, `07-plugins/README.md`, `CATALOG.md`에 문서화했습니다.
- **Bedrock / Vertex / Foundry의 auto 모드 (v2.1.158)** — 이제 세 서드파티 제공자에서 Opus 4.7/4.8에 auto 모드를 쓸 수 있으며, `CLAUDE_CODE_ENABLE_AUTO_MODE=1` 환경 변수로 **켭니다**. `09-advanced-features/README.md`, `10-cli/README.md`, `CATALOG.md`에 문서화했습니다.
- **세션 중 `EnterWorktree` 전환 (v2.1.157)** — `EnterWorktree` 도구로 세션 안에서 Claude가 관리하는 워크트리 사이를 오갈 수 있습니다. 끝난 워크트리는 잠기지 않은 채로 남으므로 `git worktree remove`/`prune`으로 정리할 수 있습니다. `09-advanced-features/README.md`에 문서화했습니다.

### 동작 변경

- **`acceptEdits` 쓰기 안전 확인 (v2.1.160)** — `acceptEdits` 모드에서도 Claude Code는 이제 셸 시작 파일(`.zshenv`, `.zlogin`, `.bash_login`, `~/.config/git/`)과 코드를 실행하는 빌드 설정(`.npmrc`, `.yarnrc*`, `bunfig.toml`, `.bazelrc`, `.pre-commit-config.yaml`, `.devcontainer/`)을 쓰기 전에 묻습니다. 그렇지 않으면 의도하지 않은 명령 실행으로 이어질 수 있습니다. `09-advanced-features/README.md`에 문서화했습니다.
- **동적 워크플로 트리거 키워드 `workflow` → `ultracode` (v2.1.160)** — "workflow"라는 단어만으로는 더 이상 동적 워크플로가 실행되지 않습니다. 트리거 키워드는 이제 `ultracode`입니다. `09-advanced-features/README.md`에 메모했습니다.

### 제거

- **`CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE`가 이제 아무 일도 하지 않음 (v2.1.160)** — 이 환경 변수는 제거되어 효과가 없습니다. `10-cli/README.md` 환경 변수 표의 문구를 "removed 2026-06-01"에서 "Removed (no-op as of v2.1.160)"로 바꿨습니다.

### 문서

- `README.md`에서 서로 맞지 않던 버전 문자열 세 개(배지와 FAQ 본문이 `2.1.145` / `v2.1.150`에 머물러 있었음)를 고치고, 낡은 Sources 링크를 정리했습니다.
- 일관된 동기화를 위해 모든 영어 문서의 메타데이터 꼬리말을 **v2.1.160 / June 2, 2026**으로 올렸습니다.

## [v2.1.150] — 2026-05-25

### Claude Code v2.1.150에 동기화

튜토리얼 범위를 Claude Code v2.1.145 → v2.1.150(2026년 5월 23일 릴리스)으로 올립니다. 지난 동기화 이후 Anthropic은 패치 다섯 개(v2.1.146~v2.1.150)를 냈습니다. 가장 큰 변화는 **번들 `/simplify` 스킬의 이름이 `/code-review`로 바뀐 것**(v2.1.146)입니다. 순수한 이름 변경이고 **별칭이 없으므로** 예전 이름은 더 이상 동작하지 않습니다. 이 저장소에도 로컬 코드 리뷰 스킬이 있으므로, 새 내장 스킬을 가리지 않도록 디렉터리 이름을 `code-review-specialist`로 바꿨습니다. 그 밖의 주요 내용: `/usage`가 비용을 분류별로 나눠 보여 주고, `Ctrl+T`로 백그라운드 세션을 고정할 수 있으며, 마크다운 렌더러가 GFM 작업 목록 체크박스를 지원하고, 새 관리형 설정 `allowAllClaudeAiMcps`가 추가되었습니다. 이번 동기화에서는 v2.1.143에 머물러 있던 모듈 README 네 개(`04-subagents`, `05-mcp`, `07-plugins`, `09-advanced-features`)도 따라잡았습니다.

### 동작 변경

- **`/simplify`가 `/code-review`로 이름이 바뀜 (v2.1.146)**: 번들 리뷰 스킬은 이제 `/code-review`로 호출하며, 선택적으로 effort 수준을 받습니다(예: `/code-review high`). `--comment`를 넘기면 발견 사항을 GitHub PR 인라인 댓글로 답니다(v2.1.147). 예전 `/simplify` 이름은 더 이상 동작하지 않으며 별칭도 없습니다. `01-slash-commands/README.md`, `03-skills/README.md`, `CATALOG.md`, `QUICK_REFERENCE.md`, `claude_concepts_guide.md`를 갱신했습니다.

### 변경

- **저장소의 로컬 `code-review` 스킬 이름을 `code-review-specialist`로 바꿈** — 새 내장 `/code-review`와 충돌하지 않게 하기 위해서입니다. 디렉터리를 `03-skills/code-review/` → `03-skills/code-review-specialist/`로 바꾸고, `README.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `CATALOG.md`, `LEARNING-ROADMAP.md`, `claude_concepts_guide.md`, `03-skills/README.md`의 설치 명령, 디렉터리 트리, 상호 참조를 모두 갱신했습니다. 충돌 이유와 내장 스킬을 가리지 않는 방법을 설명하는 메모도 추가했습니다.

### 추가

- **`/usage`의 분류별 비용 내역 (v2.1.149)** — 비용 화면이 이제 지출을 분류별(스킬, 서브에이전트, 플러그인, MCP 서버별 비용)로 나눠 보여 줍니다. `CATALOG.md`와 `claude_concepts_guide.md`에 문서화했습니다.
- **백그라운드 세션 고정 — `Ctrl+T` (v2.1.147)** — `claude agents`에서 세션을 고정하면 유휴 상태에서도 유지되고, Claude Code 업데이트를 적용하려고 제자리에서 다시 시작하며, 메모리가 부족할 때는 고정하지 않은 세션을 먼저 정리한 뒤에야 정리됩니다. `10-cli/README.md`에 문서화했습니다.
- **GFM 작업 목록 체크박스 렌더링 (v2.1.149)** — 마크다운 렌더러가 이제 `- [ ]` / `- [x]`를 체크박스로 그립니다. `09-advanced-features/README.md`에 문서화했습니다.
- **`allowAllClaudeAiMcps` 관리형 설정 (v2.1.149)** — 조직 전체에서 claude.ai 클라우드 MCP 커넥터를 로드하도록 허용합니다. `05-mcp/README.md`에 문서화했습니다.

### 제거

- **Stop/SubagentStop 훅 입력 필드 `background_tasks`와 `session_crons`** — `06-hooks/README.md`와 `resources.md`에서 지웠습니다. v2.1.145 릴리스 노트를 보고 추가했지만 공식 훅 레퍼런스 페이지에는 나오지 않습니다. 문서를 공개된 레퍼런스와 맞추려고 지웠습니다.

### 문서

- 모듈 README 네 개를 v2.1.143에서 v2.1.150으로 따라잡았습니다: `04-subagents/README.md`, `05-mcp/README.md`, `07-plugins/README.md`, `09-advanced-features/README.md`.
- 일관된 동기화를 위해 모든 영어 문서의 메타데이터 꼬리말을 **v2.1.150 / May 25, 2026**으로 올렸습니다.

## [v2.1.145] — 2026-05-20

### Claude Code v2.1.145에 동기화

튜토리얼 범위를 Claude Code v2.1.143 → v2.1.145(2026년 5월 19일 릴리스)로 올립니다. 지난 동기화 이후 Anthropic은 패치 두 개(v2.1.144, v2.1.145)를 냈습니다. 주요 내용: `/extra-usage`가 `/usage-credits`로 이름이 바뀌었고, `/model`이 기본적으로 세션 한정이 되었고, 번들 스킬 세 개(`/run`, `/verify`, `/run-skill-generator`)가 새로 생겼고, Stop/SubagentStop 훅 입력 필드 `background_tasks`와 `session_crons`가 생겼고, 스크립트용 `claude agents --json`이 생겼으며, 환경 변수만 붙인 Bash 명령의 자동 승인 허점을 막는 보안 수정이 들어갔습니다. 이번 동기화에서는 v2.1.143 동기화 때 빠져 v2.1.138에 머물러 있던 루트 참고 문서 여섯 개(`LEARNING-ROADMAP.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `resources.md`, `claude_concepts_guide.md`, `STYLE_GUIDE.md`)도 따라잡았습니다.

### 추가

- `/usage-credits` 슬래시 명령 (v2.1.144) — `/extra-usage`를 대신하는 주 이름입니다. `/extra-usage`도 별칭으로 계속 동작합니다. `01-slash-commands/README.md`와 `CATALOG.md`에 문서화했습니다.
- 새 번들 스킬 세 개 (v2.1.145) — `/run`(변경 사항이 실행되는 모습을 보려고 프로젝트 앱을 띄움), `/verify`(앱을 빌드·실행·관찰해 수정이 동작하는지 확인), `/run-skill-generator`(프로젝트별 스킬을 만들어 `/run`/`/verify`에 특정 프로젝트를 다루는 방법을 가르침). `03-skills/README.md`, `CATALOG.md`, `QUICK_REFERENCE.md`에 문서화했습니다. 정식 번들 스킬 수는 **9개**가 되었습니다.
- Stop/SubagentStop 훅 입력 필드 `background_tasks`와 `session_crons` (v2.1.145) — 훅 작성자는 이 값을 읽고, 백그라운드 작업이나 예약 작업이 아직 남아 있을 때 중지를 막을지 정할 수 있습니다. `06-hooks/README.md`에 문서화했습니다.
- `claude agents --json` (v2.1.145) — 스크립트(상태 표시줄, 세션 선택기, tmux-resurrect)용으로 에이전트 목록을 기계가 읽을 수 있는 JSON으로 출력합니다. `10-cli/README.md`에 문서화했습니다.
- 요약표에 빠져 있던 훅 이벤트 행 다섯 개 — `Setup`, `UserPromptExpansion`, `PermissionDenied`, `PostToolBatch`(본문은 이미 "29 events"라고 했지만 `CATALOG.md`, `claude_concepts_guide.md`, `INDEX.md`의 요약표는 25개만 나열했습니다).

### 동작 변경

- **`/model`이 기본적으로 세션 한정 (v2.1.144)**: 모델을 고르면 이제 현재 세션에만 적용됩니다. 고른 뒤 `d`를 누르면 이후 세션의 새 기본값으로 정합니다. `01-slash-commands/README.md`에 문서화했습니다.
- **환경 변수만 붙인 Bash 명령의 자동 승인 차단 (v2.1.145 보안 수정)**: 허용 목록에 `FOO=bar`만 있을 때 `FOO=bar somecommand` 형태의 명령은 더 이상 자동 승인되지 않습니다. 이런 명령은 전체 명령을 덮는 `Bash(...)` 권한 규칙으로 명시적으로 다시 허용합니다. `06-hooks/README.md`에 문서화했습니다.
- **`context: fork` 무한 루프 수정 (v2.1.145)**: 드물게 `context: fork`를 쓰는 스킬이 무한히 다시 호출되는 루프를 일으킬 수 있었습니다. `03-skills/README.md`에 메모로 문서화했습니다.

### 문서

- 루트 참고 문서 여섯 개를 v2.1.138에서 v2.1.145로 따라잡았습니다: `LEARNING-ROADMAP.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `resources.md`, `claude_concepts_guide.md`, `STYLE_GUIDE.md`.
- 번들 스킬 불일치를 고쳤습니다. `CATALOG.md`, `QUICK_REFERENCE.md`, `03-skills/README.md`가 서로 다른 5개짜리 목록 세 개를 나열하고 있었습니다. 정식 9개(`/batch`, `/claude-api`, `/debug`, `/fewer-permission-prompts`, `/loop`, `/run`, `/run-skill-generator`, `/simplify`, `/verify`)로 맞췄습니다. `QUICK_REFERENCE.md`의 칸은 `/voice`와 `/browse`도 번들 스킬로 잘못 나열했는데, 둘 다 번들 스킬이 아닙니다.
- 저장소의 나머지 부분과 맞추려고 `QUICK_REFERENCE.md`와 `resources.md`의 "New Features (March 2026)"를 "New Features (May 2026)"로 바꿨습니다.
- `README.md`의 버전 배지를 `2.1.138`에서 `2.1.145`로 올리고, 본문의 "latest: v2.1.138" 두 곳을 갱신했습니다.
- 기여자가 현재 버전을 복사하도록 STYLE_GUIDE의 메타데이터 꼬리말 예시를 `2.1.97`에서 `2.1.145`로 갱신했습니다.

## [v2.1.143] — 2026-05-19

### Claude Code v2.1.143에 동기화

튜토리얼 범위를 Claude Code v2.1.138 → v2.1.143(2026년 5월 15일 릴리스)으로 올립니다. 지난 동기화 이후 Anthropic은 패치 다섯 개(v2.1.139~v2.1.143)를 냈습니다. 주요 내용: `/goal`과 `/scroll-speed` 슬래시 명령, 디스패치 플래그를 모두 갖춘 `claude agents` Agent View(리서치 프리뷰), Stop 훅 안전 한도, 훅 exec 형식(`args`), PostToolUse의 `continueOnBlock`, 훅 `terminalSequence` 출력, Opus 4.7이 기본이 된 빠른 모드, Bedrock/Vertex/Foundry용 Windows에서 기본으로 켜진 PowerShell, `worktree.bgIsolation` 설정.

### 추가

- `/goal <statement>` 슬래시 명령 (v2.1.139) — 경과 시간, 턴 수, 토큰 사용량을 보여 주는 실시간 오버레이 패널과 함께 세션 수준 완료 조건을 등록합니다. `01-slash-commands/README.md`에 문서화하고 `10-cli/README.md`에서 연결했습니다.
- `/scroll-speed <±N>` 슬래시 명령 (v2.1.139) — TUI 실시간 미리 보기의 스크롤 속도를 조절하며, 컴퓨터별로 유지됩니다. `01-slash-commands/README.md`에 문서화했습니다.
- `claude agents` Agent View (리서치 프리뷰, v2.1.139)와 디스패치 플래그 `--cwd`(v2.1.141), `--add-dir`, `--settings`, `--mcp-config`, `--plugin-dir`, `--permission-mode`, `--model`, `--effort`, `--dangerously-skip-permissions`(v2.1.142). `10-cli/README.md`에 문서화했습니다.
- `claude plugin details <name>` (v2.1.139) — 플러그인 전체 구성과 턴별·호출별 예상 토큰 비용을 보여 줍니다. v2.1.142에서 상세 창에 LSP 서버가 추가되었습니다. `07-plugins/README.md`에 문서화했습니다.
- `/plugin` 탐색 창의 마켓플레이스 컨텍스트 비용 예측 (v2.1.143). `07-plugins/README.md`에 문서화했습니다.
- 훅 **exec 형식** (`args: string[]`, v2.1.139) — 셸 파싱 없이 `execve()`로 바로 실행합니다. 셸 형식 `command` 필드와 함께 쓸 수 없습니다. `06-hooks/README.md`에 문서화했습니다.
- PostToolUse의 훅 `continueOnBlock: true` 필드 (v2.1.139) — 차단된 도구 결과를 턴을 중단하는 대신 `tool_result`로 Claude에게 돌려줍니다. `06-hooks/README.md`에 문서화했습니다.
- 훅 `terminalSequence` JSON 출력 필드 (v2.1.141) — 데스크톱 알림, 창 제목, 벨을 위한 OSC 이스케이프 시퀀스를 그대로 내보냅니다. `06-hooks/README.md`에 문서화했습니다.
- `worktree.bgIsolation: "none"` 설정 (v2.1.143) — 백그라운드 세션이 격리된 워크트리 대신 현재 작업 사본을 직접 편집합니다. `09-advanced-features/README.md`에 문서화했습니다.
- 이제 모든 MCP stdio 서버의 환경에 `CLAUDE_PROJECT_DIR`이 전달되며(v2.1.139), 플러그인과 프로젝트 `.mcp.json`의 `command`/`args`/`env` 필드에서 `${CLAUDE_PROJECT_DIR}` 치환을 지원합니다. `05-mcp/README.md`에 문서화했습니다.
- 서브에이전트 OTEL 헤더 `x-claude-code-agent-id`와 `x-claude-code-parent-agent-id` (v2.1.139) — `claude_code.llm_request` OTEL 스팬에 `agent_id` / `parent_agent_id` 속성으로 노출됩니다. `04-subagents/README.md`에 문서화했습니다.
- `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1` (v2.1.142) — v2.1.142에서 기본값이 Opus 4.7로 바뀐 뒤 빠른 모드를 Opus 4.6으로 되돌려 고정합니다. `10-cli/README.md`에 문서화했습니다.
- `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`과 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1` (v2.1.143) — 기본으로 켜진 PowerShell 도구를 끄거나, `-ExecutionPolicy Bypass` 대신 시스템 실행 정책을 따르게 합니다. `09-advanced-features/README.md`에 문서화했습니다.
- `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` (v2.1.143) — Stop 훅의 연속 8회 차단 안전 한도를 바꿉니다(`0`이면 끔). `06-hooks/README.md`와 `09-advanced-features/README.md`에 문서화했습니다.
- `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` (v2.1.141) — SSH 키가 없는 CI 러너를 위해 플러그인을 설치할 때 GitHub 플러그인 소스를 HTTPS로 클론하게 합니다. `07-plugins/README.md`에 문서화했습니다.
- `ANTHROPIC_WORKSPACE_ID` (v2.1.141) — 페더레이션 워크로드 ID 토큰의 범위를 특정 작업 공간으로 정합니다. `09-advanced-features/README.md`에 문서화했습니다.
- 루트 수준 `SKILL.md` 플러그인 패턴 (v2.1.142) — `skills/` 하위 디렉터리 없이 최상위 `SKILL.md`만 있는 플러그인은 스킬 하나로 나타납니다. `07-plugins/README.md`에 문서화했습니다.
- `/schedule`의 마케팅 이름 **Routines** (Anthropic 블로그, 2026-05-14) — `09-advanced-features/README.md`에 한 줄 메모로 넣었습니다. CLI에서는 여전히 `/schedule`입니다.

### 동작 변경

- **빠른 모드 기본값이 Opus 4.7로 바뀜 (v2.1.142)**: `/fast`는 이제 기본으로 Opus 4.7을 씁니다(이전에는 Opus 4.6). 되돌리려면 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`을 설정합니다.
- **Bedrock/Vertex/Foundry용 Windows에서 PowerShell 도구가 기본으로 켜짐 (v2.1.143)**: Claude Code는 `-ExecutionPolicy Bypass`로 PowerShell을 호출합니다. `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`(시스템 정책 따르기)이나 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`(도구 끄기)으로 바꿀 수 있습니다.
- **API 키 인증을 설정하면 원격 제어, `/schedule`, claude.ai MCP 커넥터, 알림 설정이 자동으로 꺼짐 (v2.1.139)**: `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN`, `apiKeyHelper` 중 하나를 설정하면, claude.ai 로그인이 함께 되어 있어도 claude.ai와 연결된 네 기능이 모두 꺼집니다.
- **Stop 훅 차단 루프가 연속 8회로 제한됨 (v2.1.143)**: 8회 연속 차단되면 경고와 함께 세션이 끝나므로, 버그 있는 Stop 훅이 세션을 끝없이 돌리지 못합니다. `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`으로 바꿉니다.
- **`subagent_type` 일치 판정이 대소문자와 구분자를 무시함 (v2.1.140)**: `code-reviewer`, `Code Reviewer`, `code_reviewer`가 모두 같은 에이전트로 해석됩니다. `04-subagents/README.md`에 문서화했습니다.

### 변경

- 루트 참고 문서(`README.md`, `CATALOG.md`)의 `28 hook events`를 `29 hook events`로 갱신했습니다. v2.1.138에서 `Setup` 훅이 생긴 뒤의 `06-hooks/README.md`, `LEARNING-ROADMAP.md`와 맞춘 것입니다.

### 번역자 참고

- 튜토리얼 번역판(`vi/`, `ja/`, `uk/`, `zh/`)은 영어판을 따릅니다. 이번 차례의 모듈 README 변경과 위 CHANGELOG를 동기화합니다. 꼬리말은 `Last Updated: May 19, 2026`과 `Claude Code Version: 2.1.143`을 반영해야 합니다.

## [v2.1.138] — 2026-05-09

### Claude Code v2.1.138에 동기화

튜토리얼 범위를 Claude Code v2.1.131 → v2.1.138(2026년 5월 9일 릴리스)로 올립니다. 지난 동기화 이후 Anthropic은 v2.1.132부터 v2.1.138까지 패치 일곱 개를 냈습니다.

### 추가 (영어 문서)

- `worktree.baseRef` 설정 (v2.1.133) — `claude --worktree`가 `origin/<default>`(`"fresh"`, 기본값)에서 브랜치를 만들지 로컬 `HEAD`(`"head"`)에서 만들지 정합니다. **동작 변경**: 기본값 `"fresh"`는 v2.1.128 동작을 되돌리므로, v2.1.128 이후 로컬 `HEAD` 기준 브랜치에 의존하던 사용자는 다시 설정해야 합니다. `09-advanced-features/README.md`에 문서화했습니다.
- `autoMode.hard_deny` 관리자 키 (v2.1.136) — 추론된 사용자 의도와 상관없이 특정 종류의 작업을 막는 분류기 규칙 배열입니다. auto 모드에서 절대 실행되면 안 되는 작업(예: `rm -rf /`, 보호된 브랜치로 강제 푸시)에 씁니다. `soft_deny`와 달리 hard-deny 규칙은 분류기가 협상할 수 없습니다. `09-advanced-features/README.md`에 문서화했습니다.
- `parentSettingsBehavior` 관리자 키 (v2.1.133+, 관리자 등급) — SDK의 `managedSettings`가 부모 프로세스 설정과 어떻게 합쳐질지 정합니다. `"first-wins"`는 기존 우선순위를 유지하고, `"merge"`는 값을 깊게 병합합니다. `09-advanced-features/README.md`에 문서화했습니다.
- `Setup` 훅 이벤트 — 초기 환경 설정(세션마다 한 번)으로, 도구를 준비하거나 의존성을 설치하는 데 씁니다. 문서화된 훅 이벤트가 28개에서 29개가 되었습니다. `06-hooks/README.md`에 문서화했습니다.
- 훅 입력 JSON의 `effort.level` 필드 (v2.1.133) — 현재 effort 수준(`low`/`medium`/`high`/`xhigh`/`max`)을 훅에 알려 줍니다. `06-hooks/README.md`에 문서화했습니다.
- Bash 하위 프로세스의 `CLAUDE_CODE_SESSION_ID` 환경 변수 (v2.1.132) — 훅 입력 JSON의 `session_id` 필드와 같은 세션 UUID로, bash 로그와 훅 텔레메트리를 연결하는 데 씁니다. `06-hooks/README.md`에 문서화했습니다.
- Bash 하위 프로세스의 `CLAUDE_EFFORT` 환경 변수 (v2.1.133) — 현재 effort 수준으로, 훅 입력 JSON의 `effort.level`과 같습니다. `06-hooks/README.md`에 문서화했습니다.
- `sandbox.bwrapPath`와 `sandbox.socatPath` 설정 (v2.1.133+, Linux/WSL) — `bubblewrap`과 `socat`이 표준이 아닌 위치에 설치되어 있을 때 Claude Code에 경로를 알려 줍니다. 기본값은 `$PATH`에서 찾는 것입니다. `09-advanced-features/README.md`에 문서화했습니다.
- `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` 환경 변수 (v2.1.132). `09-advanced-features/README.md`에 문서화했습니다.
- `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` 환경 변수 (v2.1.136) — OpenTelemetry 데이터를 수집하는 조직에서 세션 품질 설문을 다시 켭니다. OTEL 배포에서는 기본으로 꺼져 있습니다. `09-advanced-features/README.md`에 문서화했습니다.

### 변경

- **동작 변경**: 계획 모드는 이제 모든 파일 쓰기를 무조건 막습니다(v2.1.136). `permissions.allow`에 일치하는 `Edit(...)` 규칙이 있어도 마찬가지입니다. 이전에는 허용적인 `Edit(...)` 규칙이 있으면 계획 모드에서도 쓰기가 통과될 수 있었는데, 그 우회로를 막았습니다. 예전 동작에 의존하던 워크플로는 편집하기 전에 계획 모드를 빠져나와야 합니다(`Shift+Tab`). `09-advanced-features/README.md`에 문서화했습니다.
- 띄어 쓴 플러그인 슬래시 명령(예: `/myplugin review`)이 이제 `/myplugin:review`로 해석됩니다. 플러그인 `skills` 설정 항목이 더 이상 기본 `skills/` 디렉터리를 가리지 않고 둘을 합칩니다. `07-plugins/README.md`에 문서화했습니다.
- MCP 서버가 이제 `/clear` 뒤에도 유지됩니다(v2.1.132+). `05-mcp/README.md`에 문서화했습니다.
- 서브에이전트가 Skill 도구로 프로젝트, 사용자, 플러그인 스킬을 찾습니다(v2.1.133). `04-subagents/README.md`에 문서화했습니다.
- 계획 모드 세션을 재개할 때 이제 `--permission-mode`가 적용됩니다(v2.1.132). `09-advanced-features/README.md`에 문서화했습니다.
- `CronList` 출력에 이제 한정자와 예약된 프롬프트 본문이 들어가므로(v2.1.136), 각 cron을 열지 않고도 무엇을 실행할지 점검할 수 있습니다. `09-advanced-features/README.md`에 문서화했습니다.

### 수정

- OAuth 갱신 토큰의 동시 갱신 경쟁 조건.
- INDEX.md 개수 어긋남: 스킬 28 → 16, 플러그인 40 → 27, 훅 스크립트 8 → 9(마크다운 내용 트리에서 다시 셈). 새 합계는 빌드 산출물과 설정이 아닌 튜토리얼 내용만 세는 `.md` 전용 방식을 반영합니다.
- `CATALOG.md`(v2.1.118 → v2.1.138)와 `claude_concepts_guide.md`(v2.1.117 → v2.1.138)의 낡은 출처 URL. 개념 가이드에 중복으로 남아 있던 예전 꼬리말도 지웠습니다.

### 번역 관리자 참고

`vi/`, `zh/`, `uk/`, `ja/` 번역 트리는 커뮤니티가 관리하며 영어 원문보다 늦을 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 갱신된 영어 파일과 비교해야 합니다.

## [v2.1.131] — 2026-05-06

### Claude Code v2.1.131에 동기화

튜토리얼 범위를 Claude Code v2.1.126 → v2.1.131(2026년 5월 6일 릴리스)로 올립니다. 지난 동기화 이후 Anthropic은 v2.1.128, v2.1.129, v2.1.131을 냈습니다. v2.1.127과 v2.1.130은 건너뛰었고 공개되지 않았습니다.

### 추가 (영어 문서)

- `--plugin-url <url>` 플래그 (v2.1.129) — 현재 세션용으로 URL에서 플러그인 `.zip` 아카이브를 받아 옵니다. 여러 번 쓸 수 있습니다. `07-plugins/README.md`에 문서화했습니다.
- `CLAUDE_CODE_FORCE_SYNC_OUTPUT` 환경 변수 (v2.1.129) — 자동 감지가 놓치는 터미널(예: Emacs `eat`)에서 동기 출력을 강제합니다. `10-cli/README.md`와 `09-advanced-features/README.md`에 문서화했습니다.
- `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` 환경 변수 (v2.1.129) — 보통 자동 업데이트되지 않는 Homebrew/WinGet 설치에서 백그라운드 업그레이드를 켭니다. `10-cli/README.md`와 `09-advanced-features/README.md`에 문서화했습니다.
- `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` 환경 변수 (v2.1.129) — `/v1/models` 게이트웨이 탐색을 켜는 데 필요합니다(아래 "변경" 참고). `10-cli/README.md`에 문서화했습니다.
- `disableRemoteControl` 설정 (v2.1.128) — 관리자가 관리형·정책 범위에서 `claude remote-control`과 `/remote-control`을 막을 수 있습니다. `09-advanced-features/README.md`에 문서화했습니다.
- `--plugin-dir`이 디렉터리와 함께 `.zip` 아카이브도 받음 (v2.1.128). `07-plugins/README.md`에 문서화했습니다.
- `skillOverrides`가 기존 `"on"`/`"off"`에 더해 `"name-only"`와 `"user-invocable-only"`를 받음 (v2.1.129). `03-skills/README.md`에 문서화했습니다.

### 변경

- **동작 변경**: 게이트웨이 `/v1/models` 탐색이 이제 **선택해서 켜는** 기능입니다(v2.1.129). 이전(v2.1.126)에는 `ANTHROPIC_BASE_URL`을 설정하면 게이트웨이의 `/v1/models` 엔드포인트로 `/model`이 자동으로 채워졌습니다. v2.1.129부터는 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`도 설정해야 하며, 이 환경 변수가 없으면 `/model`은 내장 고정 목록을 씁니다. `10-cli/README.md`에 문서화했습니다.
- `/mcp`가 서버별 도구 수를 보여 주고, 도구 0개를 보고하는 서버를 눈에 띄게 표시합니다(v2.1.128). `05-mcp/README.md`에 문서화했습니다.
- 인수 없는 `/color`는 무작위 세션 색을 고릅니다(v2.1.128). `/color <name|hex>`는 계속 특정 색을 정합니다. `01-slash-commands/README.md`에 문서화했습니다.
- `--channels` 플래그가 이제 API 키(콘솔) 인증에서도 동작합니다(v2.1.128). 이전 릴리스에서는 Pro/Max OAuth가 필요했습니다. `09-advanced-features/README.md`에 문서화했습니다.
- Ctrl+R 기록 선택기의 기본값이 **모든 프로젝트의 모든 프롬프트**가 되었습니다(v2.1.129). 선택기 안에서 Ctrl+S를 누르면 현재 프로젝트로 범위를 좁힙니다. `09-advanced-features/README.md`에 문서화했습니다.
- `/context`가 더 이상 ASCII 시각화를 대화에 쏟아 내지 않습니다(v2.1.129). 시각화는 UI에만 표시되어, 호출마다 들던 약 1.6k 토큰 비용이 없어졌습니다. `09-advanced-features/README.md`에 문서화했습니다.
- 끌어다 놓은 이미지가 너무 크면 자동으로 축소됩니다(v2.1.128). 이전 버전은 이미지를 그냥 거부했습니다.

### 수정

- Windows에서 VS Code 확장 프로그램 활성화 (v2.1.131).
- Mantle 엔드포인트 인증 (v2.1.131).
- 1시간 프롬프트 캐시 TTL이 더 이상 5분으로 잘리지 않음 (v2.1.129).
- 10MB를 넘는 stdin 페이로드에서 생기던 충돌 (v2.1.128).

### 번역 관리자 참고

`vi/`, `zh/`, `uk/`, `ja/` 번역 트리는 커뮤니티가 관리하며 영어 원문보다 늦을 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 갱신된 영어 파일과 비교해야 합니다.

## [v2.1.126] — 2026-05-02

### Claude Code v2.1.126에 동기화

튜토리얼 범위를 Claude Code v2.1.119 → v2.1.126(2026년 5월 1일 릴리스)으로 올립니다. v2.1.120은 첫 릴리스 당일(2026-04-24)에 롤백되었지만, 처음 보고된 회귀를 고쳐 2026-04-28에 다시 릴리스되었습니다. v2.1.124와 v2.1.125는 Anthropic이 건너뛰었고 릴리스되지 않았습니다.

### 추가 (영어 문서)

- `claude project purge [path]` 하위 명령 (v2.1.126) — 프로젝트의 모든 Claude Code 상태(대화 기록, 작업, 디버그 로그, 파일 편집 기록, 프롬프트 기록, `~/.claude.json` 항목)를 지웁니다. `--dry-run`, `-y/--yes`, `-i/--interactive`, `--all`을 지원합니다. `10-cli/README.md`에 문서화했습니다.
- `claude plugin prune` 하위 명령 (v2.1.121) — 자동 설치된 뒤 남겨진 플러그인 의존성을 지웁니다. `plugin uninstall --prune`은 연쇄로 지웁니다. `07-plugins/README.md`에 문서화했습니다.
- `claude ultrareview [target]` 하위 명령 (v2.1.120) — CI나 스크립트에서 `/ultrareview`를 비대화형으로 실행하고, 발견 사항을 stdout에 출력하며, 성공하면 0, 실패하면 1로 끝납니다. `--json`과 `--timeout <minutes>`를 지원합니다. `10-cli/README.md`에 문서화했습니다.
- 스킬 내용 안에서 쓸 수 있는 `${CLAUDE_EFFORT}` 자리 표시자 (v2.1.120) — 현재 effort 수준으로 바뀝니다. `03-skills/README.md`에 문서화했습니다.
- `alwaysLoad` MCP 서버 설정 옵션 (v2.1.121) — `true`이면 그 서버의 모든 도구가 도구 검색 지연 로드를 건너뜁니다. `05-mcp/README.md`에 문서화했습니다.
- `PostToolUse.hookSpecificOutput.updatedToolOutput`이 이제 모든 도구에서 동작합니다(v2.1.121). 이전에는 MCP 전용이었습니다. `06-hooks/README.md`에 문서화했습니다.
- `ANTHROPIC_BEDROCK_SERVICE_TIER` 환경 변수 (v2.1.122) — Bedrock 서비스 등급(`default`, `flex`, `priority`)을 고릅니다. `10-cli/README.md` 환경 변수 표에 문서화했습니다.
- `--dangerously-skip-permissions`의 경로 범위 확장 (v2.1.121, v2.1.126) — 이제 `.claude/skills/`, `.claude/agents/`, `.claude/commands/`, `.claude/`, `.git/`, `.vscode/`, 셸 설정 파일에 쓸 때도 확인을 건너뜁니다. 치명적인 삭제 명령(`rm -rf /` 등)은 여전히 묻습니다. `09-advanced-features/README.md` 권한 모드 단락에 문서화했습니다.
- OAuth 코드 붙여 넣기 대체 방법 (v2.1.126) — 브라우저 콜백이 localhost에 닿지 못할 때(WSL2, SSH, 컨테이너) `claude auth login`이 터미널에 붙여 넣은 OAuth 코드를 받습니다. `10-cli/README.md`에 문서화했습니다.
- 입력해서 거르는 `/skills` 메뉴 (v2.1.121). `03-skills/README.md`에 문서화했습니다.
- `AI_AGENT` 환경 변수 (v2.1.120) — 하위 프로세스에 설정되어 `gh`가 트래픽을 Claude Code의 것으로 구분할 수 있습니다. `10-cli/README.md` 환경 변수 표에 문서화했습니다.

### 변경

- `--from-pr`(v2.1.119)과 `/resume`의 PR URL 검색(v2.1.122)이 이제 모두 GitHub, GitHub Enterprise, GitLab, Bitbucket URL을 지원합니다.
- Windows: Git for Windows / Git Bash가 더 이상 필요하지 않습니다(v2.1.120). Git Bash가 없으면 Claude Code는 PowerShell을 셸 도구로 씁니다. v2.1.126부터는 PowerShell 도구가 켜져 있으면 PowerShell이 주 셸입니다. Microsoft Store, PATH 없는 MSI, `.NET global tool`로 설치한 PowerShell 7도 감지합니다. `09-advanced-features/README.md` 플랫폼 메모에 문서화했습니다.
- `ANTHROPIC_BASE_URL`이 Anthropic 호환 게이트웨이를 가리키면, `/model` 선택기가 이제 게이트웨이의 `/v1/models` 엔드포인트에서 모델 목록을 가져옵니다(v2.1.126). `10-cli/README.md`에 문서화했습니다.
- `--dangerously-skip-permissions`가 훨씬 넓어진 허용 목록에 대해 더 이상 쓰기 확인을 묻지 않습니다("추가" 참고). 치명적인 삭제는 여전히 묻습니다.
- 붙여 넣은 이미지 자동 축소 (v2.1.126) — 2000px보다 큰 이미지는 붙여 넣을 때 축소됩니다. 기록에 있는 너무 큰 이미지는 자동으로 지우고 요청을 다시 시도합니다(튜토리얼과는 안전·사용성 메모 정도로만 관련됨).

### 보안

- 우선순위가 더 높은 관리형 설정 출처에 `sandbox` 블록이 없을 때 `allowManagedDomainsOnly` / `allowManagedReadPathsOnly`가 무시되던 문제를 고쳤습니다(v2.1.126).

### 번역 관리자 참고

`vi/`, `zh/`, `uk/`, `ja/` 번역 트리는 커뮤니티가 관리하며 영어 원문보다 늦을 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 갱신된 영어 파일과 비교해야 합니다.

## [v2.4.0] — 2026-04-27

### Claude Code v2.1.119에 동기화

튜토리얼 범위를 Claude Code v2.1.112 → v2.1.119(2026년 4월 23일 릴리스)로 올립니다. v2.1.120은 4월 24일에 나왔다가 회귀 때문에 같은 날 잠시 롤백되었고, 4월 28일에 수정과 함께 다시 릴리스되어 지금은 정상 릴리스 라인에 들어 있습니다. 그다음 v2.1.126(2026년 5월 1일)이 다음 안정 버전이며, 위의 v2.1.126 항목에서 다룹니다.

### 추가 (영어 문서)

- 네이티브 바이너리 패키징 메모 (v2.1.113) — CLI가 이제 플랫폼별 네이티브 바이너리로 배포됩니다
- 네이티브 macOS/Linux 빌드에서 Glob/Grep을 `bfs`/`ugrep`으로 대체한다는 각주 (v2.1.117)
- 예제를 곁들인 `mcp_tool` 훅 유형 (v2.1.118)
- PostToolUse / PostToolUseFailure 입력의 `duration_ms` 필드 (v2.1.119)
- `prUrlTemplate` 설정 (v2.1.119)과 넓어진 `--from-pr` 제공자 목록 (GitLab, Bitbucket)
- `cleanupPeriodDays`의 범위 확장 (체크포인트 + 작업 + 셸 스냅숏 + 백업, v2.1.117)
- 모든 수명 주기 이벤트에서 플러그인 마켓플레이스 정책 적용 (v2.1.117)과 `hostPattern`/`pathPattern` 정규식 (v2.1.119)
- 새 환경 변수: `DISABLE_UPDATES`, `CLAUDE_CODE_HIDE_CWD`, `CLAUDE_CODE_FORK_SUBAGENT`, `OTEL_LOG_TOOL_DETAILS`, Vertex에서 켜는 `ENABLE_TOOL_SEARCH`
- 새 슬래시 명령: `/btw`, 맞춤 테마를 지원하는 `/theme`
- 정식 명령 `/usage` (`/cost`와 `/stats`를 합침, v2.1.118)
- 포크된 서브에이전트 (`CLAUDE_CODE_FORK_SUBAGENT=1`, v2.1.117)
- auto 모드 `"$defaults"` 토큰 (v2.1.118)
- `wslInheritsWindowsSettings` 관리형 정책 (v2.1.118)
- Vim 비주얼 / 비주얼 라인 모드 (v2.1.118)
- `claude install [version]`과 `claude plugin tag` 하위 명령

### 변경

- 문서 호스트 이전: `docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 effort 수준: 2026-04-16 출시 이후 `xhigh`가 Claude Code 기본값입니다. Opus 4.7의 기본 컨텍스트 윈도가 1M임을 확인했습니다(v2.1.117에서 `/context`가 200K로 잘못 세던 문제를 고침)
- Opus 4.6 / Sonnet 4.6을 쓰는 Pro/Max 구독자의 기본 effort가 `medium`에서 `high`로 올라감 (v2.1.117)
- `STYLE_GUIDE.md`의 출처 URL을 Claude Apps 글에서 `code.claude.com/docs/en/changelog`로 바꿈

### 지원 중단 (추적 중, 제거되지 않음)

- `includeCoAuthoredBy` 설정 → `attribution.commit` / `attribution.pr` 사용
- `voiceEnabled` 설정 → `voice.enabled` 사용

### 번역 관리자 참고

`vi/`, `zh/`, `uk/` 번역 트리는 커뮤니티가 관리하며 영어 원문보다 늦을 수 있습니다. 번역을 동기화하는 기여자는 이 릴리스에서 갱신된 영어 파일과 비교해야 합니다.

## v2.1.112 — 2026-04-16

### 주요 내용

- 모든 영어 튜토리얼을 Claude Code v2.1.112와 새 Opus 4.7 모델(`claude-opus-4-7`)에 맞춰 동기화했습니다. 새 `xhigh` effort 수준(Opus 4.7의 기본값, `high`와 `max` 사이), 새 내장 슬래시 명령 두 개(`/ultrareview`, `/less-permission-prompts`), Opus 4.7을 쓰는 Max 구독자는 더 이상 `--enable-auto-mode` 없이 auto 모드를 쓸 수 있다는 점, Windows의 PowerShell 도구, "Auto (match terminal)" 테마, 프롬프트 이름을 딴 계획 파일을 포함합니다. 영어 문서 꼬리말 18개를 모두 Claude Code v2.1.112로 올렸습니다. @Luong NGUYEN

### 기능

- 모든 모듈, 루트 문서, 예제, 참고 자료에 우크라이나어(uk) 번역을 완성해 추가 (039dde2) @Evgenij I

### 버그 수정

- pre-tool-check.sh 훅 프로토콜 버그 수정 (bce7cf8) @yarlinghe
- CI를 통과하도록 잘못된 mermaid 예제를 text 블록으로 바꿈 (b8a7b1f) @Evgenij I
- 우크라이나어 claude_concepts_guide.md 목차의 CP1251 인코딩 수정 (d970cc6) @Evgenij I
- 우크라이나어 README 초안을 전체 번역으로 바꾸고 깨진 앵커 수정 (f6d73e2) @Evgenij I
- 모든 꼬리말의 Claude Code 버전을 2.1.97로 바로잡음 (63a1416) @Luong NGUYEN
- 2026-04-09 문서 정확성 갱신 적용 (e015f39) @Luong NGUYEN

### 문서

- Claude Code v2.1.112에 동기화 (Opus 4.7, `xhigh` effort, `/ultrareview`, `/less-permission-prompts`, PowerShell 도구, Auto-match-terminal 테마) @Luong NGUYEN
- Claude Code v2.1.110에 동기화 (TUI, 푸시 알림, 세션 요약) (15f0085) @Luong NGUYEN
- `/team-onboarding`, `/ultraplan`, Monitor 도구와 함께 Claude Code v2.1.101에 동기화 (2deba3a) @Luong NGUYEN
- 베트남어 문서를 영어 원문과 동기화 (561c6cb) @Thiên Toán
- 모든 파일의 Last Updated 날짜와 Claude Code 버전 갱신 (7f2e773) @Luong NGUYEN
- 언어 전환기에 우크라이나어 링크 추가 (9c224ff) @Luong NGUYEN
- 기여자 단락 제거 (f07313d) @Luong NGUYEN
- GitHub 지표를 스타 21,800개 이상, 포크 2,585개 이상으로 갱신 (4f55374) @Luong NGUYEN

**전체 변경 기록**: https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### 기능

- 언어별 EPUB 결과물 빌드와 게시 (90e9c30) @Thiên Toán
- 06-hooks에 빠져 있던 pre-tool-check.sh 훅 추가 (b511ed1) @JiayuWang
- zh/ 디렉터리에 중국어 번역 추가 (89e89d4) @Luong NGUYEN
- performance-optimizer 서브에이전트와 dependency-check 훅 추가 (f53d080) @qk

### 버그 수정

- Windows Git Bash 호환성과 stdin JSON 프로토콜 (2cbb10c) @Luong NGUYEN
- 08-checkpoints의 autoCheckpoint 설정 문서 바로잡음 (749c79f) @JiayuWang
- SVG 이미지를 자리 표시자로 바꾸지 않고 삽입 (1b16709) @Thiên Toán
- 메모리 README의 중첩 코드 펜스 렌더링 (ce24423) @Zhaoshan Duan
- squash 병합에서 빠진 리뷰 수정 사항 적용 (34259ca) @Luong NGUYEN
- 훅 스크립트를 Windows Git Bash와 호환되게 하고 stdin JSON 프로토콜 사용 (107153d) @binyu li

### 문서

- 모든 튜토리얼을 최신 Claude Code 문서와 동기화 (2026년 4월) (72d3b01) @Luong NGUYEN
- 언어 전환기에 중국어 링크 추가 (6cbaa4d) @Luong NGUYEN
- 영어와 베트남어 사이 언어 전환기 추가 (100c45e) @Luong NGUYEN
- GitHub #1 Trending 배지 추가 (0ca8c37) @Luong NGUYEN
- 컨텍스트 영역 모니터링용 cc-context-stats 소개 (d41b335) @Luong NGUYEN
- luongnv89/skills 모음과 luongnv89/asm 스킬 관리자 소개 (7e3c0b6) @Luong NGUYEN
- README 통계를 현재 GitHub 지표(스타 5,900개 이상, 포크 690개 이상)로 갱신 (5001525) @Luong NGUYEN
- README 통계를 현재 GitHub 지표(스타 3,900개 이상, 포크 460개 이상)로 갱신 (9cb92d6) @Luong NGUYEN

### 리팩터링

- Kroki HTTP 의존성을 로컬 mmdc 렌더링으로 교체 (e76bbe4) @Luong NGUYEN
- 품질 검사를 pre-commit으로 옮기고 CI는 두 번째 검사로 둠 (6d1e0ae) @Luong NGUYEN
- auto 모드 권한 기준선 축소 (2790fb2) @Luong NGUYEN
- auto-adapt 훅을 한 번 실행하는 권한 설정 스크립트로 교체 (995a5d6) @Luong NGUYEN

### 기타

- 품질 관문을 앞당김 — pre-commit에 mypy 추가, CI 실패 수정 (699fb39) @Luong NGUYEN
- 베트남어(Tiếng Việt) 번역 추가 (a70777e) @Thiên Toán

**전체 변경 기록**: https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### 문서

- 모든 튜토리얼과 참고 자료를 Claude Code v2.1.84와 동기화 (f78c094) @luongnv89
  - 슬래시 명령을 내장 55개 이상 + 번들 스킬 5개로 갱신하고, 3개를 지원 중단으로 표시
  - 훅 이벤트를 18개에서 25개로 늘리고 `agent` 훅 유형 추가 (이제 4가지)
  - 고급 기능에 Auto Mode, Channels, Voice Dictation 추가
  - 스킬 프런트매터 필드 `effort`, `shell`과 에이전트 필드 `initialPrompt`, `disallowedTools` 추가
  - WebSocket MCP 전송 방식, elicitation, 2KB 도구 한도 추가
  - 플러그인 LSP 지원, `userConfig`, `${CLAUDE_PLUGIN_DATA}` 추가
  - 모든 참고 문서(CATALOG, QUICK_REFERENCE, LEARNING-ROADMAP, INDEX) 갱신
- README를 랜딩 페이지 구조의 안내서로 새로 씀 (32a0776) @luongnv89

### 버그 수정

- CI 준수를 위해 빠져 있던 cSpell 단어와 README 단락 추가 (93f9d51) @luongnv89
- cSpell 사전에 `Sandboxing` 추가 (b80ce6f) @luongnv89

**전체 변경 기록**: https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### 버그 수정

- CI 링크 검사에서 실패하던 죽은 마켓플레이스 링크 제거 (3fdf0d6) @luongnv89
- cSpell 사전에 `sandboxed`와 `pycache` 추가 (dc64618) @luongnv89

**전체 변경 기록**: https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 기능

- 자가 평가와 레슨 퀴즈 스킬로 적응형 학습 경로 추가 (1ef46cd) @luongnv89
  - `/self-assessment` — 기능 영역 10가지에 걸친 대화형 숙련도 퀴즈와 맞춤 학습 경로
  - `/lesson-quiz [lesson]` — 목표를 정한 질문 8~10개로 레슨별 이해도 확인

### 버그 수정

- 깨진 URL, 지원 중단 사항, 낡은 참조 갱신 (8fe4520) @luongnv89
- resources와 self-assessment 스킬의 깨진 링크 수정 (7a05863) @luongnv89
- 개념 가이드의 중첩 코드 블록에 물결표 펜스 사용 (5f82719) @VikalpP
- cSpell 사전에 빠진 단어 추가 (8df7572) @luongnv89

### 문서

- 5단계 QA — 문서 전반의 일관성, URL, 용어 수정 (00bbe4c) @luongnv89
- 3~4단계 완료 — 새 기능 범위와 참고 문서 갱신 (132de29) @luongnv89
- MCP 컨텍스트 비대 단락에 MCPorter 런타임 추가 (ef52705) @luongnv89
- 안내서 6개에 빠진 명령, 기능, 설정 추가 (4bc8f15) @luongnv89
- 기존 저장소 관례를 바탕으로 스타일 가이드 추가 (84141d0) @luongnv89
- 가이드 비교표에 자가 평가 행 추가 (8fe0c96) @luongnv89
- PR #7 기여자 목록에 VikalpP 추가 (d5b4350) @luongnv89
- README와 로드맵에 자가 평가와 레슨 퀴즈 스킬 참조 추가 (d5a6106) @luongnv89

### 새 기여자

- @VikalpP 님이 #7에서 첫 기여를 했습니다

**전체 변경 기록**: https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 기능

- 모든 문서를 Claude Code 2026년 2월 기능과 동기화 (487c96d)
  - 튜토리얼 디렉터리 10개 전체와 참고 문서 7개에 걸쳐 파일 26개 갱신
  - **자동 메모리**(Auto Memory) 문서 추가 — 프로젝트별로 유지되는 학습 내용
  - **원격 제어**(Remote Control), **웹 세션**(Web Sessions), **데스크톱 앱**(Desktop App) 문서 추가
  - **에이전트 팀**(Agent Teams, 실험적 다중 에이전트 협업) 문서 추가
  - **MCP OAuth 2.0**, **도구 검색**(Tool Search), **Claude.ai 커넥터** 문서 추가
  - 서브에이전트용 **영구 메모리**(Persistent Memory)와 **워크트리 격리**(Worktree Isolation) 문서 추가
  - **백그라운드 서브에이전트**, **작업 목록**(Task List), **프롬프트 제안**(Prompt Suggestions) 문서 추가
  - **샌드박스**(Sandboxing)와 **관리형 설정**(Managed Settings, Enterprise) 문서 추가
  - **HTTP 훅**과 새 훅 이벤트 7개 문서 추가
  - **플러그인 설정**, **LSP 서버**, 마켓플레이스 갱신 사항 문서 추가
  - 되감기 옵션 **Summarize from Checkpoint** 문서 추가
  - 새 슬래시 명령 17개 문서화 (`/fork`, `/desktop`, `/teleport`, `/tasks`, `/fast` 등)
  - 새 CLI 플래그 문서화 (`--worktree`, `--from-pr`, `--remote`, `--teleport`, `--teammate-mode` 등)
  - 자동 메모리, effort 수준, 에이전트 팀 등에 쓰는 새 환경 변수 문서화

### 디자인

- 로고를 최소한의 색상을 쓰는 나침반·괄호 모양으로 다시 디자인 (20779db)

### 버그 수정 / 바로잡기

- 모델 이름 갱신: Sonnet 4.5 → **Sonnet 4.6**, Opus 4.5 → **Opus 4.6**
- 권한 모드 이름 수정: 지어낸 "Unrestricted/Confirm/Read-only"를 실제 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`로 교체
- 훅 이벤트 수정: 지어낸 `PreCommit`/`PostCommit`/`PrePush`를 지우고 실제 이벤트(`SubagentStart`, `WorktreeCreate`, `ConfigChange` 등) 추가
- CLI 문법 수정: `claude-code --headless`를 `claude -p`(print 모드)로 교체
- 체크포인트 명령 수정: 지어낸 `/checkpoint save/list/rewind/diff`를 실제 `Esc+Esc` / `/rewind` 인터페이스로 교체
- 세션 관리 수정: 지어낸 `/session list/new/switch/save`를 실제 `/resume`/`/rename`/`/fork`로 교체
- 플러그인 매니페스트 형식 수정: `plugin.yaml` → `.claude-plugin/plugin.json`으로 이전
- MCP 설정 경로 수정: `~/.claude/mcp.json` → `.mcp.json`(프로젝트) / `~/.claude.json`(사용자)
- 문서 URL 수정: `docs.claude.com` → `docs.anthropic.com`. 지어낸 `plugins.claude.com` 제거
- 여러 파일에서 지어낸 설정 필드 제거
- 모든 "Last Updated" 날짜를 2026년 2월로 갱신

**전체 변경 기록**: https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0

---

**마지막 업데이트**: 2026년 8월 25일
**Claude Code 버전**: 2.1.245
**출처**:
- https://code.claude.com/docs/en/changelog
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
