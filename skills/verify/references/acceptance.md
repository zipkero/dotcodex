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
1. Status: `approved` 또는 `rejected`
2. Target
3. Validation: 기준마다 다음을 기록한다.
   - Criterion
   - Source
   - Evidence
   - Result: `충족` | `불충족` | `근거 부족`
4. Completed requirements: Phased에서 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`, 없으면 `없음`
5. Issues: `rejected`일 때만 다음을 기록한다.
   - Category: `quality` | `correctness` | `design/scope` | `evidence`
   - Repair stage: `quality`·`correctness`·`design/scope`일 때 구현·Task·설계·spec 중 수정 소유 단계
   - Resolution: `evidence`일 때 `Repair stage` 대신 필요한 입력·환경·재검증 조건
   - Problem
6. Explanation: `approved`일 때 간결한 승인 근거와 남은 위험

Per-Request 결과 검토는 같은 판정 기준을 유지하면서 결과·핵심 근거·미실행 검증만 간결하게 보고할 수 있다. 검증 중 파일이나 상태를 변경하거나 별도 검증 Markdown을 만들지 않는다.
