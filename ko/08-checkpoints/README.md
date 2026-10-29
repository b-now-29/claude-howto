<!-- i18n-source: 08-checkpoints/README.md -->
<!-- i18n-source-sha: ba721a3 -->
<!-- i18n-date: 2026-10-29 -->

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 체크포인트와 되감기

체크포인트를 쓰면 대화 상태를 저장하고 Claude Code 세션의 이전 시점으로 되감을 수 있습니다. 여러 접근 방식을 시험해 보거나, 실수를 되돌리거나, 대안을 비교할 때 아주 유용합니다.

## 개요

체크포인트는 대화 상태를 저장하고 이전 시점으로 되감게 해 주어, 안전하게 실험하고 여러 접근 방식을 탐색할 수 있게 합니다. 체크포인트는 다음을 담은 대화 상태의 스냅샷입니다.
- 주고받은 모든 메시지
- 파일 수정 내역
- 도구 사용 기록
- 세션 컨텍스트

여러 접근 방식을 탐색하거나, 실수를 되돌리거나, 대안을 비교할 때 체크포인트가 큰 도움이 됩니다.

## 핵심 개념

| 개념 | 설명 |
|------|------|
| **체크포인트** | 메시지, 파일, 컨텍스트를 포함한 대화 상태의 스냅샷 |
| **되감기(Rewind)** | 이전 체크포인트로 돌아가고 그 뒤의 변경은 버림 |
| **분기점(Branch Point)** | 여러 접근 방식을 탐색하기 시작하는 체크포인트 |

## 체크포인트 열기

체크포인트는 크게 두 가지 방법으로 열고 관리합니다.

### 단축키 사용
`Esc`를 두 번(`Esc` + `Esc`) 누르면 체크포인트 화면이 열리고 저장된 체크포인트를 둘러볼 수 있습니다.

### 슬래시 명령 사용
`/rewind` 명령(별칭: `/checkpoint`, `/undo`)으로 빠르게 엽니다.

```bash
# 되감기 화면 열기
/rewind

# 또는 별칭 사용
/checkpoint
```

## 되감기 옵션

되감기를 하면 여섯 가지 옵션이 있는 메뉴가 나타납니다.

1. **Restore code and conversation** -- 파일과 메시지를 모두 그 체크포인트로 되돌림
2. **Restore conversation** -- 메시지만 되감고 현재 코드는 그대로 둠
3. **Restore code** -- 파일 변경만 되돌리고 대화 기록은 모두 유지
4. **Summarize from here** -- 이 시점부터 뒤의 대화를 AI가 만든 요약으로 압축해 컨텍스트 윈도 공간을 확보. 선택한 시점 이전의 메시지는 그대로 남음. 디스크의 파일은 바뀌지 않음. 원래 메시지는 세션 기록에 보존됨. 요약이 특정 주제에 집중하도록 지시를 줄 수도 있음
5. **Summarize up to here** -- 반대 방향: 선택한 시점 *이전*의 모든 것을 AI가 만든 요약으로 압축하고, 이 시점부터의 메시지는 그대로 둠. "Summarize from here"와 함께 쓰면 컨텍스트 윈도를 양방향으로, 원하는 부분만 압축할 수 있음. 디스크의 파일은 바뀌지 않고, 원래 메시지는 세션 기록에 보존됨
6. **Never mind** -- 취소하고 현재 상태로 돌아감

> **참고**: 대화를 복원하거나 **Summarize from here**를 고르면, 선택한 메시지의 원래 프롬프트가 입력란에 복원되어 다시 보내거나 고칠 수 있습니다. **Summarize up to here**를 고르면 입력란이 빈 채로 대화 끝에 머뭅니다.

> **팁**: 되감은 뒤 종료하기 전에 Claude에게 프롬프트를 하나 보내세요. Claude Code는 되감은 대화를 그 프롬프트와 함께 저장하므로, 먼저 종료하면 되감은 메시지가 다시 돌아올 수 있습니다. 다시 지우려면 `/rewind`를 실행하고 같은 프롬프트를 고른 뒤 **Restore conversation**을 선택합니다. 복원한 파일은 되감기가 남긴 상태 그대로 유지됩니다.

> **`/clear`는 더 이상 넘을 수 없는 경계가 아닙니다(v2.1.191+)**: `/rewind`는 `/clear`를 실행하기 *전에* 만든 체크포인트에서도 재개할 수 있습니다. 대화를 지워도 그 전 상태가 영구히 사라지지 않으므로, 이전 코드나 컨텍스트가 다시 필요하면 clear를 건너 되감을 수 있습니다.

## 자동 체크포인트

Claude Code는 체크포인트를 자동으로 만듭니다.

- **사용자 프롬프트마다** - 사용자가 입력할 때마다 새 체크포인트가 생김
- **유지됨** - 체크포인트는 대화와 함께 저장되므로 세션을 재개한 뒤에도 `/rewind`가 동작함
- **자동 정리** - 체크포인트는 30일 뒤 자동으로 정리됨

그래서 몇 분 전부터 며칠 전까지, 대화의 어느 이전 시점으로든 언제나 되감을 수 있습니다.

## 사용 사례

| 상황 | 워크플로 |
|------|----------|
| **접근 방식 탐색** | 저장 → A 시도 → 저장 → 되감기 → B 시도 → 비교 |
| **안전한 리팩터링** | 저장 → 리팩터링 → 테스트 → 실패하면 되감기 |
| **A/B 테스트** | 저장 → 설계 A → 저장 → 되감기 → 설계 B → 비교 |
| **실수 복구** | 문제 발견 → 마지막 정상 상태로 되감기 |

## 체크포인트 사용하기

### 보기와 되감기

`Esc`를 두 번 누르거나 `/rewind`를 쓰면 체크포인트 브라우저가 열립니다. 쓸 수 있는 모든 체크포인트가 시각과 함께 나열됩니다. 체크포인트를 고르면 그 상태로 되감습니다.

### 체크포인트 세부 정보

각 체크포인트에는 다음이 표시됩니다.
- 만든 시각
- 수정된 파일
- 대화의 메시지 수
- 사용한 도구

## 실전 예제

### 예제 1: 여러 접근 방식 탐색

```text
User: Let's add a caching layer to the API

Claude: I'll add Redis caching to your API endpoints...
[체크포인트 A에서 변경]

User: Actually, let's try in-memory caching instead

Claude: I'll rewind to explore a different approach...
[사용자가 Esc+Esc를 눌러 체크포인트 A로 되감음]
[체크포인트 B에서 인메모리 캐싱 구현]

User: Now I can compare both approaches
```

### 예제 2: 실수 복구

```text
User: Refactor the authentication module to use JWT

Claude: I'll refactor the authentication module...
[대규모 변경]

User: Wait, that broke the OAuth integration. Let's go back.

Claude: I'll help you rewind to before the refactoring...
[사용자가 Esc+Esc를 눌러 리팩터링 전 체크포인트 선택]

User: Let's try a more conservative approach this time
```

### 예제 3: 안전한 실험

```text
User: Let's try rewriting this in a functional style
[실험 전 체크포인트 생성]

Claude: [실험적 변경]

User: The tests are failing. Let's rewind.
[사용자가 Esc+Esc를 눌러 체크포인트로 되감음]

Claude: I've rewound the changes. Let's try a different approach.
```

### 예제 4: 접근 방식 분기

```text
User: I want to compare two database designs
[체크포인트를 기억해 둠 - "Start"라고 부름]

Claude: I'll create the first design...
[Schema A 구현]

User: Now let me go back and try the second approach
[사용자가 Esc+Esc를 눌러 "Start"로 되감음]

Claude: Now I'll implement Schema B...
[Schema B 구현]

User: Great! Now I have both schemas to choose from
```

## 체크포인트 보존

Claude Code는 체크포인트를 자동으로 관리합니다.

- 사용자 프롬프트마다 체크포인트가 자동으로 만들어짐
- 오래된 체크포인트는 최대 30일 동안 보존
- 저장 공간이 끝없이 늘지 않도록 자동 정리

## 워크플로 패턴

### 탐색을 위한 분기 전략

여러 접근 방식을 탐색할 때:

```text
1. 처음 구현으로 시작 → 체크포인트 A
2. 접근 방식 1 시도 → 체크포인트 B
3. 체크포인트 A로 되감기
4. 접근 방식 2 시도 → 체크포인트 C
5. B와 C의 결과 비교
6. 가장 좋은 방식을 골라 계속 진행
```

### 안전한 리팩터링 패턴

큰 변경을 할 때:

```text
1. 현재 상태 → 체크포인트 (자동)
2. 리팩터링 시작
3. 테스트 실행
4. 테스트 통과 → 계속 작업
5. 테스트 실패 → 되감고 다른 방식 시도
```

## 모범 사례

체크포인트는 자동으로 만들어지므로 상태를 직접 저장할 걱정 없이 작업에 집중할 수 있습니다. 그래도 다음을 기억하세요.

### 체크포인트를 효과적으로 쓰기

✅ **해야 할 일:**
- 되감기 전에 쓸 수 있는 체크포인트 살펴보기
- 다른 방향을 탐색하고 싶을 때 되감기 사용
- 여러 접근 방식을 비교하도록 체크포인트 유지
- 각 되감기 옵션이 하는 일(코드와 대화 복원, 대화 복원, 코드 복원, 요약) 이해하기

❌ **하지 말아야 할 일:**
- 코드 보존을 체크포인트에만 맡기기
- 외부 파일 시스템 변경까지 체크포인트가 추적하리라 기대하기
- 체크포인트를 git 커밋 대신 쓰기

## 설정

체크포인트는 Claude Code에 내장된 기본 동작이라 켜기 위한 설정이 필요 없습니다. 사용자 프롬프트마다 체크포인트가 자동으로 만들어집니다.

체크포인트 동작은 두 가지 설정으로 정합니다. 스냅샷을 만들지 여부와 보존 기간입니다.

```json
{
  "fileCheckpointingEnabled": true,
  "cleanupPeriodDays": 30
}
```

| 설정 | 기본값 | 효과 |
|------|--------|------|
| `fileCheckpointingEnabled` | `true` | 편집할 때마다 그 전에 파일 스냅샷을 만들어 `/rewind`가 복원할 수 있게 함. v2.1.119+ 필요. `/config`에서 **Rewind code (checkpoints)**로 표시. 같은 역할의 환경 변수: `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING` |
| `cleanupPeriodDays` | `30` | 세션 기록과 체크포인트를 보존할 일수 |

Claude Code는 **최근 체크포인트 100개**의 스냅샷만 보관합니다. 그보다 오래된 것은 보존 기간 안이라도 버려집니다.

> **v2.1.117 변경 사항**: 이제 `cleanupPeriodDays`는 체크포인트뿐 아니라 디스크의 여러 캐시 보존 기간도 정합니다(아래는 일부이며, 세션 기록, `file-history/`, `plans/`, `debug/`, `paste-cache/` 등도 포함).
>
> - 세션 체크포인트
> - `~/.claude/tasks/` — 영구 태스크 목록
> - `~/.claude/shell-snapshots/` — 저장된 셸 환경 스냅샷
> - `~/.claude/backups/` — `~/.claude.json`의 이전 버전(최신 5개 유지)
>
> 이제 설정 하나로 이 디렉터리들을 같은 일수가 지나면 한꺼번에 정리합니다.

## 한계

체크포인트에는 다음 한계가 있습니다.

- **Bash로만 바꾼 파일은 추적하지 않음** - Claude가 파일 편집 도구로 한 번 편집한 파일은, 그 뒤 Bash 명령이 그 파일에 한 변경도 `/rewind`가 되돌립니다. Bash 명령으로만 바꾼 파일은 추적하지 않습니다. v2.1.309부터 `sed -i`는 다른 Bash 편집과 똑같이 취급됩니다
- **외부 변경은 추적하지 않음** - Claude Code 밖(에디터, 터미널 등)에서 한 변경은 담기지 않습니다
- **버전 관리를 대신하지 않음** - 코드베이스의 영구적이고 감사 가능한 변경에는 git을 씁니다
- **서브에이전트 편집은 복원하지 않음** - 대화에서 Claude가 아직 편집하지 않은 파일을 서브에이전트가 편집하면 추적되지 않으므로 `/rewind`가 그대로 둡니다(git으로 되돌리세요). 예외는 포그라운드 `context: fork` 스킬로, 그 편집은 메인 세션의 편집처럼 복원됩니다

> **v2.1.216 변경 사항**: `/rewind`는 더 이상 추적 경로에 있는 심볼릭 링크나 하드 링크를 통해 파일을 복원하거나 삭제하지 않습니다. 추적 경로가 심볼릭 링크나 하드 링크로 연결되면 링크를 따라가지 않고 건너뛰며, 이 이유로 건너뛴 경로 수를 알려 줍니다.

## 문제 해결

### 체크포인트가 보이지 않을 때

**문제**: 있어야 할 체크포인트가 없음

**해결**:
- 체크포인트가 정리되었는지 확인
- 디스크 공간 확인
- `cleanupPeriodDays`가 충분히 큰지 확인(기본값: 30일)

### 되감기 실패

**문제**: 체크포인트로 되감을 수 없음

**해결**:
- 커밋하지 않은 변경과 충돌하지 않는지 확인
- 체크포인트가 손상되었는지 확인
- 다른 체크포인트로 되감아 보기

## Git과 함께 쓰기

체크포인트는 git을 보완하지만 대신하지는 않습니다.

| 기능 | Git | 체크포인트 |
|------|-----|------------|
| 범위 | 파일 시스템 | 대화 + 파일 |
| 지속성 | 영구 | 세션 기반 |
| 단위 | 커밋 | 어느 시점이든 |
| 속도 | 느림 | 즉시 |
| 공유 | 가능 | 제한적 |

둘을 함께 씁니다.
1. 빠른 실험에는 체크포인트
2. 확정한 변경에는 git 커밋
3. git 작업 전에 체크포인트 만들기
4. 성공한 체크포인트 상태를 git에 커밋

## 빠른 시작 가이드

### 기본 워크플로

1. **평소처럼 작업** - Claude Code가 체크포인트를 자동으로 만듦
2. **되돌아가고 싶다면?** - `Esc`를 두 번 누르거나 `/rewind` 사용
3. **체크포인트 고르기** - 목록에서 골라 되감기
4. **복원할 대상 고르기** - 코드와 대화 복원, 대화 복원, 코드 복원, 여기부터 요약, 여기까지 요약, 취소 중에서 선택
5. **계속 작업** - 그 시점으로 돌아와 있음

### 단축키

- **`Esc` + `Esc`** - 체크포인트 브라우저 열기
- **`/rewind`** - 체크포인트를 여는 다른 방법
- **`/checkpoint`**, **`/undo`** - `/rewind`의 별칭

## 되감을 때를 아는 법: 컨텍스트 모니터링

체크포인트로 되돌아갈 수는 있지만, *언제* 되돌아가야 할까요? 대화가 길어지면 Claude의 컨텍스트 윈도가 차면서 모델 품질이 소리 없이 떨어집니다. 모르는 사이에 반쯤 눈먼 모델이 만든 코드를 내보내고 있을 수도 있습니다.

**[cc-context-stats](https://github.com/luongnv89/cc-context-stats)**는 Claude Code 상태 표시줄에 실시간 **컨텍스트 구간(zone)**을 추가해 이 문제를 풉니다. 컨텍스트 윈도에서 지금 어디쯤인지 추적하며, **Plan**(초록, 계획하고 코딩해도 안전)에서 **Code**(노랑, 새 계획 시작은 피함)를 거쳐 **Dump**(주황, 마무리하고 되감기)까지 보여 줍니다. 구간이 바뀌는 것이 보이면, 품질이 떨어진 출력으로 밀어붙이지 말고 체크포인트를 활용해 새로 시작할 때입니다.

## 관련 개념

- **[고급 기능](../09-advanced-features/)** - 계획 모드와 그 밖의 고급 기능
- **[메모리 관리](../02-memory/)** - 대화 기록과 컨텍스트 관리
- **[슬래시 명령](../01-slash-commands/)** - 사용자가 호출하는 단축 명령
- **[훅](../06-hooks/)** - 이벤트 기반 자동화
- **[플러그인](../07-plugins/)** - 묶음 확장 패키지

## 추가 자료

- [Official Checkpointing Documentation](https://code.claude.com/docs/en/checkpointing)
- [고급 기능 가이드](../09-advanced-features/) - 확장 사고(extended thinking)와 그 밖의 기능

## 요약

체크포인트는 Claude Code의 자동 기능으로, 작업을 잃을 걱정 없이 여러 접근 방식을 안전하게 탐색하게 해 줍니다. 사용자 프롬프트마다 새 체크포인트가 자동으로 만들어지므로 세션의 어느 이전 시점으로든 되감을 수 있습니다.

주요 이점:
- 여러 접근 방식을 겁 없이 실험
- 실수를 빠르게 복구
- 여러 해법을 나란히 비교
- 버전 관리 시스템과 안전하게 함께 사용

기억하세요. 체크포인트는 git을 대신하지 않습니다. 빠른 실험에는 체크포인트를, 영구적인 코드 변경에는 git을 쓰세요.

---

**최종 업데이트**: 2026년 10월 26일
**Claude Code 버전**: 2.1.310
**출처**:
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/settings
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/claude-directory
- https://code.claude.com/docs/en/changelog#2-1-309
- https://code.claude.com/docs/en/checkpointing#bash-command-changes-not-tracked
- https://code.claude.com/docs/en/checkpointing#subagent-edits-not-restored
- https://code.claude.com/docs/en/checkpointing#rewound-messages-can-return-if-you-exit-before-your-next-prompt
**호환 모델**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
