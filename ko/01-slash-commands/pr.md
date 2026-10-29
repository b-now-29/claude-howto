---
description: 코드를 정리하고 변경 사항을 스테이징해 풀 리퀘스트 준비하기
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---
<!-- i18n-source: 01-slash-commands/pr.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# 풀 리퀘스트 준비 체크리스트

PR을 만들기 전에 다음 단계를 실행합니다.

1. 린트 실행: `prettier --write .`
2. 테스트 실행: `npm test`
3. git diff 검토: `git diff HEAD`
4. 변경 사항 스테이징: `git add .`
5. conventional commits 형식에 맞춰 커밋 메시지 작성:
   - `fix:` 버그 수정
   - `feat:` 새 기능
   - `docs:` 문서
   - `refactor:` 코드 구조 변경
   - `test:` 테스트 추가
   - `chore:` 유지 보수

6. 다음 내용을 담은 PR 요약 작성:
   - 무엇이 바뀌었는지
   - 왜 바뀌었는지
   - 어떤 테스트를 했는지
   - 예상되는 영향

---
**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/commands
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
