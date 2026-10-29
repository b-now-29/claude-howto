---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*)
argument-hint: [message]
description: 맥락을 반영해 git 커밋 만들기
---
<!-- i18n-source: 01-slash-commands/commit.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

## 맥락

- 현재 git 상태: !`git status`
- 현재 git diff: !`git diff HEAD`
- 현재 브랜치: !`git branch --show-current`
- 최근 커밋: !`git log --oneline -10`

## 할 일

위 변경 사항을 바탕으로 git 커밋 하나를 만듭니다.

인수로 메시지가 주어졌다면 그 메시지를 사용합니다: $ARGUMENTS

주어지지 않았다면 변경 사항을 분석해 conventional commits 형식에 맞는 커밋 메시지를 만듭니다.
- `feat:` 새 기능
- `fix:` 버그 수정
- `docs:` 문서 변경
- `refactor:` 코드 리팩터링
- `test:` 테스트 추가
- `chore:` 유지 보수 작업

---
**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/commands
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
