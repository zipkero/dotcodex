# Phased 구현 조정

## 진입과 Task 선택
- 승인 상태는 `~/.codex/docs/phased-state.md`를 따른다.
- 기능 `README.md`, `spec.md`, `design.md`, `implement.md`가 있고 `SPEC`과 `DESIGN`이 `[x]`여야 한다.
- 사용자가 `task-<nnn>`을 지정하면 해당 Task, 지정하지 않으면 위에서부터 첫 `[ ]` Task를 선택한다. 여러 Task나 기능 전체 구현은 `implement-loop`로 진행한다.
- 문서나 승인 상태가 부족하거나 대상을 하나로 확정할 수 없으면 구현하지 않고 필요한 소유 단계나 입력을 보고한다.

## 결과 처리
- Task의 `목적`, `검증 조건`, `참조`와 승인된 설계 의미가 유지되는 내부 접근 차이만 main이 `접근`에 반영한다. 그 밖의 차이는 해당 문서 소유 단계로 반환한다.
- Task 체크박스와 `IMPLEMENT`는 `verify` 승인 뒤 main이 갱신한다.
- 승인이 취소된 Task는 남아 있는 코드를 현재 기준과 대조해 필요한 수정·검증만 수행한다.
