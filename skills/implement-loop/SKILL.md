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
2. 검증 조건의 근거와 실행 가능성을 확인한다. 구현 후 수동 확인은 구현과 가능한 검증을 마친 뒤 요청한다.
3. Task마다 새 worker를 `implement`로 호출한다. `blocked`이면 main이 기존 승인과 입력으로 보완한 뒤 같은 역할을 다시 호출하고, 해소할 수 없으면 상태를 유지하고 중단한다.
4. worker가 `completed`를 반환하면 `verify`를 실행한다.
5. `approved`이면 다음 Task로 진행한다. `rejected`이면 사유와 근거를 기록해 재시도 또는 중단을 결정한다.

## 재시도와 기록
- 거절 사유에 실제 문제가 있으면 문제별 `Repair stage` 중 가장 앞선 소유 단계에서 수정한다. `implement` 외 단계의 수정이 필요하면 해당 단계로 반환하고 중단한다.
- `evidence` 문제만 있으면 main이 `Resolution`에 따라 입력·환경을 보완하고, `Resolution`과 Task `확인`에 지정된 검증을 실행해 문제·`Resolution`·근거를 `verify`에 전달한다.
- 재시도의 원인·근거·보완 내용과 구현 재시도·근거 재검증의 누적 횟수를 `최근 reject`에 기록하고, 재개 시 이어서 계산한다.

## 중단
- 승인 기준 변경·Task 재분해·별도 변경·사용자 결정이 필요하면 남은 Task를 변경하지 않고 중단하며 소유 단계와 재개 조건을 보고한다.
- 구현 원인을 해소할 수 없으면 중단한다.
- 현재 승인 밖의 되돌리기 어렵거나 외부에 영향을 주는 작업은 사용자 확인을 위해 중단한다.
- `evidence` 보완에 필요한 입력·환경을 갖출 수 없으면 중단한다.

## 완료 보고
- 최종 상태를 먼저 밝히고 승인된 Task, 재시도와 reject 사유를 요약한다. 중단한 경우 중단 Task·이유·재개 조건을 함께 보고한다.
