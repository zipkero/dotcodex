---
name: verify
description: "Judge implemented work against Task criteria and any SPEC requirements completed by the Task."
---

# Verify

## 목적
- 방금 구현한 단일 Task 또는 Per-Request 변경을 `approved` 또는 `rejected`로 판단하고 근거를 보고한다.
- Phased mode에서는 Task-level 판단을 기본으로 하되, 이번 Task 승인으로 완료되는 `SPEC §5.N`은 완료 조건 성립까지 확인한다.
- 별도 `verify.md`를 만들지 않는다.

## 컨텍스트 로딩
1. Phased mode:
   - feature `README.md`의 `SPEC`, `ANALYSIS`, `TASKS`가 모두 `[x]`여야 한다.
     하나라도 승인되지 않았으면 필요한 문서 작성 또는 검증 단계를 보고하고 중단한다.
   - 파일 존재만으로 승인을 추정하지 않으며, 나머지 진입 조건은 `implement` skill의 Phased mode와 동일하다.
   - `implement.md`의 대상 Task와 참조된 `spec.md`, `analysis.md`를 읽는다.
   - 사용자가 `task-<nnn>`을 지정하면 해당 Task를 검증한다.
   - 지정이 없으면 직전 구현 대상이 단일하게 식별될 때만 검증한다.
   - 변경 범위는 기본적으로 커밋하지 않은 작업 디렉터리 변경분이다.
     이미 커밋됐다면 커밋 식별자, 파일 목록 또는 비교 범위를 확인한다.
   - 대상 Task를 `[x]`로 가정했을 때 참조된 `SPEC §5.N`의 매핑 Task가 모두 `[x]`가 되는지 계산한다.
2. Per-Request mode:
   - feature 문서를 읽거나 갱신하지 않는다.
   - 사용자 요청, 변경 범위와 실행 결과를 확인한다.
3. 대상이 모호하면 판단하지 않고 식별 가능한 후보와 필요한 입력을 요청한다.

## verifier agent 사용 기준
- `agents/verifier.toml`은 읽기 전용 독립 검증 subagent이며, 이 skill의 판단 기준을 기준 소스로 따른다.
- 변경이 여러 파일에 걸치고 동작, 상태, 외부 I/O, 동시성, 경계 중 하나 이상에 영향을 주면 verifier agent를 사용한다.
- Per-Request 변경이라도 main의 diff 확인만으로 정확성을 판단하기 어렵거나 독립 검증 컨텍스트가 필요하면 사용을 고려한다.
- 문서, 오타, 정적 설정 문구처럼 diff만으로 판단 가능한 변경은 main이 직접 검증할 수 있다.
- 사용 여부는 근거 수집 전에 판단하며, 최종 승인/거절과 상태 전환은 main이 수행한다.

## 판단 기준
- Phased mode에서는 대상 Task의 `검증 조건`을 기준으로 관련 `SPEC §5.N`의 완료 조건·제약·제외 범위와
  `analysis.md`의 설계 결정을 함께 확인한다.
- 명시적으로 변경하기로 한 범위를 제외하고 기존 동작과 적용되는 공개 contract를 유지해야 한다.
- 이번 승인으로 완료되는 `SPEC §5.N`은 매핑된 Task 전체의 변경을 합쳐 완료 조건 자체를 판단한다.
  하나라도 성립하지 않으면 `correctness`로 reject한다.
- Per-Request mode에서는 사용자 요청, 변경 diff와 관련 실행 결과를 기준으로 삼는다.
- 문서 매핑만으로 승인하지 않으며, 현재 Task와 이번 승인으로 완료되는 요구사항만 판단한다.

## 검증 절차
판단은 대화 기억이나 구현 의도가 아니라 직접 확인한 파일, diff, 테스트 결과, 실행 로그와 산출물에 근거한다.

1. 구현과 변경된 테스트를 평가하기 전에 `판단 기준`에서 검증 기준 목록을 만든다.
   각 기준에는 출처와 독립적으로 판정 가능한 문장 하나를 적는다.
2. 변경 diff를 기준 목록과 대조한다. 기준을 구현에 맞춰 합치거나 삭제하거나 완화하지 않으며,
   새로운 기준은 적용 근거가 되는 출처를 확인한 경우에만 추가한다.
3. 각 기준에 근거를 연결한다.
   - 최소 근거는 변경 diff이다.
   - 외부 관찰 가능한 동작 변경은 테스트 또는 명확한 실행 결과로 확인한다.
   - 동작 변경이 없는 작업은 정확성이 diff에서 확인될 때 diff 기반 추론을 근거로 사용할 수 있다.
   - 추가하거나 수정한 테스트는 assertion과 구현 diff가 해당 기준을 실제로 검증하는지 확인한다.
   - 실행하지 못한 검증은 성공으로 간주하지 않는다.
     관련 기존 테스트를 실행하지 않았다면 그 사실을 보고하고, 판단할 수 없으면 `근거 부족`으로 처리한다.
4. 각 기준을 `충족`, `불충족`, `근거 부족`으로 판정한다.
   하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved`다.

검증 근거를 수집하는 동안 테스트, 운영 코드, 문서를 수정하지 않는다.
판단 후 문서 상태 변경은 `상태 전환`만 따른다.

## 출력 구조
1. Status: `approved` 또는 `rejected`
2. Target: Phased mode의 `task-<nnn>` 제목 또는 Per-Request 요청
3. Validation: 검증 기준 목록과 같은 순서로 기준마다 다음 내용을 적는다.
   - Criterion: 판정할 기준
   - Source: 기준의 출처
   - Evidence: 확인한 diff, 테스트, 실행 결과 또는 산출물
   - Result: `충족` | `불충족` | `근거 부족`
4. Completed requirements: Phased mode에서 이번 승인으로 완료되는 `SPEC §5.N`의 성립/불성립 또는 `없음`
5. `rejected`인 경우 Issues:
   - Category: `style/minor` | `correctness` | `design/scope`
   - Repair stage: 구현 수정은 `implement`, Task 기준 수정은 `tasks-init`, 설계 수정은 `analyze-init`,
     승인된 요구사항 수정은 `spec-init` 중 가장 이른 수정 소유 단계
   - 실제 근거와 함께 구체적 문제를 적는다.
6. `approved`인 경우 Explanation:
   - 무엇이 어떻게 충족되었는지 2-3문장으로 적는다.
   - 남은 리스크가 있으면 적는다.

## reject 분류
- 모든 reject category는 Task 승인을 막으며, 해소 전까지 체크박스는 `[x]`로 전환하지 않는다.
- `style/minor`: 명명, 주석, 포맷 같은 비동작 문제다. `implement`의 주석 기준 위반도 포함한다.
- `correctness`: 요구 동작, 검증 조건, invariant, 출력이 맞지 않는다.
  검증 조건 완화나 사례 삭제로 검증력이 낮아진 경우도 포함한다.
- `design/scope`: 설계 결정에서 이탈했거나 요청 범위를 초과 또는 미달했다.

## 상태 전환
- 검증 단계는 먼저 `approved` 또는 `rejected` 판단과 근거를 확정한다.
- main은 Phased mode에서 현재 승인된 `spec.md`와 `analysis.md` 기준으로 `approved`인 경우에만 대상 Task 하나를 `[x]`로 변경한다.
- 모든 Task가 현재 승인된 문서 기준으로 `approved`되어 `[x]`가 된 경우에만 main이
  `features/<feature-dir>/README.md`의 `IMPLEMENT`를 `[x]`로 변경하고
  `- <yyyy-MM-dd>: IMPLEMENT 완료` 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지한다.
- 완료되는 요구사항 불성립으로 rejected된 경우도 대상 Task만 `[ ]`로 유지하고, 앞선 `[x]` Task는 되돌리지 않는다.
- 이미 승인된 Task를 재검증해 실패한 경우에는 `[x]`를 `[ ]`로 되돌린다.
- 그 결과 모든 Task 완료 상태가 아니게 되면 README의 `[x] IMPLEMENT`를 `[ ] IMPLEMENT`로 되돌리고, 이력에 재검증 실패로 IMPLEMENT 상태를 되돌렸다는 한 줄을 추가한다.
- Per-Request mode는 문서나 체크박스를 갱신하지 않는다.
