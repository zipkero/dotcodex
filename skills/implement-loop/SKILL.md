---
name: implement-loop
description: "Coordinate implementation, verification, and bounded retries for remaining Phased Tasks."
---

# Implement Loop

## 전제
- 대상 기능의 구현 의도가 명확하고 `~/.codex/skills/implement/references/phased.md`의 진입 조건을 충족해야 한다.
- 승인 상태와 기록은 `~/.codex/docs/phased-state.md`를 따른다.
- 첫 `[ ]` Task가 없으면 모든 적용 중인 `SPEC §5.N`의 Task 매핑과 `IMPLEMENT [x]`를 확인한 뒤 완료를 보고한다.

## 반복
1. 위에서부터 첫 `[ ]` Task를 선택한다.
2. Task의 검증 조건이 확인 가능한 근거를 지정하는지 확인한다.
3. `implement`로 worker를 호출한다. `blocked`이면 main이 기존 승인과 입력으로 보완할 수 있는지 확인하고, 실제로 보완했으면 같은 역할을 다시 호출한다. 해소할 수 없으면 상태를 유지하고 중단한다.
4. worker가 `completed`를 반환하면 `verify`를 실행한다.
5. `approved`이면 main이 Task와 기능 상태를 갱신하고 다음 Task로 진행한다. `rejected`이면 사유와 근거를 기록해 재시도 또는 중단을 결정한다.

## 재시도와 기록
- 최초 호출, 입력 보완 뒤 호출과 `blocked` 뒤 실제 재호출을 포함해 Task당 worker 호출은 최대 3회다.
- 호출 전에 `시도: <1-3>/3`을 기록하고 loop 재실행과 문서 갱신 뒤에도 유지한다. reject는 `최근 reject`에 사유와 근거를 기록한다.
- 구현 변경 없이 근거만 보완해 다시 검증하는 경우 worker 호출 횟수를 늘리지 않는다.
- 필요한 근거를 보완할 수 없거나 재검증에도 같은 근거가 부족하면 중단한다.
- 재작업이 승인된 Task의 동작에 영향을 주면 해당 범위를 다시 검증한다.
- Task가 승인되면 `시도`와 `최근 reject`를 제거한다.

## 중단
- 요구사항·설계·Task 목적·검증 조건·참조의 의미 변경, Task 재분해, 승인 범위 밖 변경, 필요한 사용자 결정, worker 호출 한도 소진이 필요하면 남은 Task를 건드리지 않고 해당 소유 단계와 재개 조건을 보고한다.
- Task를 건너뛰거나 순서를 바꾸지 않는다.

## 완료 보고
- 승인된 Task, 재시도와 reject 사유, 중단 지점과 재개 조건, 최종 상태를 보고한다.
