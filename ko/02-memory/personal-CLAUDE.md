<!-- i18n-source: 02-memory/personal-CLAUDE.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

# 나의 개발 선호 설정

## 나에 대해
- **경력**: 풀스택 개발 8년
- **선호 언어**: TypeScript, Python
- **소통 방식**: 직설적으로, 예시와 함께
- **학습 방식**: 코드가 곁들여진 시각적 다이어그램

## 코드 선호

### 오류 처리
try-catch 블록과 의미 있는 오류 메시지로 명시적으로 오류를 처리하는 것을 선호합니다.
막연한 오류는 피합니다. 디버깅을 위해 오류는 항상 로그로 남깁니다.

### 주석
주석에는 '무엇'이 아니라 '왜'를 씁니다. 코드는 그 자체로 설명이 되어야 합니다.
주석은 비즈니스 로직이나 한눈에 알기 어려운 결정을 설명해야 합니다.

### 테스트
TDD(테스트 주도 개발)를 선호합니다.
테스트를 먼저 쓰고, 그다음 구현합니다.
구현 세부 사항이 아니라 동작에 집중합니다.

### 아키텍처
모듈화되고 결합도가 낮은 설계를 선호합니다.
테스트하기 쉽도록 의존성 주입을 사용합니다.
관심사를 분리합니다 (Controllers, Services, Repositories).

## 디버깅 선호
- 접두사를 붙여 console.log를 사용합니다: `[DEBUG]`
- 맥락을 포함합니다: 함수 이름, 관련 변수
- 가능하면 스택 트레이스를 사용합니다
- 로그에는 항상 타임스탬프를 넣습니다

## 소통
- 복잡한 개념은 다이어그램으로 설명합니다
- 이론보다 구체적인 예시를 먼저 보여 줍니다
- 변경 전/후 코드 조각을 포함합니다
- 마지막에 핵심 내용을 요약합니다

## 프로젝트 구성
프로젝트는 다음과 같이 구성합니다.
```text
project/
  ├── src/
  │   ├── api/
  │   ├── services/
  │   ├── models/
  │   └── utils/
  ├── tests/
  ├── docs/
  └── docker/
```

## 도구
- **IDE**: vim 키 바인딩을 쓰는 VS Code
- **터미널**: Oh-My-Zsh를 쓰는 Zsh
- **포맷**: Prettier (한 줄 100자)
- **린터**: airbnb 설정을 쓰는 ESLint
- **테스트 프레임워크**: Jest와 React Testing Library

---
**마지막 업데이트**: 2026년 8월 4일
**Claude Code 버전**: 2.1.220
**출처**:
- https://code.claude.com/docs/en/memory
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
