---
name: implement-loop
description: "Run the remaining Tasks in a documented feature through implementation, independent verification, and state transition until completion or a user decision is required. Use when the user asks to implement all remaining Tasks or explicitly invokes implement-loop for features/<feature-dir>/implement.md."
---

# Implement Loop

## 목적

- `features/<feature-dir>/implement.md`의 남은 Task를 위에서부터 순서대로 구현하고 검증한다.
- 구현은 `implement`, 판단과 상태 전환은 `verify`의 기준을 그대로 따른다.
- 이 skill은 반복, 재시도와 정지 조건만 소유한다.

## 전제 조건

- 대상 feature와 구현 의도가 명확해야 한다.
- `implement.md`가 없으면 `implement-init`이 필요하다고 보고하고 중단한다.
- 첫 `[ ]` Task가 없으면 이미 완료됐다고 보고한다.

## 반복

1. 위에서부터 첫 `[ ]` Task를 선택한다.
2. Task의 `확인`이 테스트, 빌드, lint, 명령 출력이나 명확한 diff처럼 실행 가능한 근거를 가리키는지 확인한다.
3. `implement` 기준으로 해당 Task 하나를 구현한다.
4. `verify` 기준으로 독립 검증하고 상태 전환까지 처리한다.
5. `approved`이면 다음 `[ ]` Task로 진행한다.
6. `rejected`이면 재시도 또는 정지를 판단한다.

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

## 금지

- 통과를 위해 `spec.md`, `analysis.md`, Task 검증 조건을 약화하거나 넓히지 않는다.
- 테스트 assertion을 약화하거나 실패 사례를 삭제하지 않는다.
- 막힌 Task를 건너뛰거나 Task 순서를 바꾸지 않는다.
- `approved` 전에 Task를 `[x]`로 바꾸지 않는다.

## 완료 보고

- 이번 실행에서 승인된 Task
- 재시도한 Task와 reject 사유
- 멈췄다면 대상 Task, 정지 근거와 필요한 사용자 결정
- 모든 Task가 끝났다면 실행한 검증과 최종 상태
