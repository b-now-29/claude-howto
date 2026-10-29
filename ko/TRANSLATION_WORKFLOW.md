# 한국어 번역 작업 규칙과 진행 상황

> 이 파일은 `ko/TRANSLATION_NOTES.md`(용어집과 문체)와 **함께** 읽습니다. 새 세션이나 컨텍스트 초기화 뒤에는 두 파일을 먼저 읽고 작업을 이어 갑니다.

## 이어서 작업하는 방법

새 세션의 첫 메시지 예시:

```text
claude/fervent-mendel-02w63r 브랜치를 가져와서 ko/TRANSLATION_NOTES.md와 ko/TRANSLATION_WORKFLOW.md를 읽고, 다음 챕터 번역을 이어서 해 줘.
```

## 작업 규칙

### 진행 방식
- 한 번에 한 챕터씩 번역합니다. 챕터를 마치면 커밋하고 푸시한 뒤, 다음 챕터는 사용자가 요청할 때까지 기다립니다.
- 원문이 200줄을 넘는 파일은 단락(`##` 제목) 단위로 나눠 번역합니다.
- 작업을 시작하기 전과 푸시하기 전에 원격 브랜치를 받아 옵니다 (`git fetch origin claude/fervent-mendel-02w63r`). 사용자가 GitHub에서 직접 고친 커밋이 있으면 rebase하지 말고 병합해서 그 커밋을 그대로 둡니다.
- 컨텍스트 사용량이 60%를 넘으면, 지금 하던 챕터를 끝내고 커밋·푸시한 뒤 사용자에게 컨텍스트 초기화(`/clear` 또는 새 세션)를 요청합니다. 챕터 도중에는 초기화하지 않습니다.

### Git
- 브랜치: `claude/fervent-mendel-02w63r` (저장소 `b-now-29/claude-howto`)
- 커밋 메시지: `docs(ko): translate <모듈 폴더> module to Korean`
- 커밋 메시지에 `Co-Authored-By` 줄을 넣지 않습니다 (저장소 `CLAUDE.md` 규칙).
- PR은 사용자가 요청할 때만 만듭니다.

### 파일 다루기
- 번역 파일은 원문과 같은 경로로 `ko/` 아래에 둡니다 (예: `05-mcp/README.md` → `ko/05-mcp/README.md`).
- 파일마다 원문 표시 주석(`i18n-source`, `i18n-source-sha`, `i18n-date`)을 넣습니다.
  - 일반 문서: 파일 맨 위
  - YAML 프런트매터가 있는 파일(스킬, 명령, 에이전트 템플릿): 프런트매터 **바로 아래**. 맨 위에 두면 복사해서 쓸 때 프런트매터가 인식되지 않습니다.
- 템플릿 프런트매터의 `description`과 본문 지침은 번역합니다. `name`, `tools`, `allowed-tools`, `model` 같은 키와 값은 그대로 둡니다.
- `.py`, `.sh`, `.json`, `.txt` 같은 스크립트·설정 파일은 번역하지 않고 `ko/`에 복사하지도 않습니다.
- 이미지는 복사하지 않고 원문 폴더의 이미지를 상대 경로로 가리킵니다 (예: `../../01-slash-commands/pr-slash-command.png`).

### 내용 다루기
- 실행되는 코드와 명령은 그대로 두고, 코드 블록 안의 주석만 번역합니다. 번역 뒤 코드 블록이 주석을 빼면 원문과 같은지 확인합니다.
- Mermaid 다이어그램은 영어 그대로 둡니다.
- 사용자가 Claude에게 입력하는 예시 문장(`User: ...`, `> Use the ... subagent` 등)은 영어로 둡니다.
- 에이전트 설명의 `PROACTIVELY` 같은 자동 호출 키워드는 "적극적으로(PROACTIVELY)"처럼 원어를 함께 남깁니다.
- 리팩터링 기법, 코드 스멜 같은 고유 기법 이름은 영어 원어를 쓰고 처음 나올 때 우리말 뜻을 붙입니다.
- 원문에 언어 표시가 없는 코드 블록에는 `text`를 붙입니다.
- 원문에 없는 내용은 꼭 필요할 때만 짧게 추가합니다 (예: 설치 단락에 한국어판 경로 안내 한 줄).

### 링크
- `ko/` 안에 번역본이 있는 파일은 번역본을 가리킵니다. 없는 파일은 원문을 가리킵니다 (`ko/<모듈>/`에서는 `../../<원문 경로>`).
- 챕터를 하나 마칠 때마다, 이미 번역한 문서에 있는 그 챕터 링크를 번역본으로 바꿉니다.
- 같은 문서 안의 앵커는 한국어 제목에 맞춥니다 (예: `#자동-메모리`). 제목이 겹쳐 앵커가 충돌하면 한쪽 제목을 조금 바꿉니다.
- 아직 번역하지 않은 다른 문서의 앵커는 영어 그대로 둡니다.
- 커밋하기 전에 `python3 scripts/check_cross_references.py`를 실행합니다. 전체 품질 검사는 아래 "품질 검사" 단락을 따릅니다.

## 품질 검사

저장소 `CLAUDE.md`는 커밋 전에 `pre-commit run --all-files`를 요구합니다.

**작업 환경 방침 (2026-10-29 결정)**
- 품질 검사의 기준 환경은 **로컬**입니다. 커밋 전 최종 검사는 로컬에서 정식 절차대로 돌립니다.
- 클라우드 세션에서는 번역과 아래 우회 방법으로 할 수 있는 검사까지만 합니다.

**첫 검사 결과 (2026-10-29, 클라우드 세션, `2170c0a` 기준)**
- `ko/` 대상 검사는 모두 통과했습니다. 번역 문제는 없었습니다.
- 환경 문제 1: pre-commit 4.7.0에서 `mermaid` 훅이 `` `language: node` must have package.json or additional_dependencies `` 오류로 멈춥니다. 저장소 설정 쪽 문제이므로 로컬에서도 생길 수 있습니다.
- 환경 문제 2: Pele 번들 출처 `cdn.jsdelivr.net`이 클라우드 프록시에서 403으로 막힙니다. npm 레지스트리는 열려 있습니다.

**1. 가상환경과 도구 설치**

```bash
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt pre-commit
```

**2. `ko/` 파일만 검사**

`--all-files` 대신 `ko/`만 지정합니다. `trailing-whitespace`, `end-of-file-fixer` 훅은 파일을 직접 고치므로, 전체로 돌리면 번역과 상관없는 파일까지 바뀔 수 있습니다. `mermaid` 훅 오류가 나면 mermaid 훅만 건너뜁니다.

```bash
SKIP=mermaid,vietnamese-mermaid,japanese-mermaid pre-commit run --files $(git ls-files ko)
```

**3. Mermaid 검사 (클라우드 세션)**

npm 레지스트리에서 같은 버전의 Pele를 받아 캐시에 넣습니다. 스크립트가 sha256을 검증하므로 파일이 다르면 검사가 실패합니다(2026-10-29에 세 파일 모두 일치 확인).

```bash
curl -sS -o /tmp/pele.tgz https://registry.npmjs.org/pele/-/pele-1.6.0.tgz
tar xzf /tmp/pele.tgz -C /tmp
mkdir -p scripts/.vendor-cache/pele/1.6.0
cp /tmp/package/dist/pele.js scripts/.vendor-cache/pele/1.6.0/pele.mjs
cp /tmp/package/dist/pele.global.js /tmp/package/LICENSE scripts/.vendor-cache/pele/1.6.0/
PATH=/opt/node22/bin:$PATH python scripts/check_mermaid.py --strict
```

로컬에서는 마지막 줄만 실행합니다(Node.js 20 이상 필요).

**주의**
- 기본 문서 검사 5종(`markdown-lint`, `cross-references`, `mermaid`, `link-check`, `markdown-rendering`)은 경로 제한이 없어 `ko/`에도 적용됩니다. `vi/`, `ja/` 같은 언어 전용 검사만 `ko/`에 없습니다.
- `link-check`는 외부 URL에 실제로 접속합니다. 실패하면 결과를 **번역 문제**와 **환경(네트워크) 문제**로 나눠 사용자에게 보고합니다.
- 클라우드 컨테이너는 세션이 끝나면 사라지므로 새 세션마다 1단계와 3단계를 다시 합니다.
- 검사에서 나온 번역 문제는 고치되, 커밋·푸시는 사용자가 요청할 때 합니다.

## 진행 상황

| 챕터 | 상태 | 커밋 |
|------|------|------|
| 용어집, 루트 `README.md` | 완료 | `3355cd7` |
| 01-slash-commands | 완료 | `651da48` |
| 02-memory | 완료 | `bd2353d` |
| 03-skills | 완료 | `33c6513` |
| 04-subagents | 완료 | `5a1ba05` |
| 05-mcp | 완료 | `a01461f` |
| 06-hooks | 완료 | `21db23a` |
| 07-plugins | 완료 | `43464a9` |
| 08-checkpoints | 완료 | `038f6c4` |
| 09-advanced-features | 완료 | `f481ec7` |
| 10-cli | 완료 | `c18e365` |
| 루트 문서 (아래 목록) | 진행 중 | |

### 루트 문서 번역 범위

기여·정책 문서(1–4)를 먼저 번역하고, 이어서 학습자용 문서(5–11)와 `CHANGELOG.md`(12)를 번역합니다. 다른 언어판(vi, ja, zh, uk)처럼 기여·정책 문서도 번역 대상입니다. 관리용 파일만 원문으로 둡니다.

**번역함** (위에서부터 차례로):

| 순서 | 파일 | 줄 수 | 이유 | 상태 |
|------|------|-------|------|------|
| 1 | `CONTRIBUTING.md` | 388 | 기여 안내. 한국어 사용자의 기여 진입점 | 완료 (`2d0d1e5`) |
| 2 | `STYLE_GUIDE.md` | 646 | 레슨 구조 규칙. 기여할 때 함께 읽음 | 완료 (`0e9b432`) |
| 3 | `CODE_OF_CONDUCT.md` | 224 | 행동 강령. 해석이 갈리면 원문이 우선한다는 안내를 맨 위에 넣음 | 완료 (`69c217b`) |
| 4 | `SECURITY.md` | 337 | 보안 정책. 원문 우선 안내를 맨 위에 넣음 | 다음 차례 |
| 5 | `LEARNING-ROADMAP.md` | 768 | 학습 순서 안내. `ko/README.md`에서 4번 링크 | 대기 |
| 6 | `QUICK_REFERENCE.md` | 556 | 학습 중 자주 보는 요약표 | 대기 |
| 7 | `CATALOG.md` | 637 | 기능·명령 카탈로그. `ko/README.md`에서 3번 링크 | 대기 |
| 8 | `claude_concepts_guide.md` | 1471 | 개념 설명 가이드 | 대기 |
| 9 | `INDEX.md` | 930 | 전체 예제 색인. 다른 번역이 끝난 뒤 링크를 맞춰 번역 | 대기 |
| 10 | `resources.md` | 271 | 외부 학습 자료 목록 | 대기 |
| 11 | `clean-code-rules.md` | 115 | 04장 clean-code-reviewer 실습에 쓰는 규칙 | 대기 |
| 12 | `CHANGELOG.md` | 1227 | 변경 기록. 원문이 계속 늘어나므로 번역 시점의 버전까지만 옮기고, `i18n-source-sha`로 이후 갱신 범위를 확인 | 대기 |

**번역하지 않음** (원문 링크 유지):

| 파일 | 이유 |
|------|------|
| `CLAUDE.md` | 이 저장소 관리용 Claude 지침. 학습 대상이 아님 |
| `README.md` | 이미 `ko/README.md`로 번역 완료 |

번역한 루트 문서를 마칠 때마다 `ko/` 안의 해당 링크(`../LEARNING-ROADMAP.md` 등)를 번역본으로 바꿉니다.

## 남은 일과 메모

- 번역이 끝나면 `.pre-commit-config.yaml`에 `ko/` 전용 검사를 추가해야 합니다 (`vi/`, `ja/` 항목 참고).
- 06 챕터의 예제 훅 스크립트(`.sh`, `.py`)와 07 챕터 플러그인의 `plugin.json`, 훅(`.js`), 스크립트, MCP 설정은 `ko/`에 없습니다. 플러그인을 설치할 때는 원문 폴더를 지정합니다.
- 03 챕터 스킬의 `scripts/`는 `ko/`에 없습니다. 한국어판 스킬을 실습할 때는 원문 폴더의 `scripts/`를 함께 복사해야 합니다.
