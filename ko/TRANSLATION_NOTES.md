# 번역 용어집 및 스타일 가이드

# Translation Glossary & Style Guide

> **중요:** 이 문서는 Claude Code 문서를 한국어로 번역할 때 지킬 규칙을 정합니다. 번역을 시작하기 전에 꼭 읽어 주세요.
>
> 작업 진행 방식과 진행 상황은 [TRANSLATION_WORKFLOW.md](TRANSLATION_WORKFLOW.md)에 있습니다. 두 파일을 함께 읽으세요.

## 기본 방침

- **문체:** 합니다체(경어체). 예: "이 명령은 코드를 분석합니다."
- **용어 방침:** IT 업계에서 이미 자리 잡은 외래어 표기를 우선합니다(예: 커밋, 브랜치). 정착된 한자어가 있으면 그것을 씁니다(예: 환경 변수, 권한).
- **코드 유지:** 실행되는 코드는 100% 그대로 둡니다. 주석과 설명문만 번역합니다.
- **Mermaid 다이어그램:** 레이블 텍스트는 영어 그대로 둡니다.
- **원문 추적:** 파일마다 맨 앞에 `i18n-source-sha`를 넣어 원문과 다시 맞출 수 있게 합니다.
- **띄어쓰기:** 영어 용어와 한국어 조사는 붙여 씁니다(예: `CLAUDE.md를`, `MCP 서버를`).

---

## 기술 용어집

모든 파일에서 똑같이 쓰기 위한 대역표입니다.

| English | 한국어 | 비고 |
|---------|--------|------|
| slash command | 슬래시 명령 | Claude Code 기능 이름 |
| command | 명령 | 문맥에 따라 "커맨드"도 허용하지만 "명령"을 우선 |
| hook | 훅 | 업계에서 정착된 표기 |
| skill | 스킬 | Claude Code 기능 이름 |
| subagent | 서브에이전트 | Claude Code 기능 이름, 붙여 씀 |
| agent | 에이전트 | 일반 외래어 표기 |
| memory | 메모리 | Claude Code 기능 이름. "기억"으로 번역하지 않음 |
| checkpoint | 체크포인트 | 세션 스냅샷 |
| plugin | 플러그인 | 일반 용어 |
| pull request / PR | 풀 리퀘스트 / PR | GitHub 용어 |
| commit | 커밋 | Git 용어 |
| branch | 브랜치 | Git 용어 |
| merge | 병합 / 머지 | "병합"을 우선, 명령어 문맥에서는 "머지" |
| MCP (Model Context Protocol) | MCP | 프로토콜 이름은 그대로 |
| CLAUDE.md | CLAUDE.md | 파일 이름은 그대로 |
| prompt | 프롬프트 | 정착된 외래어 |
| workflow | 워크플로 | 국립국어원 표기("워크플로우" 아님) |
| repository | 저장소 | "리포지토리"보다 "저장소"를 우선 |
| issue | 이슈 | GitHub 용어 |
| release | 릴리스 | "릴리즈" 아님 |
| API | API | 그대로 |
| CLI | CLI | 그대로 |
| CI/CD | CI/CD | 그대로 |
| pre-commit hook | pre-commit 훅 | 도구 이름은 그대로 |
| environment variable | 환경 변수 | 정착된 번역어 |
| dependencies | 의존성 | 정착된 번역어 |
| template | 템플릿 | 외래어 |
| worktree | 워크트리 | Git 용어 |
| frontmatter | 프런트매터 | YAML 머리 블록. 처음 나올 때 `frontmatter` 병기 |
| token | 토큰 | 외래어 |
| context window | 컨텍스트 윈도 | 국립국어원 표기("윈도우" 아님) |
| context | 컨텍스트 | 문맥에 따라 "맥락"도 허용 |
| fork | 포크 | Git 용어 |
| clone | 클론 / 복제 | 명령어 문맥에서는 "클론" |
| sandbox | 샌드박스 | 외래어 |
| boilerplate | 보일러플레이트 | 외래어 |
| debugging | 디버깅 | 외래어 |
| linting | 린트 / 린팅 | 외래어 |
| refactoring | 리팩터링 | "리팩토링" 아님 |
| build | 빌드 | 외래어 |
| deploy | 배포 | 정착된 번역어 |
| feature | 기능 | 정착된 번역어 |
| user | 사용자 | "유저" 아님 |
| developer | 개발자 | 정착된 번역어 |
| documentation | 문서 | 정착된 번역어 |
| roadmap | 로드맵 | 외래어 |
| ecosystem | 생태계 | 정착된 번역어 |
| orchestration | 오케스트레이션 | 외래어 |
| permission | 권한 | 정착된 번역어 |
| settings | 설정 | 정착된 번역어 |
| configuration | 구성 / 설정 | "설정"을 우선 |
| trigger | 트리거 | 외래어 |
| event | 이벤트 | 외래어 |
| script | 스크립트 | 외래어 |
| handler | 핸들러 | 외래어 |
| wrapper | 래퍼 | 외래어 |
| middleware | 미들웨어 | 외래어 |
| pipeline | 파이프라인 | 외래어 |
| best practice | 모범 사례 | "베스트 프랙티스" 아님 |
| use case | 사용 사례 | 문맥에 따라 "활용 사례" |
| trade-off | 트레이드오프 | 외래어 |
| argument | 인수 | 명령에 넘기는 값 |
| session | 세션 | 외래어 |
| tutorial | 튜토리얼 | 외래어 |
| module | 모듈 | 외래어 |
| example | 예시 / 예제 | 코드면 "예제" |
| tip | 팁 | 외래어 |
| troubleshooting | 문제 해결 | 정착된 번역어 |

---

## Claude Code 고유 명사 (절대 변경 불가)

아래는 Claude Code의 **제품 이름과 기능 이름**이므로 **번역하지 않고 영어 그대로** 씁니다.

| 표기 | 종류 | 참고 |
|------|------|------|
| Claude | 제품 이름 | "클로드"로 쓰지 않음 |
| Claude Code | 제품 이름 | 그대로 |
| Anthropic | 회사 이름 | "앤트로픽"으로 쓰지 않음 |
| CLAUDE.md | 파일 이름 | 대문자 유지 |
| SKILL.md | 파일 이름 | 대문자 유지 |
| MCP / Model Context Protocol | 프로토콜 이름 | 약어를 쓰고, 처음 나올 때 풀어서 병기 |
| `.claude/` | 디렉터리 이름 | 그대로 |
| `~/.claude/` | 경로 | 그대로 |
| `claude.ai` / `claude.com` / `code.claude.com` | URL | 그대로 |
| Sonnet / Opus / Haiku | 모델 이름 | 한글로 쓰지 않음 |

다만 **기능 분류 이름**은 한국어로 씁니다.

| English | 한국어 | 비고 |
|---------|--------|------|
| Slash Commands (기능 이름) | 슬래시 명령 | 제목에서도 번역 |
| Memory (기능 이름) | 메모리 | "기억"으로 번역하지 않음 |
| Skills (기능 이름) | 스킬 | 그대로 |
| Subagents (기능 이름) | 서브에이전트 | "하위 에이전트"는 쓰지 않음 |
| Hooks (기능 이름) | 훅 | 그대로 |
| Plugins (기능 이름) | 플러그인 | 그대로 |
| Checkpoints (기능 이름) | 체크포인트 | 그대로 |
| Advanced Features (분류) | 고급 기능 | 번역 |
| CLI Reference (분류) | CLI 레퍼런스 | 번역 |

---

## 모듈 이름 다루기

모듈 제목과 URL은 이렇게 다룹니다.

| 원문 제목 | 한국어 | URL (변경하지 않음) |
|-----------|--------|--------------------|
| 01 Slash Commands | 슬래시 명령 | `01-slash-commands/` |
| 02 Memory | 메모리 | `02-memory/` |
| 03 Skills | 스킬 | `03-skills/` |
| 04 Subagents | 서브에이전트 | `04-subagents/` |
| 05 MCP | MCP | `05-mcp/` |
| 06 Hooks | 훅 | `06-hooks/` |
| 07 Plugins | 플러그인 | `07-plugins/` |
| 08 Checkpoints | 체크포인트 | `08-checkpoints/` |
| 09 Advanced Features | 고급 기능 | `09-advanced-features/` |
| 10 CLI | CLI | `10-cli/` |

**중요:** 디렉터리 이름과 파일 경로는 절대 바꾸지 않습니다. 한국어로 바꾸는 것은 제목과 본문 속 언급뿐입니다.

---

## 번역 규칙

### 1. 코드와 명령어

**황금률:** 실행할 수 있는 코드는 100% 그대로 둡니다. 번역하는 것은 주석과 설명문뿐입니다.

**올바른 예 (✅):**

````markdown
이 명령을 실행하려면:

```bash
/optimize
```

이 명령은 코드를 분석합니다.
````

**잘못된 예 (❌):**

````markdown
```bash
/최적화  # 명령 이름은 절대 번역하지 않음
```
````

### 2. 코드 안의 주석

주석은 한국어로 번역합니다. 함수, 변수, 클래스 이름은 영어 그대로 둡니다.

```python
# ✅ 올바름: 주석만 번역
# 이 슬래시 명령은 코드를 최적화합니다
def create_subagent(name: str, system_prompt: str):
    pass
```

### 3. Mermaid 다이어그램

**100% 그대로 둡니다.** mermaid 블록 안의 텍스트는 번역하지 않습니다.

### 4. 링크와 앵커

- 내부 링크는 **상대 경로**를 씁니다. `ko/` 안에서 서로 가리키는 링크는 `ko/` 안의 파일을 가리키게 합니다.
- 아직 번역되지 않은 파일은 원문(`../01-slash-commands/README.md` 같은 경로)으로 연결합니다.
- 제목을 번역하면 앵커도 바뀝니다. 목차 링크는 번역된 제목에 맞춰 고칩니다(예: `#슬래시-명령`).
- 외부 URL은 바꾸지 않습니다.

### 5. 코드 블록 언어 표시

모든 코드 블록에는 언어를 표시합니다(`bash`, `json`, `markdown` 등). 원문에 표시가 있으면 그대로 둡니다.

### 6. 파일 머리말

번역 파일마다 맨 앞에 다음을 넣습니다.

```markdown
<!-- i18n-source: 01-slash-commands/README.md -->
<!-- i18n-source-sha: <원문 커밋 SHA 앞 7자리> -->
<!-- i18n-date: YYYY-MM-DD -->
```

---

## 문체 예시

| 원문 | ✅ 권장 | ❌ 피함 |
|------|---------|---------|
| Run the following command. | 다음 명령을 실행합니다. | 다음 명령을 실행하라. |
| You can use skills to... | 스킬을 사용하면 ...할 수 있습니다. | 당신은 스킬을 사용하여 ...할 수 있다. |
| Note: | 참고: | 노트: |
| Tip: | 팁: | 꿀팁: |
| Warning: | 주의: | 경고!! |

- "당신"은 쓰지 않습니다. 주어를 빼거나 "사용자"로 씁니다.
- 영어 수동태는 능동형으로 자연스럽게 바꿉니다.
- 한 문장이 너무 길면 둘로 나눕니다.
