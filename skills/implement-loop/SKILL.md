---
name: implement-loop
description: "Coordinate implementation, verification, and retries for remaining Phased Tasks."
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
5. `approved`이면 다음 Task로 진행한다. `rejected`이면 사유와 근거를 기록해 재시도 또는 중단을 결정한다.

## 재시도와 기록
- `Category`가 `evidence`면 다른 재시도보다 먼저 처리한다. 구현하지 않고, main이 `Resolution`이 요구하는 입력·환경을 갖춰 `Resolution`과 Task `확인`의 명령·테스트를 실행해 모은 근거와 직전 `Resolution`을 넘겨 4번의 `verify`를 다시 한다.
- 근거 재검증은 Task별로 누적 2회까지이며, 중간에 다른 reject가 나와도 초기화하지 않는다. 구현 재시도와 따로 세고, 보고에서도 두 횟수를 나눠 적는다.
- 재시도 때는 미해결 원인과 보완 내용을 기록하고, reject는 `최근 reject`에 사유와 근거를 기록한다.
- 구현의 원인 해소가 불가능하면 중단한다.
- 재작업이 승인된 Task의 동작에 영향을 주면 해당 범위를 다시 검증한다.
- Task가 승인되면 `최근 reject`를 제거한다.

## 중단
- 요구사항·설계·Task 목적·검증 조건·참조의 의미 변경, Task 재분해, 승인된 결과와 무관한 별도 변경, 필요한 사용자 결정이 필요하면 남은 Task를 건드리지 않고 해당 소유 단계와 재개 조건을 보고한다.
- `evidence`로 rejected인데 근거를 더 보완할 수 없으면 중단한다. `Resolution`의 입력·환경을 갖출 수 없거나 근거 재검증 2회를 쓴 경우다.

## 완료 보고
- 첫 줄은 `<!-- prowl-workflow: v1 implement-loop -->`다.
- 승인된 Task, 재시도와 reject 사유, 중단 지점과 재개 조건, 최종 상태를 보고한다.
- 중단했으면 `Stopped at: task-<nnn>`과 `Stop reason:` 줄을 적는다. `Stop reason` 값은 `decision_needed`(의미 변경·재분해·사용자 결정), `blocked`(구현의 blocked, 또는 구현의 원인을 지금 조건에서 해소할 수 없음), `evidence_exhausted`(근거를 더 보완할 수 없음) 중 하나를 backtick으로 적고, `blocked`·`evidence_exhausted`면 재개 조건을 `Resolution:` 줄에 적는다.
