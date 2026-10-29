---
description: 모든 변경 사항을 스테이징하고 커밋한 뒤 원격으로 푸시하기 (주의해서 사용)
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git diff:*), Bash(git log:*), Bash(git pull:*)
---
<!-- i18n-source: 01-slash-commands/push-all.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# 전부 커밋하고 푸시하기

⚠️ **주의**: 모든 변경 사항을 스테이징하고, 커밋하고, 원격으로 푸시합니다. 모든 변경 사항이 함께 묶여도 된다고 확신할 때만 사용하세요.

## 워크플로

### 1. 변경 사항 분석
병렬로 실행합니다.
- `git status` - 수정/추가/삭제/추적되지 않은 파일 표시
- `git diff --stat` - 변경 통계 표시
- `git log -1 --oneline` - 메시지 스타일을 참고할 최근 커밋 표시

### 2. 안전 검사

**❌ 다음이 발견되면 멈추고 경고합니다:**
- 비밀 정보: `.env*`, `*.key`, `*.pem`, `credentials.json`, `secrets.yaml`, `id_rsa`, `*.p12`, `*.pfx`, `*.cer`
- API 키: 실제 값이 들어 있는 `*_API_KEY`, `*_SECRET`, `*_TOKEN` 변수 (`your-api-key`, `xxx`, `placeholder` 같은 자리표시자는 제외)
- 큰 파일: Git LFS 없이 `>10MB`
- 빌드 산출물: `node_modules/`, `dist/`, `build/`, `__pycache__/`, `*.pyc`, `.venv/`
- 임시 파일: `.DS_Store`, `thumbs.db`, `*.swp`, `*.tmp`

**API 키 검증:**
수정된 파일에서 다음과 같은 패턴을 확인합니다.
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ 실제 키 발견!
AWS_SECRET_KEY=AKIA...         # ❌ 실제 키 발견!
STRIPE_API_KEY=sk_live_...    # ❌ 실제 키 발견!

# ✅ 허용되는 자리표시자:
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ 확인 사항:**
- `.gitignore`가 제대로 설정되어 있는지
- 병합 충돌이 없는지
- 올바른 브랜치인지 (main/master라면 경고)
- API 키가 자리표시자뿐인지

### 3. 확인 요청

요약을 보여 줍니다.
```text
📊 변경 요약:
- 파일 X개 수정, Y개 추가, Z개 삭제
- 합계: +AAA줄 추가, -BBB줄 삭제

🔒 안전: ✅ 비밀 정보 없음 | ✅ 큰 파일 없음 | ⚠️ [경고]
🌿 브랜치: [name] → origin/[name]

진행할 작업: git add . → commit → push

진행하려면 'yes', 취소하려면 'no'를 입력하세요.
```

**명시적인 "yes"를 받을 때까지 기다립니다.**

### 4. 실행 (확인 후)

순서대로 실행합니다.
```bash
git add .
git status  # 스테이징 확인
```

### 5. 커밋 메시지 생성

변경 사항을 분석해 conventional commit을 만듭니다.

**형식:**
```text
[type]: Brief summary (max 72 characters)

- Key change 1
- Key change 2
- Key change 3
```

**유형:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`

**예시:**
```text
docs: Update concept README files with comprehensive documentation

- Add architecture diagrams and tables
- Include practical examples
- Expand best practices sections
```

### 6. 커밋과 푸시

```bash
git commit -m "$(cat <<'EOF'
[Generated commit message]
EOF
)"
git push  # 실패하면: git pull --rebase && git push
git log -1 --oneline --decorate  # 확인
```

### 7. 성공 확인

```text
✅ 원격에 푸시했습니다!

커밋: [hash] [message]
브랜치: [branch] → origin/[branch]
변경된 파일: X개 (+추가, -삭제)
```

## 오류 처리

- **git add 실패**: 권한과 잠긴 파일을 확인하고, 저장소가 초기화되었는지 확인합니다
- **git commit 실패**: pre-commit 훅 문제를 고치고, git 설정(user.name/email)을 확인합니다
- **git push 실패**:
  - fast-forward가 아닐 때: `git pull --rebase && git push`
  - 원격 브랜치가 없을 때: `git push -u origin [branch]`
  - 보호된 브랜치일 때: 대신 PR 워크플로를 사용합니다

## 언제 사용하나요

✅ **적합한 경우:**
- 여러 파일에 걸친 문서 업데이트
- 테스트와 문서를 포함한 기능
- 여러 파일에 걸친 버그 수정
- 프로젝트 전체 포맷/리팩터링
- 설정 변경

❌ **피해야 할 경우:**
- 무엇이 커밋되는지 확실하지 않을 때
- 비밀 정보/민감한 데이터가 포함되어 있을 때
- 리뷰 없이 보호된 브랜치에 올릴 때
- 병합 충돌이 있을 때
- 커밋 기록을 잘게 나누고 싶을 때
- pre-commit 훅이 실패할 때

## 대안

사용자가 직접 통제하고 싶어 하면 다음을 제안합니다.
1. **선택적 스테이징**: 특정 파일만 검토하고 스테이징
2. **대화형 스테이징**: `git add -p`로 변경 조각 선택
3. **PR 워크플로**: 브랜치 생성 → 푸시 → PR (`/pr` 명령 사용)

**⚠️ 기억하세요**: 푸시하기 전에 항상 변경 사항을 검토하세요. 확신이 없으면 개별 git 명령으로 더 세밀하게 다루세요.

---
**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/commands
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
