---
name: doc-generator
description: 소스 코드로부터 정확하고 종합적인 API 문서 만들기. API 문서를 만들거나 업데이트할 때, OpenAPI 명세를 생성할 때, 또는 사용자가 API 문서, 엔드포인트, 문서화를 언급할 때 사용합니다.
---
<!-- i18n-source: 03-skills/doc-generator/SKILL.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# API 문서 생성 스킬

## 만드는 것

- OpenAPI/Swagger 명세
- API 엔드포인트 문서
- SDK 사용 예시
- 연동 가이드
- 오류 코드 레퍼런스
- 인증 가이드

## 문서 구조

### 엔드포인트마다

````markdown
## GET /api/v1/users/:id

### 설명
이 엔드포인트가 하는 일에 대한 짧은 설명

### 매개변수

| 이름 | 타입 | 필수 | 설명 |
|------|------|------|------|
| id | string | 예 | 사용자 ID |

### 응답

**200 Success**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 Not Found**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "User does not exist"
}
```

### 예시

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```
````

---

**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/skills
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
