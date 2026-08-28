---
name: implement-loop
description: "Implement and verify all remaining Tasks in a documented feature, coordinating retries and state transitions until completion or a user decision is required."
---

# Implement Loop

## 목적

- `features/<feature-dir>/implement.md`의 남은 Task를 위에서부터 순서대로 구현하고 검증하도록 조정한다.
- 구현은 `implement`, 검증과 상태 전환은 `verify`를 따른다.
- 이 skill은 Task 선택, 반복, 재시도와 정지 조건만 정의한다.

## 전제 조건

- 대상 기능과 구현 의도가 명확해야 한다.
- `implement` skill §컨텍스트 로딩의 `Phased` 작업 진입 조건을 충족해야 한다.
  충족하지 않으면 필요한 작성 단계를 안내하고 중단한다.
- 첫 `[ ]` Task가 없으면 적용 중인 모든 `SPEC §5.N`이 Task에 매핑되고 README의 `IMPLEMENT`가 `[x]`일 때만 완료를 보고한다.
  하나라도 충족되지 않으면 완료로 간주하지 않고 확인한 불일치와 `verify`의 상태 전환 복구가 필요함을 보고한 뒤 중단한다.

## 반복

1. 위에서부터 첫 `[ ]` Task를 선택한다.
2. Task의 `확인`이 테스트, 빌드, lint, 명령 출력이나 명확한 diff처럼 실행 가능한 근거를 가리키는지 확인한다.
3. Task 하나를 `implement` 기준으로 구현한다. worker가 `blocked`를 반환하면 문서와 Task 상태를 유지하고 즉시 중단하며,
   `completed`를 반환한 경우에만 `verify`를 실행한다.
4. `verify`가 `approved`이면 main이 `verify`의 상태 전환 규칙을 적용하고 해당 Task의 `시도`, `최근 reject` 기록을 제거한 뒤 다음 `[ ]` Task로 진행한다.
5. `rejected`이면 반환된 사유와 근거를 기록하고 재시도 또는 정지를 판단한다.

## 재시도

- 구현 수정만으로 reject 사유를 해결할 수 있으면 같은 Task를 다시 구현한다.
- reject 사유와 근거를 다음 구현의 입력으로 전달한다.
- 최초 구현을 포함한 Task당 최대 3회 한도는 해당 Task가 승인될 때까지 `implement-loop` 재실행 사이에도 유지한다.
- reject가 발생하면 해당 Task에 `시도: <1-3>/3`, `최근 reject: <verify 사유와 근거>`만 기록하고 다음 시도에서 갱신한다.
- `evidence` reject는 구현을 재시도하지 않고, `Resolution`의 필요한 입력·환경·재검증 조건을 보고한 뒤 중단한다.
- 구현 재시도를 하지 않기로 했거나 한도를 소진하면 `verify`가 반환한 결과, 사유와 근거 및 재시도 중단 이유를 그대로 보고하고 중단한다.
- 재작업이 앞서 승인된 Task의 동작에 영향을 주면 영향받은 범위를 다시 검증한다.

## 정지 조건

다음 중 하나면 남은 Task를 건드리지 않고 중단한다.

- `spec.md`, `design.md`, Task 목적, 검증 조건이나 참조를 바꿔야 한다.
- 완료 조건이 충돌하거나 현재 설계로 달성할 수 없다.
- 실행 가능한 검증 근거 없이 수동 판단에만 의존한다.
- Task가 독립적으로 검증 가능한 동작 단위가 아니어서 재분해가 필요하다.
- 재시도 한도를 소진했다.
- 되돌리기 어렵거나 외부에 영향을 주는 작업에 사용자 결정이 필요하다.

위임 실패 시 다음 진행 방법은 `implement`의 호출 계약을 따른다.

## 금지

- 막힌 Task를 건너뛰거나 Task 순서를 바꾸지 않는다.

## 완료 보고

- 이번 실행에서 승인된 Task
- 재시도한 Task와 reject 사유
- 멈췄다면 대상 Task, 정지 근거와 필요한 사용자 결정
- 모든 Task가 끝났다면 실행한 검증과 확정한 최종 상태
