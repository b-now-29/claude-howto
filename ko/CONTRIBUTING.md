<!-- i18n-source: CONTRIBUTING.md -->
<!-- i18n-source-sha: 0e01159 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Claude How To에 기여하기

이 프로젝트에 관심을 가져 주셔서 감사합니다! 이 가이드는 효과적으로 기여하는 방법을 안내합니다.

## 이 프로젝트 소개

Claude How To는 그림과 예제로 배우는 Claude Code 가이드입니다. 다음을 제공합니다.
- 기능의 동작 방식을 설명하는 **Mermaid 다이어그램**
- 바로 쓸 수 있는 **실전용 템플릿**
- 맥락과 모범 사례를 담은 **실제 예제**
- 입문부터 고급까지 이어지는 **단계별 학습 경로**

## 기여 유형

### 1. 새 예제나 템플릿
기존 기능(슬래시 명령, 스킬, 훅 등)의 예제를 추가합니다.
- 복사해서 바로 쓸 수 있는 코드
- 동작 방식에 대한 명확한 설명
- 사용 사례와 장점
- 문제 해결 팁

### 2. 문서 개선
- 헷갈리는 단락을 명확하게 고치기
- 오타와 문법 고치기
- 빠진 정보 추가하기
- 코드 예제 개선하기

### 3. 기능 가이드
새 Claude Code 기능의 가이드를 만듭니다.
- 단계별 튜토리얼
- 구조 다이어그램
- 자주 쓰는 패턴과 안티패턴
- 실제 워크플로

### 4. 버그 보고
발견한 문제를 알려 주세요.
- 기대한 동작
- 실제로 일어난 일
- 재현 단계
- 관련 Claude Code 버전과 운영체제

### 5. 의견과 제안
가이드를 함께 개선해 주세요.
- 더 나은 설명 제안
- 다루지 않은 부분 지적
- 새 단락이나 구성 변경 제안

## 시작하기

### 1. 포크와 클론
```bash
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto
```

### 2. 브랜치 만들기
내용을 알 수 있는 브랜치 이름을 씁니다.
```bash
git checkout -b add/feature-name
git checkout -b fix/issue-description
git checkout -b docs/improvement-area
```

### 3. 환경 설정

pre-commit 훅은 커밋할 때마다 CI와 같은 검사를 로컬에서 실행합니다. PR이 받아들여지려면 다섯 가지 검사를 모두 통과해야 합니다.

**필요한 의존성:**

```bash
# Python 도구 (이 프로젝트의 패키지 관리자는 uv)
pip install uv
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# Markdown 린터 (Node.js)
npm install -g markdownlint-cli

# Mermaid 다이어그램은 Pele로 렌더링하며 Node.js >= 20이 필요합니다
# (전역으로 설치할 것은 없음). 버전 확인:
node --version

# pre-commit을 설치하고 훅을 활성화
uv pip install pre-commit
pre-commit install
```

**설정 확인:**

```bash
pre-commit run --all-files
```

첫 실행 때 Mermaid 훅용 Node.js 22(pre-commit이 준비하므로 시스템에 Node.js가 없어도 동작)와 고정 버전의 Pele 번들을 `scripts/.vendor-cache/pele/`에 내려받습니다. 그 뒤로는 오프라인에서도 동작합니다. Mermaid 훅은 strict 모드로 실행되므로 Pele를 실행할 수 없으면 실패합니다. 첫 다운로드 전에 오프라인에서 커밋하려면 `SKIP=mermaid,vietnamese-mermaid,japanese-mermaid`로 이 훅들을 건너뜁니다.

커밋할 때마다 실행되는 훅은 다음과 같습니다.

| 훅 | 검사 내용 |
|------|---------------|
| `markdown-lint` | Markdown 서식과 구조 |
| `cross-references` | 상대 링크, 앵커, 코드 블록 |
| `mermaid-syntax` | 모든 ` ```mermaid ` 블록이 올바르게 파싱되는지 |
| `link-check` | 외부 URL에 접속할 수 있는지 |
| `build-epub` | EPUB가 오류 없이 생성되는지 (`.md` 변경 시) |

## 디렉터리 구조

```text
├── 01-slash-commands/      # 사용자가 호출하는 단축 명령
├── 02-memory/              # 지속되는 컨텍스트 예제
├── 03-skills/              # 재사용할 수 있는 기능
├── 04-subagents/           # 전문 AI 어시스턴트
├── 05-mcp/                 # Model Context Protocol 예제
├── 06-hooks/               # 이벤트 기반 자동화
├── 07-plugins/             # 기능 묶음
├── 08-checkpoints/         # 세션 스냅샷
├── 09-advanced-features/   # 계획, 사고, 백그라운드 작업
├── 10-cli/                 # CLI 레퍼런스
├── scripts/                # 빌드와 유틸리티 스크립트
└── README.md               # 메인 가이드
```

## 예제를 기여하는 방법

### 슬래시 명령 추가
1. `01-slash-commands/`에 `.md` 파일을 만듭니다
2. 다음을 넣습니다.
   - 하는 일에 대한 명확한 설명
   - 사용 사례
   - 설치 방법
   - 사용 예시
   - 사용자 맞춤 팁
3. `01-slash-commands/README.md`를 갱신합니다

### 스킬 추가
1. `03-skills/`에 디렉터리를 만듭니다
2. 다음을 넣습니다.
   - `SKILL.md` - 주 문서
   - `scripts/` - 필요하면 보조 스크립트
   - `templates/` - 프롬프트 템플릿
   - README에 사용 예시
3. `03-skills/README.md`를 갱신합니다

### 서브에이전트 추가
1. `04-subagents/`에 `.md` 파일을 만듭니다
2. 다음을 넣습니다.
   - 에이전트의 목적과 능력
   - 시스템 프롬프트 구조
   - 사용 사례 예시
   - 연동 예시
3. `04-subagents/README.md`를 갱신합니다

### MCP 설정 추가
1. `05-mcp/`에 `.json` 파일을 만듭니다
2. 다음을 넣습니다.
   - 설정 설명
   - 필요한 환경 변수
   - 설정 방법
   - 사용 예시
3. `05-mcp/README.md`를 갱신합니다

### 훅 추가
1. `06-hooks/`에 `.sh` 파일을 만듭니다
2. 다음을 넣습니다.
   - 셔뱅(shebang)과 설명
   - 로직을 설명하는 명확한 주석
   - 오류 처리
   - 보안 고려 사항
3. `06-hooks/README.md`를 갱신합니다

## 작성 지침

### Markdown 스타일
- 명확한 제목을 씁니다 (단락은 H2, 하위 단락은 H3)
- 문단은 짧고 한 가지 주제에 집중합니다
- 목록에는 글머리 기호를 씁니다
- 코드 블록에는 언어를 표시합니다
- 단락 사이에 빈 줄을 넣습니다

### 코드 예제
- 복사해서 바로 쓸 수 있게 만듭니다
- 알기 어려운 로직에는 주석을 답니다
- 간단한 버전과 고급 버전을 모두 넣습니다
- 실제 사용 사례를 보여 줍니다
- 생길 수 있는 문제를 짚어 줍니다

### 문서
- "무엇"뿐 아니라 "왜"를 설명합니다
- 사전 요구 사항을 적습니다
- 문제 해결 단락을 넣습니다
- 관련 주제로 링크합니다
- 입문자도 이해하기 쉽게 씁니다

### JSON/YAML
- 들여쓰기를 일관되게 맞춥니다 (2칸 또는 4칸)
- 설정을 설명하는 주석을 넣습니다
- 검증 예시를 넣습니다

### 다이어그램
- 가능하면 Mermaid를 씁니다
- 다이어그램은 단순하고 읽기 쉽게 만듭니다
- 다이어그램 아래에 설명을 붙입니다
- 관련 단락으로 링크합니다

## 커밋 지침

Conventional Commits 형식을 따릅니다.
```text
type(scope): description

[optional body]
```

유형:
- `feat`: 새 기능이나 예제
- `fix`: 버그 수정이나 정정
- `docs`: 문서 변경
- `refactor`: 코드 구조 변경
- `style`: 서식 변경
- `test`: 테스트 추가나 변경
- `chore`: 빌드, 의존성 등

예시:
```text
feat(slash-commands): Add API documentation generator
docs(memory): Improve personal preferences example
fix(README): Correct table of contents link
docs(skills): Add comprehensive code review skill
```

## 제출하기 전에

### 체크리스트
- [ ] 코드가 프로젝트 스타일과 관례를 따른다
- [ ] 새 예제에 명확한 문서가 있다
- [ ] README 파일을 갱신했다 (해당 폴더와 루트 모두)
- [ ] 민감한 정보(API 키, 자격 증명)가 없다
- [ ] 예제를 테스트했고 동작한다
- [ ] 링크를 확인했고 올바르다
- [ ] 파일 권한이 알맞다 (스크립트는 실행 가능)
- [ ] 커밋 메시지가 명확하고 내용을 잘 설명한다

### 로컬 테스트
```bash
# 모든 pre-commit 검사 실행 (CI와 같은 검사)
pre-commit run --all-files

# 변경 사항 검토
git diff
```

## 풀 리퀘스트 절차

1. **명확한 설명과 함께 PR을 만듭니다**:
   - 무엇을 추가하거나 고치나요?
   - 왜 필요한가요?
   - 관련 이슈 (있다면)

2. **관련 정보를 넣습니다**:
   - 새 기능이면 사용 사례
   - 문서면 개선한 점
   - 예제면 변경 전후 비교

3. **이슈를 연결합니다**:
   - 관련 이슈를 자동으로 닫으려면 `Closes #123`을 씁니다

4. **리뷰를 기다려 주세요**:
   - 관리자가 개선점을 제안할 수 있습니다
   - 피드백에 따라 다듬습니다
   - 최종 결정은 관리자가 합니다

## 코드 리뷰 절차

리뷰어는 다음을 확인합니다.
- **정확성**: 설명대로 동작하나요?
- **품질**: 실전에 쓸 수 있는 수준인가요?
- **일관성**: 프로젝트의 패턴을 따르나요?
- **문서**: 명확하고 빠짐없나요?
- **보안**: 취약점은 없나요?

## 이슈 보고

### 버그 보고
다음을 넣습니다.
- Claude Code 버전
- 운영체제
- 재현 단계
- 기대한 동작
- 실제 동작
- 해당되면 스크린샷

### 기능 요청
다음을 넣습니다.
- 사용 사례나 해결하려는 문제
- 제안하는 해결 방법
- 검토한 대안
- 추가 맥락

### 문서 이슈
다음을 넣습니다.
- 헷갈리거나 빠진 내용
- 개선 제안
- 예시나 참고 자료

## 프로젝트 정책

### 민감한 정보
- API 키, 토큰, 자격 증명은 절대 커밋하지 않습니다
- 예제에는 자리표시자 값을 씁니다
- 설정 파일에는 `.env.example`을 함께 둡니다
- 필요한 환경 변수를 문서에 적습니다

### 코드 품질
- 예제는 핵심에 집중하고 읽기 쉽게 만듭니다
- 해결책을 과하게 설계하지 않습니다
- 알기 어려운 로직에는 주석을 답니다
- 제출하기 전에 충분히 테스트합니다

### 지식 재산
- 원작 콘텐츠의 소유권은 작성자에게 있습니다
- 프로젝트는 교육용 라이선스를 씁니다
- 기존 저작권을 존중합니다
- 필요한 곳에는 출처를 밝힙니다

## 도움 받기

- **질문**: GitHub Issues에서 논의를 엽니다
- **일반적인 도움**: 기존 문서를 확인합니다
- **개발 도움**: 비슷한 예제를 살펴봅니다
- **코드 리뷰**: PR에서 관리자를 태그합니다

## 기여자 소개

기여자는 다음 곳에 소개됩니다.
- README.md의 Contributors 단락
- GitHub 기여자 페이지
- 커밋 기록

## 보안

예제와 문서를 기여할 때는 안전한 코딩 관행을 따라 주세요.

- **비밀 값이나 API 키를 코드에 직접 넣지 않습니다** - 환경 변수를 씁니다
- **보안상 영향을 알립니다** - 생길 수 있는 위험을 짚어 줍니다
- **안전한 기본값을 씁니다** - 보안 기능을 기본으로 켭니다
- **입력을 검증합니다** - 올바른 입력 검증과 정제 방법을 보여 줍니다
- **보안 참고 사항을 넣습니다** - 보안 고려 사항을 문서에 적습니다

보안 문제는 [SECURITY.md](../SECURITY.md)의 취약점 보고 절차를 참고하세요.

## 행동 강령

모두를 환영하고 포용하는 커뮤니티를 만들고자 합니다. 커뮤니티 기준 전체는 [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md)를 읽어 주세요.

요약하면 다음과 같습니다.
- 존중하고 포용합니다
- 피드백을 너그럽게 받아들입니다
- 다른 사람이 배우고 성장하도록 돕습니다
- 괴롭힘이나 차별을 하지 않습니다
- 문제는 관리자에게 알립니다

모든 기여자는 이 강령을 지키고 서로를 친절과 존중으로 대해야 합니다.

## 라이선스

이 프로젝트에 기여하면, 기여한 내용이 MIT 라이선스로 배포되는 데 동의하는 것입니다. 자세한 내용은 [LICENSE](../LICENSE) 파일을 참고하세요.

## 질문이 있나요?

- [README](README.md)를 확인합니다
- [LEARNING-ROADMAP.md](../LEARNING-ROADMAP.md)를 살펴봅니다
- 기존 예제를 참고합니다
- 논의할 이슈를 엽니다

기여해 주셔서 감사합니다! 🙏

---
**최종 수정일**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/overview
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
