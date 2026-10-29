<!-- i18n-source: 07-plugins/devops-automation/README.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../../resources/logos/claude-howto-logo.svg">
</picture>

# DevOps 자동화 플러그인

배포, 모니터링, 장애 대응을 위한 완전한 DevOps 자동화입니다.

## 기능

✅ 자동 배포
✅ 롤백 절차
✅ 시스템 상태 모니터링
✅ 장애 대응 워크플로
✅ Kubernetes 연동

## 설치

```bash
claude --plugin-dir ./07-plugins/devops-automation
```

> **참고:** 한국어판 폴더에는 설명 문서만 있습니다. 플러그인을 설치할 때는 `plugin.json`, 훅, 스크립트가 들어 있는 원문 폴더(`./07-plugins/devops-automation`)를 지정합니다. 한국어로 번역한 명령·에이전트를 쓰려면 이 폴더의 `.md` 파일을 원문 폴더 사본에 덮어씁니다.

## 포함된 것

### 슬래시 명령
- `/deploy` - 운영 또는 스테이징에 배포
- `/rollback` - 이전 버전으로 롤백
- `/status` - 시스템 상태 확인
- `/incident` - 운영 장애 처리

### 서브에이전트
- `deployment-specialist` - 배포 작업
- `incident-commander` - 장애 조율
- `alert-analyzer` - 시스템 상태 분석

### MCP 서버
- Kubernetes 연동

### 스크립트
- `deploy.sh` - 배포 자동화
- `rollback.sh` - 롤백 자동화
- `health-check.sh` - 상태 확인 도구

### 훅
- `pre-deploy.js` - 배포 전 검증
- `post-deploy.js` - 배포 후 작업

## 사용법

### 스테이징에 배포
```text
/deploy staging
```

### 운영에 배포
```text
/deploy production
```

### 롤백
```text
/rollback production
```

### 상태 확인
```text
/status
```

### 장애 처리
```text
/incident
```

## 요구 사항

- Claude Code 2.1+
- Kubernetes CLI (kubectl)
- 클러스터 접근 설정

## 설정

Kubernetes 설정을 지정합니다.
```bash
export KUBECONFIG=~/.kube/config
```

## 워크플로 예시

```text
User: /deploy production

Claude:
1. 배포 전 훅 실행 (kubectl, 클러스터 연결 확인)
2. deployment-specialist 서브에이전트에 위임
3. deploy.sh 스크립트 실행
4. Kubernetes MCP로 배포 진행 상황 모니터링
5. 배포 후 훅 실행 (파드 대기, 스모크 테스트)
6. 배포 요약 제공

결과:
✅ 배포 완료
📦 버전: v2.1.0
🚀 파드: 3/3 준비됨
⏱️  소요 시간: 2m 34s
```

---

**최종 업데이트**: 2026년 10월 18일
**Claude Code 버전**: 2.1.302
**출처**:
- https://code.claude.com/docs/en/plugins
- https://github.com/anthropics/claude-code/releases/tag/v2.1.131
- https://github.com/anthropics/claude-code/releases/tag/v2.1.138
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/plugins/cli-reference
- https://code.claude.com/docs/en/plugins/install
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
