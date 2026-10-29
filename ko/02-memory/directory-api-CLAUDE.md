<!-- i18n-source: 02-memory/directory-api-CLAUDE.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# API 모듈 기준

이 파일은 /src/api/ 아래 모든 것에 대해 루트 CLAUDE.md를 보완합니다. 메모리 파일은
덮어쓰지 않고 이어 붙입니다. 루트 CLAUDE.md는 계속 적용되며, Claude Code는
이 하위 트리의 파일을 읽거나, 쓰거나, 편집할 때 필요에 따라 이 파일을 불러옵니다.

## API 전용 기준

### 요청 검증
- 스키마 검증에는 Zod를 사용합니다
- 입력은 항상 검증합니다
- 검증 오류는 400으로 반환합니다
- 필드 단위 오류 정보를 포함합니다

### 인증
- 모든 엔드포인트는 JWT 토큰이 필요합니다
- 토큰은 Authorization 헤더에 넣습니다
- 토큰은 24시간 뒤 만료됩니다
- 리프레시 토큰 방식을 구현합니다

### 응답 형식

모든 응답은 다음 구조를 따라야 합니다.

```json
{
  "success": true,
  "data": { /* 실제 데이터 */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

오류 응답:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User message",
    "details": { /* 필드 오류 */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 페이지네이션
- 커서 기반 페이지네이션을 사용합니다 (오프셋 방식 아님)
- `hasMore` 불리언을 포함합니다
- 최대 페이지 크기는 100으로 제한합니다
- 기본 페이지 크기: 20

### 속도 제한
- 인증된 사용자는 시간당 1000회 요청
- 공개 엔드포인트는 시간당 100회 요청
- 한도를 넘으면 429를 반환합니다
- retry-after 헤더를 포함합니다

### 캐싱
- 세션 캐싱에는 Redis를 사용합니다
- 캐시 유지 시간: 기본 5분
- 쓰기 작업이 일어나면 캐시를 무효화합니다
- 캐시 키에 리소스 유형 태그를 붙입니다

---
**마지막 업데이트**: 2026년 10월 10일
**Claude Code 버전**: 2.1.296
**출처**:
- https://code.claude.com/docs/en/memory
- https://github.com/anthropics/claude-code/releases/tag/v2.1.288
- https://github.com/anthropics/claude-code/releases/tag/v2.1.293
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
