<!-- i18n-source: 08-checkpoints/checkpoint-examples.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# 체크포인트 예제

Claude Code에서 체크포인트를 효과적으로 쓰는 실제 예제입니다.

참고: 체크포인트는 사용자 프롬프트마다 자동으로 만들어지므로 직접 저장할 필요가 없습니다. 되감으려면 `Esc`를 두 번(`Esc+Esc`) 누르거나 `/rewind`로 체크포인트 브라우저를 엽니다.

## 예제 1: 데이터베이스 마이그레이션

### 시나리오
MySQL에서 PostgreSQL로 옮겨야 하는데, 직접 마이그레이션과 점진적인 이중 쓰기(dual-write) 방식을 모두 시험해 보고 싶습니다.

### 워크플로

```text
User: Let's implement a direct migration from MySQL to PostgreSQL
      [체크포인트 A 자동 생성]

Claude: I'll implement the direct migration...
[데이터베이스 드라이버 변경, 쿼리 수정, 스키마 수정]

User: Run the tests
      [체크포인트 B 자동 생성]

Claude: Running tests...
❌ 15 tests failing - connection pool issues

User: This approach has issues. Let's try the dual-write pattern instead.
[사용자가 Esc+Esc를 누르고 체크포인트 A를 골라 "Restore code and conversation" 선택]

User: Now implement a dual-write pattern where we write to both databases

Claude: I'll implement dual-write migration...
[이중 쓰기 계층과 점진적 마이그레이션 전략 구현]

User: Run the tests

Claude: Running tests...
✅ All tests passing

User: The dual-write approach is clearly safer. Let's commit this.
```

## 예제 2: 성능 최적화

### 시나리오
시험해 볼 성능 최적화 전략이 여러 가지입니다.

### 워크플로

```text
User: Let's optimize the API response time. Current: 450ms
      [체크포인트 "Baseline" 자동 생성]

Claude: I'll add database query caching...
[Redis 캐싱 구현]

User: Run performance tests

Claude: Results: 280ms average response time (38% improvement)

User: Let me try a different approach.
[사용자가 Esc+Esc를 누르고 "Baseline" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Try database query optimization instead

Claude: I'll optimize database queries...
[인덱스 추가, N+1 쿼리 최적화]

User: Run performance tests

Claude: Results: 180ms average response time (60% improvement)

User: Let me also try a third approach.
[사용자가 Esc+Esc를 누르고 "Baseline" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Try response compression and CDN

Claude: I'll add compression and CDN...
[gzip 구현, CDN 설정]

User: Run performance tests

Claude: Results: 320ms average response time (29% improvement)

User: Query optimization was the best. Let me go back and combine it with caching.
[사용자가 Esc+Esc를 누르고 쿼리 최적화 뒤의 체크포인트를 골라 "Restore code and conversation" 선택]

User: Now add Redis caching on top of these optimized queries

Claude: [최적화한 쿼리 위에 Redis 캐싱 추가]

User: Run performance tests

Claude: Results: 95ms average response time (79% improvement) 🎉

User: Perfect! This is the winner. Let's commit this.
```

## 예제 3: UI/UX 반복 개선

### 시나리오
대시보드에 여러 레이아웃을 시험합니다.

### 워크플로

```text
User: Let's redesign the dashboard with a sidebar layout
      [체크포인트 "Start" 자동 생성]

Claude: [사이드바 레이아웃 구현]

User: Show me a screenshot

Claude: [스크린샷 표시]

User: The sidebar feels cramped. Let's try a top navigation instead.
[사용자가 Esc+Esc를 누르고 "Start" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Try a top navigation layout

Claude: [상단 내비게이션 구현]

User: Show me a screenshot

Claude: [스크린샷 표시]

User: Better, but let's try a card-based grid layout
[사용자가 Esc+Esc를 누르고 "Start" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Try a card-based grid layout

Claude: [카드 그리드 구현]

User: Show me a screenshot

Claude: [스크린샷 표시]

User: This is great! But can we combine the top nav with the card grid?
[사용자가 Esc+Esc를 누르고 카드 그리드 뒤의 체크포인트를 골라 "Restore code and conversation" 선택]

User: Add top navigation to the card grid layout

Claude: [카드 그리드에 상단 내비게이션 추가]

User: Perfect! This is the final design.
```

## 예제 4: 디버깅 세션

### 시나리오
여러 가설을 세워 까다로운 버그를 추적합니다.

### 워크플로

```text
User: There's a memory leak in the application
      [체크포인트 "Before debugging" 자동 생성]

Claude: Let me investigate...
[코드 분석]

User: I think it's in the event listeners. Let's fix those.

Claude: [이벤트 리스너 정리 수정]

User: Run memory profiler

Claude: Still leaking memory - event listeners weren't the issue

User: Let me go back and try a different hypothesis.
[사용자가 Esc+Esc를 누르고 "Before debugging" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Maybe it's the database connections?

Claude: [커넥션 풀링 수정 구현]

User: Run memory profiler

Claude: Still leaking - not the connections either

User: Let me go back and try another angle.
[사용자가 Esc+Esc를 누르고 "Before debugging" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Let's check for circular references

Claude: [캐시의 순환 참조를 찾아 수정]

User: Run memory profiler

Claude: ✅ Memory leak fixed! The circular references in the caching layer were the culprit.

User: Excellent! Let's commit this fix.
```

## 예제 5: API 설계 발전시키기

### 시나리오
여러 번 반복하며 REST API를 설계합니다.

### 워크플로

```text
User: Create a REST API for our blog platform
      [체크포인트 "Before API design" 자동 생성]

Claude: [기본 CRUD API 생성]

User: This is too simple. Let's add pagination and filtering

Claude: [페이지네이션과 필터링 추가]

User: We need better resource relationships

Claude: [HATEOAS 링크 구현]

User: Actually, let's try GraphQL instead of REST
[사용자가 Esc+Esc를 누르고 "Before API design" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Implement a GraphQL API for our blog platform instead

Claude: [GraphQL API 구현]

User: I think REST is better for our use case.
[사용자가 Esc+Esc를 누르고 HATEOAS 뒤의 체크포인트를 골라 "Restore code and conversation" 선택]

User: Let's finalize the REST approach.
```

## 예제 6: 설정 관리

### 시나리오
여러 설정 방식을 시험합니다.

### 워크플로

```text
User: Move configuration to environment variables
      [체크포인트 "Original config" 자동 생성]

Claude: [환경 변수 설정 구현]

User: Test in development

Claude: Works locally but deployment issues in production

User: Let me try a different approach.
[사용자가 Esc+Esc를 누르고 "Original config" 체크포인트를 골라 "Restore code and conversation" 선택]

User: Try a config file approach with validation

Claude: [JSON 스키마 검증을 갖춘 YAML 설정 구현]

User: Test in development

Claude: ✅ Works well, good validation errors

User: Can we also support env vars for sensitive values?

Claude: [비밀 값용 환경 변수 재정의 추가]

User: Test deployment

Claude: ✅ All environments working correctly

User: Perfect! This is production-ready.
```

## 예제 7: 테스트 전략

### 시나리오
종합적인 테스트를 구현합니다.

### 워크플로

```text
User: Add unit tests for the auth module
      [체크포인트 자동 생성]

Claude: [Jest 단위 테스트 추가]

User: Run tests

Claude: 45 tests passing, 78% coverage

User: Add integration tests too

Claude: [통합 테스트 추가]

User: Run tests

Claude: 89 tests passing, 92% coverage, but tests are slow (3 minutes)

User: The tests are too slow. Let's optimize.

Claude: [테스트 준비 최적화, 병렬 실행 추가]

User: Run tests

Claude: 89 tests passing, 92% coverage, 35 seconds ✅

User: Great! Now add E2E tests for critical paths

Claude: [Playwright E2E 테스트 추가]

User: Run all tests

Claude: 112 tests passing, 94% coverage, 2 minutes

User: Perfect balance of coverage and speed!
```

## 예제 8: 체크포인트에서 요약하기

### 시나리오
긴 디버깅 세션 뒤, 컨텍스트는 유지하면서 대화를 줄이고 싶습니다.

### 워크플로

```text
User: [20개 넘는 메시지로 디버깅과 탐색을 한 뒤]

[사용자가 Esc+Esc를 누르고 초기 체크포인트를 골라 "Summarize from here" 선택]
[선택적으로 지시 제공: "Focus on what we tried and what worked"]

Claude: [그 시점부터의 대화 요약 생성]
[원래 메시지는 대화 기록에 보존됨]
[요약이 보이는 대화를 대체해 컨텍스트 윈도 사용량을 줄임]

User: Now let's continue with the approach that worked.
```

## 핵심 정리

1. **체크포인트는 자동**: 사용자 프롬프트마다 체크포인트가 만들어지므로 직접 저장할 필요가 없습니다
2. **Esc+Esc 또는 /rewind 사용**: 체크포인트 브라우저를 여는 두 가지 방법입니다
3. **알맞은 복원 옵션 고르기**: 필요에 따라 코드, 대화, 둘 다 복원하거나 요약합니다
4. **실험을 두려워하지 않기**: 체크포인트 덕분에 과감한 변경도 안전하게 시도할 수 있습니다
5. **git과 함께 쓰기**: 탐색에는 체크포인트, 확정한 작업에는 git을 씁니다
6. **긴 세션 요약하기**: "Summarize from here"로 대화를 관리하기 쉬운 크기로 유지합니다

---
**최종 업데이트**: 2026년 7월 29일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/changelog
- https://code.claude.com/docs/en/model-config
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
