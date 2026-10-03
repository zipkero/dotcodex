# 구현 승인 판정 계약

## 기준
- Per-Request는 사용자 요청, 변경 diff와 관련 실행 결과를 기준으로 한다.
- Phased는 `~/.codex/skills/verify/references/phased.md`의 기준을 함께 적용한다.
- 판정 대상이 만들거나 보존해야 하는 실제 동작과 이번 승인으로 완료되는 요구사항만 판정한다.

## 절차
1. 원본 기준에서 독립적으로 판정할 기준과 출처를 정한다.
2. 각 기준을 실제 변경과 현재 코드 상태에 대조한다.
3. 기준에 대응하는 실행 결과나 확인 가능한 산출물을 연결한다. 기존 근거는 명령·cwd·환경·대상 코드와 미커밋 diff가 현재 상태에 대응할 때만 재사용한다.
4. 각 기준을 `충족`, `불충족`, `근거 부족`으로 판정한다.

하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved`다. verifier는 읽기 전용 후보를 반환하고 main이 최종 판정한다.

## 출력
아래 번호 항목 형식을 유지하고, 각 번호 항목은 들여쓰지 않는다.
1. Status: `approved` 또는 `rejected`
2. Target: Phased는 `task-<nnn>: 제목`
3. Validation: 기준마다 다음을 기록한다.
   - Criterion
   - Source
   - Evidence
   - Result: `충족` | `불충족` | `근거 부족`
4. Completed requirements: Phased에서 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`마다 아래에 `- SPEC §5.N: 성립 — <근거>` 또는 `- SPEC §5.N: 불성립 — <근거>` 한 줄, 없으면 이 줄에 `없음`만 적는다.
5. Issues: `rejected`일 때만 다음을 기록한다.
   - Category: `style/minor` | `correctness` | `design/scope` | `evidence` 중 하나를 backtick으로 적는다.
     - `style/minor`: 정확성을 깨지 않는 이름·주석·포맷 관례 위반
     - `correctness`: 완료 조건·Task 목적·검증 조건 불충족, 버그, 잘못된 출력
     - `design/scope`: 설계 결정 이탈, 범위 초과·미달
     - `evidence`: 불충족을 확인한 것이 아니라 성립 여부를 확인할 근거가 없음
   - Repair stage: `style/minor`·`correctness`·`design/scope`일 때 구현·Task·설계·spec 중 수정 소유 단계
   - Resolution: `evidence`일 때 `Repair stage` 대신 필요한 입력·환경·재검증 조건
   - Problem
6. Explanation: `approved`일 때 간결한 승인 근거와 남은 위험

Per-Request 결과 검토는 같은 판정 기준을 유지하면서 결과·핵심 근거·미실행 검증만 간결하게 보고할 수 있다. 검증 중 파일이나 상태를 변경하거나 별도 검증 Markdown을 만들지 않는다.
