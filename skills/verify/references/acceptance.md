# 구현 승인 판정 계약

## 기준
- Per-Request는 사용자 요청, 변경 diff와 관련 실행 결과를 기준으로 한다.
- Phased는 `~/.codex/skills/verify/references/phased.md`의 기준을 함께 적용한다.
- 판정 대상이 만들거나 보존해야 하는 실제 동작과 이번 승인으로 완료되는 요구사항만 판정한다.

기존 검증 근거는 명령·cwd·환경·대상 코드와 미커밋 diff가 현재 상태에 대응할 때만 재사용한다.

하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved`다.

## 출력
결과와 대상을 먼저 밝히고, 다음 정보를 제목·목록·표로 구성한다.

- `Status`: `approved` 또는 `rejected`
- `Target`: Phased는 `task-<nnn>: <제목>`, Per-Request는 요청한 변경
- `Requirements`: Phased에서 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`별 판정과 관련 검증 기준. 해당 요구사항이 없으면 생략한다.
- `Issues`: `rejected`일 때 문제별로 `Problem`에 문제와 근거, `Category`에 분류를 적는다.
  실제 문제는 `Repair stage`에 수정할 소유 단계를 적는다. Phased는 `implement`·`implement-init`·`design-init`·`spec-init`, Per-Request는 구현 수정에 `implement`, 프로젝트 기준 문서 수정에 `project-init`을 사용한다.
  근거 부족은 `Resolution`에 필요한 입력·환경·재검증 조건을 적는다.
- `Validation`: 기준마다 `Criterion`·`Source`·`Evidence`·`Result`를 묶어 적는다. `Result`는 `충족`, `불충족`, `근거 부족`이며 상세 근거는 이 항목에 둔다.
- `Explanation`: `approved`일 때 승인으로 확인된 범위와 남은 위험

요구사항에 걸린 기준이 `불충족`이면 `Requirements`의 판정은 `불성립`, `불충족` 없이 `근거 부족`이면 `미확인`, 모두 `충족`이면 `성립`이다. Phased 판정 범위와 요구사항 전체 판정 시점은 `references/phased.md`를 따른다.

- `Category` 값의 뜻:
  - `style/minor`: 정확성을 깨지 않는 이름·주석·포맷 관례 위반
  - `correctness`: 완료 조건·Task 목적·검증 조건 불충족, 버그, 잘못된 출력
  - `design/scope`: 설계 결정 이탈, 범위 초과·미달
  - `evidence`: 불충족을 확인한 것이 아니라 성립 여부를 확인할 근거가 없음

Per-Request 결과 검토는 같은 판정 기준으로 결과·핵심 근거·미실행 검증만 간결하게 보고할 수 있다.
판정 과정은 읽기 전용이다. 별도 검증 Markdown은 만들지 않는다.
