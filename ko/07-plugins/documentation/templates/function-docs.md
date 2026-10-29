<!-- i18n-source: 07-plugins/documentation/templates/function-docs.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# 함수: `functionName`

## 설명
함수가 하는 일을 짧게 설명합니다.

## 시그니처
```typescript
function functionName(param1: Type1, param2: Type2): ReturnType
```

## 매개변수

| 매개변수 | 타입 | 필수 | 설명 |
|----------|------|------|------|
| param1 | Type1 | 예 | param1 설명 |
| param2 | Type2 | 아니요 | param2 설명 |

## 반환값
**타입**: `ReturnType`

반환하는 값의 설명.

## 예외
- `Error`: 잘못된 입력이 주어졌을 때
- `TypeError`: 잘못된 타입이 전달되었을 때

## 예제

### 기본 사용법
```typescript
const result = functionName('value1', 'value2');
console.log(result);
```

### 고급 사용법
```typescript
const result = functionName(
  complexParam1,
  { option: true }
);
```

## 참고
- 추가 참고 사항이나 주의 사항
- 성능 고려 사항
- 모범 사례

## 함께 보기
- [관련 함수](#)
- [API 문서](#)

---

**최종 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/plugins
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
