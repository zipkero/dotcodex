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
   - `확인`에 수동 확인이 있으면 `manual_check`로 중단한다.
3. Task마다 새 worker를 `implement`로 호출한다. `blocked`이면 main이 기존 승인과 입력으로 보완 가능 여부를 확인한다.
   - 실제로 보완했으면 같은 역할을 다시 호출한다.
   - 해소할 수 없으면 상태를 유지하고 중단한다.
4. worker가 `completed`를 반환하면 `verify`를 실행한다.
5. `approved`이면 다음 Task로 진행한다. `rejected`이면 사유와 근거를 기록해 재시도 또는 중단을 결정한다.

## 재시도와 기록
- 거절 사유에 실제 문제가 있으면 문제별 `Repair stage` 중 가장 앞선 소유 단계에서 수정한다. `implement` 외 단계의 수정이 필요하면 해당 단계로 반환하고 중단한다.
- 거절 사유가 `evidence` 문제뿐이면 main이 다음 순서로 근거를 보완한다.
  1. 각 문제의 `Resolution`이 요구하는 입력·환경을 준비한다.
  2. 해당 `Resolution`과 Task `확인`에 지정된 명령·테스트를 실행한다.
  3. 수집한 근거와 해당 문제·`Resolution`을 전달해 `verify`를 다시 실행한다.
- 구현 재시도는 Task별 2회로 제한하고, 소진하면 `retry_exhausted`로 중단한다.
- 근거 재검증은 Task별 누적 2회로 제한한다. 다른 reject가 나와도 횟수를 유지하고, 구현 재시도와 구분해 집계·보고한다.
- 재시도 때는 미해결 원인과 보완 내용을 기록하고, `최근 reject`에 사유·근거와 구현 재시도·근거 재검증의 누적 횟수를 기록한다. 재개 시 해당 횟수를 이어서 계산한다.
- 재작업이 승인된 Task의 동작에 영향을 주면 해당 범위를 다시 검증한다.
- Task가 승인되면 `최근 reject`를 제거한다.

## 중단
- 다음 중 하나가 필요하면 남은 Task를 변경하지 않고 해당 소유 단계와 재개 조건을 보고한다.
  - 승인 기준의 의미 변경
  - Task 재분해
  - 승인된 결과와 무관한 별도 변경
  - 사용자 결정
- 구현 문제의 원인을 현재 조건에서 해소할 수 없으면 중단한다.
- 이미 승인된 Task의 동작이 성립하지 않는다고 드러나면 중단한다.
- 현재 사용자 승인 범위에 포함되지 않은, 되돌리기 어렵거나 외부에 영향을 주는 일이 필요하면 사용자 확인을 위해 중단한다.
- 거절 사유가 `evidence` 문제뿐일 때 필요한 입력·환경을 갖출 수 없거나 근거 재검증 2회를 소진했으면 중단한다.

## 완료 보고
- 최종 상태를 먼저 밝히고 승인된 Task, 재시도와 reject 사유를 요약한다. 중단한 경우 중단 Task·이유·재개 조건을 함께 보고한다.
- 중단 이유는 다음 값과 해당 상황을 설명하는 문장으로 보고한다.
  - `decision_needed`: 의미 변경·재분해·사용자 결정이 필요함
  - `regression`: 이미 승인된 Task의 동작이 성립하지 않음
  - `retry_exhausted`: 구현 재시도 한도 소진
  - `evidence_exhausted`: 근거를 더 보완할 수 없음
  - `manual_check`: 수동 확인이 필요해 자동으로 진행하지 않음
  - `approval_needed`: 현재 승인 범위 밖의 되돌리기 어렵거나 외부에 영향을 주는 일에 사용자 확인이 필요함
  - `blocked`: 그 밖의 사유로 구현이 blocked이거나 구현 문제의 원인을 현재 조건에서 해소할 수 없음
