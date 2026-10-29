---
description: 소스 코드로부터 종합적인 API 문서 만들기
---
<!-- i18n-source: 01-slash-commands/generate-api-docs.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# API 문서 생성기

다음 순서로 API 문서를 생성합니다.

1. `/src/api/`의 모든 파일을 살펴봅니다
2. 함수 시그니처와 JSDoc 주석을 추출합니다
3. 엔드포인트/모듈별로 정리합니다
4. 예시가 담긴 markdown을 만듭니다
5. 요청/응답 스키마를 포함합니다
6. 오류 관련 문서를 추가합니다

출력 형식:
- `/docs/api.md`에 Markdown 파일로 저장
- 모든 엔드포인트에 curl 예시 포함
- TypeScript 타입 추가

---
**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/commands
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
