# Codex 역할·Skill 경계 정리 분석

> 이 문서는 당시 승인된 분석의 기록이다. 구현 위임의 현재 기준은 루트 `AGENTS.md`와 `skills/implement/SKILL.md`를 따른다.

## 근거

확인 사실:

- 승인된 `spec.md`는 built-in `worker`의 구현 역할을 유지하면서 Task 구현 모델만 명시적으로 선택하도록 요구한다.
- 현재 설치본은 `codex-cli 0.146.0`이고 `multi_agent`가 활성화돼 있다.
- 현재 `spawn_agent`는 `agent_type`, `model`, `reasoning_effort`, `fork_turns`, `task_name`, `message`를 입력받는다.
- 현재 호출 기능은 `worker`, `gpt-5.6-sol`, `medium`과 `"none"` 또는 양의 정수 `fork_turns`를 함께 지원한다.
- 현재 세션의 호출 계약은 전체 이력 fork가 부모 역할·모델·추론 수준을 상속하므로 명시적 재정의와 함께 사용할 수 없다고 규정한다.
- Codex `rust-v0.146.0` 소스 구현은 전체 이력에서 `agent_type`을 거절하면서도 모델·추론 수준 재정의 함수를 호출하므로
  현재 세션 계약과 구현 세부가 완전히 같지는 않다. 이 환경의 실행 기준은 실제 노출된 현재 세션 계약으로 둔다.
- [공식 Subagents 문서](https://developers.openai.com/codex/subagents)는 built-in `worker`를 구현과 수정에 집중하는 agent로 설명한다.
  모델과 추론 수준은 custom agent 값이 있으면 그 값을, 없으면 명시적 spawn 값, `[agents]` 기본값, 부모 값 순서로 해석한다.
- 개발자 사례는 spawn마다 모델을 명시하는 방식, custom agent에 고정하는 방식, 별도 `codex exec` 작업자 우회로 나뉜다.
  모델 입력이 숨겨지거나 전체 이력 fork를 사용해 지정값이 무시·거절됐다는 문제 보고도 반복된다.
- 현재 `AGENTS.md`, `README.md`, `implement`, `implement-loop`, `verify`는 custom `implementer`를 구현 주체로 사용한다.
- `agents/implementer.toml`은 모델뿐 아니라 별도 행동·반환 규약을 정의해 built-in `worker`와 역할이 겹친다.
- analyzer와 verifier의 읽기 전용 역할, 기존 문서·검증 기준, Git 참고 정보와 기능 문서 추적 정책은 유지할 수 있다.

추정과 남은 근거 한계:

- 실제 worker 종단 호출의 실행 모델 표시는 별도로 검증하지 않았다. 아래 설계는 승인된 `spec.md`, 현재 호출 규격,
  공식 문서와 실제 설정 파일을 기준으로 확정하며, 소스 구현과 호출 계약의 차이 때문에 전체 이력 fork를 사용하지 않는다.

## 1. 구조

### 1.1 실행 책임

- main은 Task 선택, agent 호출, 사용자 확인, verifier 후보 검토, 최종 승인·거절과 문서 상태 변경을 소유한다.
- `analyzer`는 완성된 단계 문서 본문을 반환하는 읽기 전용 custom agent다.
- built-in `worker`는 Task 하나 또는 작은 Per-Request 변경을 공유 작업 공간에 구현한다.
- `verifier`는 지정된 검증 Skill로 독립 조사하고 후보 판단과 근거만 반환하는 읽기 전용 custom agent다.
- `implement`는 단일 구현 기준, `implement-loop`는 반복·재시도·정지 순서, `verify`는 검증·상태 전환 기준을 소유한다.

단일 Task는 main이 built-in `worker`를 명시적 모델·추론 수준과 제한된 이력으로 호출해 구현한다.
여러 Task는 main이 `implement-loop`에 따라 같은 흐름을 순차 반복한다(SPEC §5.1).

`agents/implementer.toml`은 삭제한다. 모델 선택 때문에 built-in 역할을 custom agent로 복제하지 않고,
호출 시점의 구조화된 입력으로만 구현 모델을 선택한다(SPEC §5.1, SPEC §5.3).

### 1.2 worker 호출과 기준 소유

main은 모든 worker 호출에 다음 값을 명시한다(SPEC §5.1, SPEC §5.2).

- `agent_type = "worker"`
- `model = "gpt-5.6-sol"`
- `reasoning_effort = "medium"`
- `fork_turns = "none"` 또는 필요한 최근 turn 수를 나타내는 양의 정수 문자열

기본값은 `"none"`이다. 최근 사용자 정정을 함께 전달할 실익이 있을 때만 필요한 최소 turn 수를 사용한다.
생략이나 `"all"`은 현재 세션 계약상 명시적 worker 선택과 양립하지 않으므로 사용하지 않는다.
소스 구현이 모델·추론 수준을 별도로 처리하더라도 역할 선택이 거절되고 호출 계약과도 다르므로 실행 근거로 삼지 않는다.

worker 메시지는 이전 대화 없이 실행 가능한 자체 완결적 입력이어야 한다.
Task의 목적·접근·검증 조건, 변경 범위, 승인된 기준 문서, 적용할 `skills/implement/SKILL.md`와 반환 형식을 포함한다.
일반 구현 규칙은 메시지에 복제하지 않고 `implement`를 단일 기준 소스로 사용한다(SPEC §5.2).

현재 호출 기능에 필요한 입력이 없거나 지정한 호출이 실패하면 main은 다른 모델, agent, 직접 구현이나 `codex exec`로 대체하지 않는다.
Task와 문서 상태를 바꾸지 않고 대상 Task, 누락된 입력 또는 오류와 영향을 보고한다(SPEC §5.2).

### 1.3 유지할 계약

`verify-analysis`의 여덟 판단 기준과 독립 검증, `spec-init`의 번호 표시와 `SPEC §5.N`,
`implement-init`의 한 번에 검증 가능한 Task 분해 기준을 유지한다(SPEC §5.4).

`verify`는 구현 주체 이름만 built-in `worker`로 바꾼다.
`quality`, `correctness`, `design/scope`와 main의 최종 판단·상태 전환 책임은 변경하지 않는다(SPEC §5.4).

`CONTEXT.md`의 브랜치·전체 기준 커밋 참고 정보와 읽기 전용 복원 비교를 유지한다(SPEC §5.5).
`.gitignore`의 `features/**` 허용 규칙과 README의 기능 문서 관리·추적 설명도 유지한다(SPEC §5.6).

## 2. 데이터 흐름

### 2.1 단일 Task

1. main이 승인된 문서와 현재 `implement.md`에서 대상 Task 하나와 허용 범위를 확정한다.
2. main이 worker 역할, 모델·추론 수준과 제한된 `fork_turns`를 함께 사용할 수 있는지 확인한다.
3. main이 Task 전체 내용, 변경 범위, 기준 문서, `implement` Skill, 검증 조건과 반환 형식을 메시지로 구성한다.
4. main이 built-in `worker`를 `gpt-5.6-sol`, `medium`, `"none"` 또는 필요한 최소 최근 turn 수로 호출한다.
5. worker가 기준 문서와 대상 파일을 조사하고 최소 범위로 구현해 필요한 확인을 실행한다.
6. 사용자 판단이나 상위 문서 변경이 필요하면 수정 없이 `blocked`와 근거를 main에 반환한다.
7. main이 직접 검증하거나 verifier에게 후보 판단을 요청한 뒤 최종 판단과 상태 전환을 수행한다.

Per-Request 변경도 worker를 사용할 때 같은 호출·반환 경계를 적용하되 기능 문서 상태는 다루지 않는다
(SPEC §5.1, SPEC §5.2).

### 2.2 여러 Task와 실패

main은 `implement-loop`에 따라 위에서부터 첫 미완료 Task를 선택하고 Task마다 새 worker를 호출한다.
동시에 쓰기 agent를 실행하지 않으며, 다음 Task 메시지에는 현재 Task가 의존하는 앞선 결과만 포함한다(SPEC §5.1).

승인이면 현재 Task만 완료 처리하고 다음 Task로 진행한다. 거절이면 가장 이른 수정 소유 단계로 돌아간다.
호출 기능이 모델 재정의를 지원하지 않거나 호출이 실패하면 다른 경로로 진행하지 않고 해당 Task에서 중단한다.
모든 Task가 승인됐을 때만 main이 `IMPLEMENT` 상태를 완료로 바꾼다(SPEC §5.1, SPEC §5.2).

### 2.3 유지되는 Git 흐름

`context-save`는 Git 작업 트리, 브랜치와 전체 `HEAD` 커밋을 읽기 전용으로 조회하고 실패한 필드를 `확인 불가`로 기록한다.
`context-restore`는 같은 규칙으로 다른 필드만 보고하며 복원 차단이나 작업 공간 변경을 수행하지 않는다(SPEC §5.5).

### 2.4 구성 표시

README는 custom `analyzer`, built-in `worker`, custom `verifier`를 구분한다.
`agents/*.toml`에는 analyzer와 verifier만 남고 구현 모델은 worker 호출 입력으로 선택된다고 설명한다(SPEC §5.3).

`implement-loop/agents/openai.yaml`은 main이 built-in worker 구현과 검증·상태 전환 순서를 조정한다고 표시한다.
`agents/implementer.toml` 삭제 뒤에도 `agents/*.toml` allowlist는 유효하므로 `.gitignore`는 변경하지 않는다(SPEC §5.6).

## 3. 인터페이스

### 3.1 spawn_agent 호출

```json
{
  "agent_type": "worker",
  "fork_turns": "none",
  "message": "<자체 완결적 worker 입력>",
  "model": "gpt-5.6-sol",
  "reasoning_effort": "medium",
  "task_name": "task_001"
}
```

일부 최근 대화가 필요하면 `fork_turns`에 `"1"` 이상의 양의 정수 문자열을 사용한다.
`task_name`은 Task ID를 영문 소문자·숫자·밑줄 규칙에 맞춰 변환한다(SPEC §5.1, SPEC §5.2).

### 3.2 worker 메시지와 반환

메시지에는 Task ID·제목, 목적·접근·검증 조건, 수정 범위, 기준 문서, `implement` Skill,
문서 상태 비변경과 무수정 `blocked` 경계를 포함한다. 공유 작업 공간에서 다른 작업자의 변경을 되돌리지 않고,
이미 생긴 변경에 맞춰 구현해야 한다는 협업 경계도 명시한다(SPEC §5.2).

반환은 `Status`, `Target`, `Changed files`, `Validation`, `Blocker`, `Residual risk`를 사용한다.
이 입력은 Task 맥락만 완결적으로 전달하고 built-in worker의 역할 자체는 재정의하지 않는다.

### 3.3 검증과 상태

verifier는 후보 판단만 반환하고 main이 최종 승인·거절과 상태 변경을 수행한다.
`verify`의 세 범주는 모두 차단 범주로 유지한다(SPEC §5.1, SPEC §5.4).

### 3.4 README와 Git 규약

README는 analyzer TOML, built-in worker의 명시적 호출, verifier TOML을 구분한다(SPEC §5.3).
현재 `CONTEXT.md` Git 참고 정보와 `.gitignore`의 `features/**` 허용 규칙은 유지한다(SPEC §5.5, SPEC §5.6).

## 4. 영향 범위

변경 대상:

- `AGENTS.md`: built-in worker의 명시적 호출, 자체 완결적 입력과 실패 처리
- `README.md`: custom implementer 제거와 analyzer·worker·verifier 구성
- `agents/implementer.toml`: 사용하지 않는 custom agent 삭제
- `skills/implement/SKILL.md`: 구현 주체를 worker로 변경하고 무수정 `blocked` 경계 유지
- `skills/implement-loop/SKILL.md`: worker 호출값, 반복 절차와 호출 실패 경로
- `skills/implement-loop/agents/openai.yaml`: built-in worker를 사용하는 main의 반복 조정 표시
- `skills/verify/SKILL.md`: 구현 주체를 worker로 변경

내용 유지 대상:

- `.gitignore`, `agents/analyzer.toml`, `agents/verifier.toml`
- `skills/verify-analysis`, `skills/implement-init`, `skills/spec-init`
- `skills/context-save`, `skills/context-restore`

`[agents].default_subagent_model`, custom `worker`, 별도 `codex exec`, analyzer·verifier 설정과 `nickname_candidates`,
Skill 설치 위치는 변경하지 않는다(SPEC §5.1~SPEC §5.6).

## 5. Decision Points

### 5.1 모델 선택 방식

- 옵션 A: built-in `worker`를 매번 명시적 모델·추론 수준으로 호출한다.
- 옵션 B: custom 구현 agent에 모델을 고정한다.
- 옵션 C: 별도 `codex exec` 작업자를 실행한다.
- 채택안: 옵션 A
- 근거: built-in 역할을 보존하면서 현재 spawn 입력으로 모델만 선택할 수 있다.
- trade-off: 호출값을 매번 전달해야 하지만 별도 역할과 전역 모델 변경을 만들지 않는다.

### 5.2 이력 전달 범위

- 옵션 A: 전체 이력을 전달한다.
- 옵션 B: 항상 fresh context를 사용한다.
- 옵션 C: fresh context를 기본으로 하고 필요한 최소 최근 turn만 선택적으로 전달한다.
- 채택안: 옵션 C
- 근거: 현재 세션 계약의 역할·모델 재정의와 양립하면서 최근 사용자 정정이 필요한 경우만 제한적으로 전달할 수 있다.
  Codex 0.146.0 소스 구현과 호출 계약의 차이도 전체 이력에 의존하지 않음으로써 실행 판단에 영향을 주지 않게 한다.
- trade-off: main이 자체 완결적 메시지를 구성해야 하지만 실행 입력이 재현 가능하다.

### 5.3 호출 실패

- 옵션 A: 다른 모델·agent·직접 구현으로 계속한다.
- 옵션 B: 해당 Task에서 중단하고 누락 입력 또는 오류와 영향을 보고한다.
- 채택안: 옵션 B
- 근거: 조용한 대체는 사용자가 선택한 모델과 실행 주체를 검증할 수 없게 한다.
- trade-off: 일시적인 실패도 재시도가 필요하지만 실제 실행 모델이 숨겨지지 않는다.

### 5.4 기준 소유와 반복

- 옵션 A: worker 메시지에 구현 규칙을 복제하고 worker가 상태 변경까지 수행한다.
- 옵션 B: `implement`를 단일 기준으로 두고 main이 `implement-loop`에 따라 검증·상태 변경을 조정한다.
- 채택안: 옵션 B
- 근거: 구현 규칙과 상태 소유의 중복을 막고 단일 Task와 반복 구현의 경계를 같게 유지한다.
- trade-off: main의 호출 입력과 반복 조정 책임이 명시적으로 늘어난다.

### 5.5 기존 문서·Git 계약

- 옵션 A: 구현 agent 변경과 함께 기존 문서·Git 계약도 다시 설계한다.
- 옵션 B: 이미 승인된 SPEC §5.4~§5.6 계약은 유지하고 구현 agent 표현만 바로잡는다.
- 채택안: 옵션 B
- 근거: 현재 설정이 해당 완료 조건을 충족하며 모델 선택 방식은 그 기준을 변경할 이유가 아니다.
- trade-off: 일부 Task는 변경이 아니라 보존 여부만 다시 검증한다.
