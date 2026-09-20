# Phased 검증

## 입력과 판정
- 기능 문서가 모두 존재하고 `SPEC`과 `DESIGN`이 `[x]`여야 한다.
- main이 대상 Task 하나를 확정해 verifier에 전달한다.
- 대상 Task의 검증 조건, 관련 `SPEC §5.N`의 완료 조건·제약·제외 범위와 `design.md` 결정을 원본에서 확인한다.
- Task를 먼저 판정하고, 이번 승인으로 마지막 매핑 Task가 완료될 때만 매핑된 Task 전체의 변경을 합쳐 `SPEC §5.N` 자체를 판정한다.
- 판정 절차와 출력은 `~/.codex/skills/verify/references/acceptance.md`를 따른다.

## 상태 전환
- `approved`이면 main이 대상 Task만 `[x]`로 바꾸고 적용 기준, 실제 결과, 검증 당시 코드 상태와 근거 식별자를 `승인 근거`에 간결히 기록한다.
- 모든 적용 중인 `SPEC §5.N`이 Task에 매핑되고 매핑 Task가 모두 승인됐을 때만 `IMPLEMENT`를 `[x]`로 바꾸고 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지한다. 승인된 Task의 재검증이 실패하면 해당 Task와 `IMPLEMENT`를 `[ ]`로 되돌리고 이력을 남긴다.
- 과거 Task의 승인 근거를 현재 상태에서 추정해 만들지 않는다.
