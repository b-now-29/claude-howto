---
name: unit-test-expand
description: 테스트되지 않은 분기와 경계 사례를 겨냥해 테스트 커버리지 높이기
---
<!-- i18n-source: 01-slash-commands/unit-test-expand.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# 단위 테스트 확장

프로젝트의 테스트 프레임워크에 맞춰 기존 단위 테스트를 확장합니다.

1. **커버리지 분석**: 커버리지 보고서를 실행해 테스트되지 않은 분기, 경계 사례, 커버리지가 낮은 영역을 찾습니다
2. **빈틈 찾기**: 코드의 논리 분기, 오류 경로, 경계 조건, null/빈 입력을 검토합니다
3. **테스트 작성**: 프로젝트의 프레임워크를 사용합니다
   - Jest/Vitest/Mocha (JavaScript/TypeScript)
   - pytest/unittest (Python)
   - Go testing/testify (Go)
   - Rust 테스트 프레임워크 (Rust)
4. **특정 시나리오 겨냥**:
   - 오류 처리와 예외
   - 경곗값 (최솟값/최댓값, 빈 값, null)
   - 경계 사례와 극단적인 사례
   - 상태 전이와 부수 효과
5. **개선 확인**: 커버리지를 다시 실행해 수치가 올랐는지 확인합니다

새 테스트 코드 블록만 보여 줍니다. 기존 테스트 패턴과 이름 규칙을 따릅니다.

---
**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/commands
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
