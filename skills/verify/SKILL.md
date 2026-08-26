---
name: verify
description: "Verify a documented Task or per-request implementation against its criteria, diff, and execution evidence."
---

# Verify

## 목적
- 구현된 단일 Task 또는 Per-Request 변경을 `approved` 또는 `rejected`로 최종 판단하고 근거를 보고하는 기준을 정의한다.
- `Phased` 작업에서는 Task 단위 판단을 기본으로 하되, 이번 Task 승인으로 완료되는 `SPEC §5.N`은 완료 조건 성립까지 확인한다.
- 검증 상세를 별도 Markdown 문서로 만들지 않는다.

## 컨텍스트 로딩
1. `Phased` 작업:
   - 기능 디렉터리에 `README.md`, `spec.md`, `analyze.md`, `implement.md`가 있고 기능 상태판의 `SPEC`과 `ANALYZE`가 모두 `[x]`여야 한다.
   - `implement.md`의 `Plan status`가 `ready`가 아니면 `implement-init`이 필요하다고 보고하고 검증하지 않는다.
   - 사용자가 기능 또는 Task의 구현 결과 검증을 요청했거나 직전 구현 대상이 단일하게 식별되어야 한다.
   - `implement.md`의 대상 Task와 참조된 `spec.md`, `analyze.md`를 읽는다.
   - 사용자가 `task-<nnn>`을 지정하면 해당 Task를 검증한다.
   - 지정이 없으면 직전 구현 대상이 단일하게 식별될 때만 검증한다.
   - 대상 Task를 `[x]`로 가정했을 때 참조된 적용 중인 `SPEC §5.N`의 매핑 Task가 모두 `[x]`가 되는지 계산한다.
2. `Per-Request` 작업:
   - 사용자가 기능 문서 없는 변경의 검증을 요청했거나 직전 Per-Request 구현 대상이 단일하게 식별되어야 한다.
   - 기능 문서를 읽거나 갱신하지 않는다.
   - 사용자 요청, 변경 범위와 실행 결과를 확인한다.
3. main이 검증 대상을 하나로 확정한다. 대상이 모호하면 식별 가능한 후보와 필요한 입력을 사용자에게 제시하고 verifier를 호출하지 않는다.
4. main은 검증 대상을 확정한 뒤 다음 우선순위로 공통 변경 범위를 확정한다.
   1. 사용자가 지정한 commit, 파일 목록 또는 비교 범위
   2. 같은 흐름에서 implement worker가 반환한 `Changed files`와 해당 diff
   3. 복원 작업이면 `CONTEXT.md`의 changed files, saved branch와 base HEAD
   4. 위 근거가 없으면 working tree와 Git history에서 수집한 후보
   5. 후보가 여러 개이거나 범위가 불확실하면 후보와 각 근거를 제시해 사용자에게 확인하며, 확인 전에는 verifier를 호출하거나 판정하지 않는다.

## verifier agent 사용 기준
- 아래 기준에 따라 이름 있는 custom agent `verifier`에게 이 skill을 기준으로 독립 후보 판단을 맡긴다.
- verifier 호출에는 검증 대상과 변경 범위, 선행 문서, 적용되는 프로젝트 `AGENTS.md`, `docs/languages.md`와 해당 언어 문서의 정확한 경로,
  실행 근거 위치를 포함하고, verifier가 해당 지침 파일을 직접 읽어 적용하도록 명시한다.
- 변경이 여러 파일에 걸치고 동작, 상태, 외부 I/O, 동시성, 경계 중 하나 이상에 영향을 주면 verifier agent를 사용한다.
- Per-Request 변경이라도 diff 확인만으로 정확성을 판단하기 어렵거나 독립 검증 컨텍스트가 필요하면 사용을 고려한다.
- 문서, 오타, 정적 설정 문구처럼 diff만으로 판단 가능한 변경은 직접 검증할 수 있다.
- main은 대상을 확정한 뒤 근거 수집 전에 verifier 사용 여부를 판단하고, 반환된 후보 근거를 검토해 최종 `approved` 또는 `rejected`와
  아래 `상태 전환`을 적용한다.

## 판단 기준
- `Phased` 작업에서는 대상 Task의 `검증 조건`을 기준으로 관련된 적용 중인 `SPEC §5.N`의 완료 조건·제약·제외 범위와
  `analyze.md`의 설계 결정을 함께 확인한다.
- 명시적으로 변경하기로 한 범위를 제외하고 기존 동작과 적용되는 공개 규약·프로젝트·언어 관례를 유지해야 한다.
- 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`은 매핑된 Task 전체의 변경을 합쳐 완료 조건 자체를 판단한다.
  하나라도 성립하지 않으면 `correctness`로 reject한다.
- `Per-Request` 작업에서는 사용자 요청, 변경 diff와 관련 실행 결과를 기준으로 삼는다.
- 문서 매핑만으로 승인하지 않으며, 현재 Task와 이번 승인으로 완료되는 요구사항만 판단한다.

## 검증 절차
판단은 대화 기억이나 구현 의도가 아니라 직접 확인한 파일, diff, 테스트 결과, 실행 로그와 산출물에 근거한다.

1. 구현과 변경된 테스트를 평가하기 전에 `판단 기준`에서 검증 기준 목록을 만든다.
   각 기준에는 출처와 독립적으로 판정 가능한 문장 하나를 적는다.
2. 변경 diff를 기준 목록과 대조한다. 기준을 구현에 맞춰 합치거나 삭제하거나 완화하지 않으며,
   새로운 기준은 적용 근거가 되는 출처를 확인한 경우에만 추가한다.
3. 각 기준에 최소 변경 diff를 연결하고, 외부 동작 변경은 테스트나 명확한 실행 결과로 확인한다.
   동작 변경이 없으면 정확성이 diff에서 확인될 때만 추론을 근거로 사용할 수 있다.
   변경한 테스트는 assertion과 구현이 기준을 실제로 검증하는지 확인하며, 실행하지 못한 검증은 보고하고 판단할 수 없으면 `근거 부족`으로 처리한다.
4. 각 기준을 `충족`, `불충족`, `근거 부족`으로 판정한다.
   하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved`다.

검증 근거를 수집하는 동안 테스트, 운영 코드, 문서를 수정하지 않는다.
판단 후 문서 상태 변경은 `상태 전환`만 따른다.

## 출력 구조
1. Status: `approved` 또는 `rejected`
2. Target: `Phased` 작업의 `task-<nnn>` 제목 또는 `Per-Request` 요청
3. Validation: 검증 기준 목록과 같은 순서로 기준마다 다음 내용을 적는다.
   - Criterion: 판정할 기준
   - Source: 기준의 출처
   - Evidence: 확인한 diff, 테스트, 실행 결과 또는 산출물
   - Result: `충족` | `불충족` | `근거 부족`
4. Completed requirements: `Phased` 작업에서만 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`의 성립 또는 불성립. 없으면 `없음`
5. `rejected`인 경우 Issues:
   - Category: `quality` | `correctness` | `design/scope` | `evidence`
   - Repair stage: `quality`, `correctness`, `design/scope`일 때 구현 수정은 `implement`, Task 기준 수정은 `implement-init`,
     설계 수정은 `analyze-init`, 승인된 요구사항 수정은 `spec-init` 중 가장 이른 수정 소유 단계
   - Resolution: `evidence`일 때 `Repair stage` 대신 필요한 입력·환경·재검증 조건
   - Problem: 실제 근거가 있는 구체적 문제
6. `approved`인 경우 Explanation: `Validation`을 반복하지 않고 결과를 1-2문장으로 요약하며, 남은 위험이 있을 때만 덧붙인다.

## reject 분류
- `quality`: 적용되는 구현·프로젝트·언어 관례를 위반한 비동작 품질 문제다.
- `correctness`: 요구 동작, 검증 조건, 불변 조건, 출력이 맞지 않는다.
  검증 조건 완화나 사례 삭제로 검증력이 낮아진 경우도 포함한다.
- `design/scope`: 설계 결정에서 이탈했거나 요청 범위를 초과 또는 미달했다.
- `evidence`: 필수 입력이나 검증 환경이 없거나 재현 가능한 실행 근거가 부족해 충족 여부를 판단할 수 없다.

## 상태 전환
- 검증 단계는 먼저 `approved` 또는 `rejected` 판단과 근거를 확정한다.
- `Phased` 상태 전환은 적용 중인 `SPEC §5.N`과 그 매핑 Task만으로 계산한다.
- `approved`이면 현재 승인된 `spec.md`와 `analyze.md` 기준으로 대상 Task만 `[x]`로 바꾼다.
  적용 중인 모든 `SPEC §5.N`이 하나 이상의 Task에 매핑되고 각 매핑 Task가 승인됐을 때만 기능 `README.md`의 `IMPLEMENT`를 `[x]`로 바꾸고
  `- <yyyy-MM-dd>: IMPLEMENT 완료` 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지하고 앞서 승인된 Task는 보존한다.
- 승인된 Task의 재검증이 실패하면 해당 Task를 `[ ]`로 되돌린다.
  모든 Task가 완료된 상태가 아니게 되면 `IMPLEMENT`도 `[ ]`로 되돌리고 재검증 실패로 상태를 되돌렸다는 이력을 추가한다.
- `Per-Request` 작업은 문서나 체크박스를 갱신하지 않는다.
