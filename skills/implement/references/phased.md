# Phased 구현 조정

이 문서는 main의 Phased 진입·Task 선택·문서 처리 기준이다. worker에게는 선택한 Task와 승인된 원본 문서,
`~/.codex/skills/implement/references/worker.md`를 전달하며 이 조정 문서의 실행을 위임하지 않는다.

## 진입과 Task 선택
- 승인 상태와 부분 무효화는 `~/.codex/docs/phased-state.md`를 직접 읽어 적용한다.
- 사용자가 구현 의도를 밝히고 `features/<feature-dir>/` 또는 `features/<feature-dir>/implement.md`를 지정한 경우 Phased로 진입한다.
  기능 경로만 언급한 경우에는 구현하지 않고 요청 의도에 맞춰 분석, 설명, 검토로 처리한다.
- 기능 디렉터리에 `README.md`, `spec.md`, `design.md`, `implement.md`가 있고 기능 상태판의 `SPEC`과 `DESIGN`이 모두 `[x]`여야 한다.
  하나라도 충족되지 않으면 구현을 보류하고 필요한 작성 단계를 보고한다.
- 승인된 `spec.md`, `design.md`와 현재 `implement.md`를 읽는다.
- 사용자가 `task-<nnn>`을 지정하면 해당 Task를 잡고, 지정하지 않으면 위에서부터 첫 미완료 Task를 잡는다.
- Task가 없거나 이미 완료되었거나 둘 이상으로 해석되면 구현하지 않고 범위를 요청한다.
- 여러 Task 또는 기능 전체 구현은 `implement-loop`로 진행한다.

## 결과와 문서 처리
- 실제 구현이 Task의 `접근`과 달라졌다면 차이의 성격, 관련 `SPEC §5.N` / `DESIGN §X.Y`, 문서 반영 여부와 그 근거를 보고한다.
- `design.md`, Task의 `목적`, `검증 조건`, `참조`에 영향이 없는 구현 상세 차이만 main이 `접근`에 반영한다.
  그 밖의 차이는 문서를 바꾸지 않고 설계 문서나 구현 체크리스트 재작성이 필요하다고 보고한다.
- 구현 중에는 `SPEC`과 `DESIGN`의 승인 상태를 유지한다. Task 체크박스와 `IMPLEMENT` 갱신은
  `verify`의 `approved` 판단 이후 main이 수행한다.
- 상위 문서 변경으로 승인이 취소된 Task는 기존 구현이 남아 있어도 현재 기준으로 다시 검증한다. 영향 없음이 근거로 확인되어 승인이 유지된 Task는 불필요하게 재구현하지 않는다.
