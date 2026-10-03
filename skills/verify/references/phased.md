# Phased 검증

## 입력과 판정
- 기능 문서가 모두 존재하고 `SPEC`과 `DESIGN`이 `[x]`여야 한다.
- main이 대상 Task 하나를 확정한다. 검증 주체는 `~/.codex/skills/verify/SKILL.md`의 선택 기준을 따른다.
- 대상 Task의 목적과 검증 조건, 관련 `SPEC §5.N`의 완료 조건·제약·제외 범위와 `design.md` 결정을 원본에서 확인한다.
- Task를 먼저 판정하고, 이번 승인으로 마지막 매핑 Task가 완료될 때만 매핑된 Task 전체의 변경을 합쳐 `SPEC §5.N` 자체를 판정한다.
- 판정 절차와 출력은 `~/.codex/skills/verify/references/acceptance.md`를 따른다.
- Phased 판정 출력의 첫 줄은 `<!-- prowl-workflow: v1 verify -->`다.

## 상태 전환
- `approved`이면 main이 대상 Task만 `[x]`로 바꾸고 적용 기준, 실제 결과, 검증 당시 코드 상태와 근거 식별자를 `승인 근거`에 간결히 기록한다.
- Task 승인 후 공통 상태 계약에 따라 `IMPLEMENT`를 갱신하고 이력을 기록한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지한다. 승인된 Task의 재검증이 실패하면 해당 Task와 `IMPLEMENT`를 `[ ]`로 되돌리고 이력을 남긴다.
