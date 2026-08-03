---
name: implement-loop
description: "Coordinate main's implementation, independent verification, and state transition loop for the remaining Tasks in a documented feature until completion or a user decision is required. Use when the user asks to implement all remaining Tasks or explicitly invokes implement-loop for features/<feature-dir>/implement.md."
---

# Implement Loop

## 목적

- main이 `features/<feature-dir>/implement.md`의 남은 Task를 위에서부터 순서대로 구현하고 검증하도록 조정한다.
- 각 Task의 구현은 built-in `worker`가 `implement` 기준으로 수행하고, verifier는 필요할 때 `verify` 기준의 후보 판단만 반환한다.
- 최종 승인·거절과 상태 전환은 main만 수행하며, 이 skill은 반복, 재시도와 정지 조건만 정의한다.

## 전제 조건

- 대상 기능과 구현 의도가 명확해야 한다.
- `implement` skill §컨텍스트 로딩의 `Phased` 작업 진입 조건을 충족해야 한다.
  충족하지 않으면 필요한 작성 단계 또는 `verify-analysis`를 안내하고 중단한다.
- 첫 `[ ]` Task가 없으면 이미 완료됐다고 보고한다.

## 반복

1. 위에서부터 첫 `[ ]` Task를 선택한다.
2. Task의 `확인`이 테스트, 빌드, lint, 명령 출력이나 명확한 diff처럼 실행 가능한 근거를 가리키는지 확인한다.
3. main은 `implement`의 worker 호출 계약에 따라 worker를 호출한다. 이 반복에서 양의 정수 `fork_turns`는 최근 사용자 정정이 필요할 때만 사용한다.
4. 호출 메시지에는 해당 Task의 목적·접근·검증 조건, 수정 범위와 승인된 `spec.md`·`analysis.md`·`implement.md`를 포함한다.
5. worker는 단일 Task를 구현하고 `implement`의 반환 형식으로 결과만 main에 반환한다.
6. main은 `verify`의 agent 사용 기준에 따라 직접 근거를 확인하거나 verifier에게 후보 판단을 요청한다.
   verifier의 결과를 검토해 최종 승인·거절을 확정하고, `approved`인 경우에만 상태를 전환한다.
7. `approved`이면 다음 `[ ]` Task로 진행한다.
8. `rejected`이면 재시도 또는 정지를 판단한다.

## 재시도

- 구현 수정만으로 reject 사유를 해결할 수 있으면 같은 Task를 다시 구현한다.
- reject 사유와 근거를 다음 구현의 입력으로 유지한다.
- 재시도는 Task당 2회로 제한한다. 처음 구현을 포함해 최대 3번 시도한다.
- 재작업이 앞서 승인된 Task의 동작에 영향을 주면 영향받은 범위를 다시 검증한다.

## 정지 조건

다음 중 하나면 남은 Task를 건드리지 않고 중단한다.

- `spec.md`, `analysis.md`, Task 목적, 검증 조건이나 참조를 바꿔야 한다.
- 완료 조건이 충돌하거나 현재 설계로 달성할 수 없다.
- 실행 가능한 검증 근거 없이 수동 판단에만 의존한다.
- Task가 독립적으로 검증 가능한 동작 단위가 아니어서 재분해가 필요하다.
- 재시도 한도를 소진했다.
- 되돌리기 어렵거나 외부에 영향을 주는 작업에 사용자 결정이 필요하다.
- `implement`의 worker 호출 계약을 충족할 수 없거나 지정한 worker 호출이 실패했다.

사용자 확인이 필요한 경우 main이 확인하며 worker와 verifier는 사용자에게 직접 묻지 않는다.
worker 호출 계약 관련 실패는 `implement`의 대체 금지와 보고 규칙을 따른다.

## 금지

- 통과를 위해 `spec.md`, `analysis.md`, Task 검증 조건을 약화하거나 넓히지 않는다.
- 테스트 assertion을 약화하거나 실패 사례를 삭제하지 않는다.
- 막힌 Task를 건너뛰거나 Task 순서를 바꾸지 않는다.
- `approved` 전에 Task를 `[x]`로 바꾸지 않는다.

## 완료 보고

- 이번 실행에서 승인된 Task
- 재시도한 Task와 reject 사유
- 멈췄다면 대상 Task, 정지 근거와 필요한 사용자 결정
- 모든 Task가 끝났다면 실행한 검증과 main이 확정한 최종 상태
