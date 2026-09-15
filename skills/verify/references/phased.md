# Phased 검증

main과 verifier는 `입력과 판정 기준`을 적용한다. `상태 전환`은 main만 수행한다.
이 문서의 검증 절차는 Phased 작업에만 적용한다.

## 입력과 판정 기준
- 기능 디렉터리에 `README.md`, `spec.md`, `design.md`, `implement.md`가 있고 기능 상태판의 `SPEC`과 `DESIGN`이 모두 `[x]`여야 한다.
- 사용자가 기능 또는 Task의 구현 결과 검증을 요청했거나 직전 구현 대상이 단일하게 식별되어야 한다.
- `implement.md`의 대상 Task와 참조된 `spec.md`, `design.md`를 읽는다.
- main은 사용자가 `task-<nnn>`을 지정하면 해당 Task를 대상으로 확정하고, 지정이 없으면 직전 구현 대상이 단일하게 식별될 때만 대상을 확정한다.
  verifier를 사용하는 경우 main이 확정한 대상을 전달하며, verifier는 전달된 대상이 이 선택 조건에 맞는지 확인한다.
- 대상 Task의 `검증 조건`을 기준으로 관련된 적용 중인 `SPEC §5.N`의 완료 조건·제약·제외 범위와
  `design.md`의 설계 결정을 함께 확인한다.
- 먼저 대상 Task 자체를 `acceptance.md` 기준으로 판정한다. 아래의 요구사항 전체 완료 판단은 이번 승인으로 마지막 매핑 Task가 완료될 때만 추가한다.
- 대상 Task를 `[x]`로 가정했을 때 참조된 적용 중인 `SPEC §5.N`의 매핑 Task가 모두 `[x]`가 되는지 계산한다.
- 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`은 매핑된 Task 전체의 변경을 합쳐 완료 조건 자체를 판단한다.
  하나라도 성립하지 않으면 `correctness`로 reject한다. main이 전달한 매핑만 믿지 말고 원본 문서와 대조한다.
- 판정 절차·근거·출력은 `~/.codex/skills/verify/references/acceptance.md`의 계약을 따른다.

## 상태 전환
- 검증 단계는 먼저 `approved` 또는 `rejected` 판단과 근거를 확정한다.
- `Phased` 상태 전환은 적용 중인 `SPEC §5.N`과 그 매핑 Task만으로 계산한다.
- `approved`이면 현재 승인된 `spec.md`와 `design.md` 기준으로 대상 Task만 `[x]`로 바꾼다.
  적용 중인 모든 `SPEC §5.N`이 하나 이상의 Task에 매핑되고 각 매핑 Task가 승인됐을 때만 기능 `README.md`의 `IMPLEMENT`를 `[x]`로 바꾸고
  `- <yyyy-MM-dd>: IMPLEMENT 완료` 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지하고 앞서 승인된 Task는 보존한다.
- 승인된 Task의 재검증이 실패하면 해당 Task를 `[ ]`로 되돌린다.
  모든 Task가 완료된 상태가 아니게 되면 `IMPLEMENT`도 `[ ]`로 되돌리고 재검증 실패로 상태를 되돌렸다는 이력을 추가한다.
