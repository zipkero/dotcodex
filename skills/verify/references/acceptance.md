# 구현 승인 판정 계약

## 기준
- Per-Request는 사용자 요청, 변경 diff와 관련 실행 결과를 기준으로 한다.
- Phased는 `~/.codex/skills/verify/references/phased.md`의 기준을 함께 적용한다.
- 실제 동작과 이번 승인으로 완료되는 요구사항을 판정한다. 테스트는 assertion과 대상 코드를 대조해 기준 위반을 잡는지 확인한다. 빌드·정적 검사는 기능 검증을 대신하지 않는다.

승인 기준이 모두 `충족`이면 `approved`, 하나라도 `불충족` 또는 `근거 부족`이면 `rejected`다.

## 검증 근거
- 실행 명령·cwd·결과에 영향을 주는 환경·종료 코드·결과 요약·미실행 항목을 기록한다.
- 근거를 재사용하거나 인계할 때는 커밋과 검증 범위의 미커밋 변경·신규 파일을 함께 식별한다. 검증 커밋이 명시된 CI 실행, diff 또는 파일 해시 등 실제 검사한 내용을 구분할 수 있는 근거를 사용한다.
- 기존 근거는 실행 조건과 대상 코드가 현재 검증 범위에 대응할 때만 재사용한다.

## 출력
결과와 대상을 먼저 밝히고, 다음 정보를 제목·목록·표로 구성한다.

- `Status`: `approved` 또는 `rejected`
- `Target`: Phased는 `task-<nnn>: <제목>`, Per-Request는 요청한 변경
- `Requirements`: Phased에서 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`별 판정과 관련 검증 기준. 해당 요구사항이 없으면 생략한다.
- `Issues`: `rejected`일 때 문제별로 `Problem`에 관련 승인 기준·문제·근거, `Category`에 분류를 적는다.
  실제 문제는 `Repair stage`에 수정할 소유 단계를 적는다. Phased는 `implement`·`implement-init`·`design-init`·`spec-init`, Per-Request는 구현 수정에 `implement`, 프로젝트 기준 문서 수정에 `project-init`을 사용한다.
  근거 부족은 `Resolution`에 필요한 입력·환경·재검증 조건을 적는다.
- `Validation`: 검증 기준별로 출처, 확인한 동작·실행 결과와 `충족`·`불충족`·`근거 부족` 판정을 설명한다.
- `Explanation`: `approved`일 때 확인된 범위와 남은 위험·개선점

요구사항에 걸린 기준이 `불충족`이면 `Requirements`의 판정은 `불성립`, `불충족` 없이 `근거 부족`이면 `미확인`, 모두 `충족`이면 `성립`이다. Phased 판정 범위와 요구사항 전체 판정 시점은 `references/phased.md`를 따른다.

- `Category` 값의 뜻:
  - `correctness`: 완료 조건·Task 목적·검증 조건 불충족, 버그, 잘못된 출력
  - `design/scope`: 설계 결정 이탈, 범위 초과·미달
  - `evidence`: 불충족을 확인한 것이 아니라 성립 여부를 확인할 근거가 없음

Per-Request 결과 검토는 같은 판정 기준으로 결과·핵심 근거·미실행 검증만 간결하게 보고할 수 있다.
판정 중 대상 구현·테스트·승인 기준은 수정하지 않는다. 별도 검증 Markdown은 만들지 않는다.
