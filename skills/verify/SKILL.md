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
   - 진입 조건은 `implement` skill의 Phased mode와 동일하다.
   - `implement.md`에서 대상 Task의 목적, 검증 조건, 참조를 읽는다.
   - `spec.md`의 완료 조건·제약·제외 범위와 필요한 `analysis.md` 설계 결정을 확인한다.
   - 사용자가 `task-<nnn>`을 지정하면 해당 Task를 검증한다.
   - 지정이 없으면 직전 구현 대상이 단일하게 식별될 때만 검증한다.
   - 검증할 변경 범위는 기본적으로 아직 커밋하지 않은 작업 디렉터리 변경분이다.
   - 변경이 이미 커밋된 경우에는 사용자가 지정한 Task와 함께 커밋 식별자, 파일 목록, 또는 비교 범위를 확인해 검증한다.
   - 대상 Task를 `[x]`로 가정했을 때 참조된 `SPEC §5.N`의 매핑 Task가 모두 `[x]`가 되는지 계산한다.
2. Per-Request mode:
   - feature 문서를 읽거나 갱신하지 않는다.
   - 사용자 요청, 위와 같은 변경 범위, 실행 결과를 기준으로 판단한다.
3. 대상이 모호하면 판단하지 않고 식별 가능한 후보와 필요한 입력을 요청한다.

## verifier agent 사용 기준
- `agents/verifier.toml`은 읽기 전용 독립 검증 subagent이며, 이 skill의 판단 기준을 기준 소스로 따른다.
- 변경이 여러 파일에 걸치고 동작, 상태, 외부 I/O, 동시성, 경계 중 하나 이상에 영향을 주면 verifier agent를 사용한다.
- Per-Request 변경이라도 main의 diff 확인만으로 정확성을 판단하기 어렵거나 독립 검증 컨텍스트가 필요하면 사용을 고려한다.
- 문서, 오타, 정적 설정 문구처럼 diff만으로 판단 가능한 변경은 main이 직접 검증할 수 있다.
- 사용 여부는 근거 수집 전에 판단하며, 최종 승인/거절과 상태 전환은 main이 수행한다.

## 근거 원칙
- 판단은 대화 기억이나 구현 의도가 아니라 파일, diff, 테스트 결과, 실행 로그, 산출물 확인에 근거한다.
- 최소 근거는 변경 diff이다. 동작 변경은 가능한 한 테스트나 실행 결과를 함께 확인한다.
- 내부 계산, 조건, 변환만 바뀌고 정확성이 diff에서 확인되는 경우에는 diff 기반 추론도 근거가 될 수 있다.
- “전에 논의했음”은 근거가 아니다. 필요한 파일이나 테스트를 다시 확인한다.
- `SPEC §5.N`과 `ANALYSIS §X.Y`는 추적 메타데이터이며, 승인 근거는 실제 산출물에서 가져온다.
- 근거만으로 정확성을 판단할 수 없으면 `rejected`로 판단하고 부족한 근거를 명시한다.

## 판단 규칙
- 1차 기준은 대상 Task의 `검증 조건`이다.
- 2차 기준은 관련 `SPEC §5.N`과 `spec.md`의 제약·제외 범위를 충족하는지 여부이다.
- 3차 기준은 `analysis.md`의 설계 결정을 벗어나지 않는지 여부이다.
- 이번 승인으로 완료되는 `SPEC §5.N`은 매핑된 Task들의 변경이 합쳐져서 완료 조건 문장 자체를 만족하는지 판단한다.
- 완료되는 요구사항이 하나라도 불성립이면 대상 Task 검증 조건 충족 여부와 무관하게 `correctness`로 reject한다.
- Per-Request mode에서는 사용자 요청과 변경 diff만 기준으로 삼는다.
- `approved`: 검증 조건을 충족하고 상위 기준을 위반하지 않는다고 근거로 판단할 수 있다.
- `rejected`: 충족하지 못하거나, 근거가 부족하거나, 범위나 설계 의도를 벗어난다.
- 문서 매핑이 맞는지만으로 승인하지 않는다.
- 실행하지 못한 검증은 성공으로 간주하지 않는다.
- 현재 Task와 그 승인으로 완료되는 요구사항만 판단하며, 다른 대기 Task나 향후 작업에 대한 의견은 두지 않는다.

## 출력 구조
1. Status: `approved` 또는 `rejected`
2. Target: Phased mode의 `task-<nnn>` 제목 또는 Per-Request 요청
3. Validation:
   - 기준 일치
   - 범위와 동작 정확성
   - 검증 근거
4. Completed requirements: Phased mode에서 이번 승인으로 완료되는 `SPEC §5.N`의 성립/불성립 또는 `없음`
5. `rejected`인 경우 Issues:
   - Category: `style/minor` | `correctness` | `design/scope`
   - 실제 근거와 함께 구체적 문제를 적는다.
6. `approved`인 경우 Explanation:
   - 무엇이 어떻게 충족되었는지 2-3문장으로 적는다.
   - 남은 리스크가 있으면 적는다.

## reject 분류
- 모든 reject category는 Task 승인을 막으며, 해소 전까지 체크박스는 `[x]`로 전환하지 않는다.
- `style/minor`: 명명, 주석, 포맷 같은 비동작 문제지만 Task 승인 전 수정이 필요하다.
- `correctness`: 요구 동작, 검증 조건, invariant, 출력이 맞지 않는다.
- `design/scope`: 설계 결정에서 이탈했거나 요청 범위를 초과 또는 미달했다.

## 상태 전환
- 검증 단계는 먼저 `approved` 또는 `rejected` 판단과 근거를 확정한다.
- Phased mode에서 `approved`인 경우에만 대상 Task 하나를 `[x]`로 변경한다.
- 모든 Task가 `[x]`가 되면 `features/<feature-dir>/README.md`의 `IMPLEMENT`를 `[x]`로 변경하고 `- <yyyy-MM-dd>: IMPLEMENT 완료` 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지한다.
- 완료되는 요구사항 불성립으로 rejected된 경우도 대상 Task만 `[ ]`로 유지하고, 앞선 `[x]` Task는 되돌리지 않는다.
- 이미 승인된 Task를 재검증해 실패한 경우에는 `[x]`를 `[ ]`로 되돌린다.
- 그 결과 모든 Task 완료 상태가 아니게 되면 README의 `[x] IMPLEMENT`를 `[ ] IMPLEMENT`로 되돌리고, 이력에 재검증 실패로 IMPLEMENT 상태를 되돌렸다는 한 줄을 추가한다.
- Per-Request mode는 문서나 체크박스를 갱신하지 않는다.

## 테스트 검증 근거 규칙
- `verify`는 검증 근거 수집 중 테스트, 운영 코드, 문서를 수정하지 않는다.
- 판단 후 상태 갱신 문서 수정은 `상태 전환` 기준만 따른다.
- 문서, 오타, 정적 설정 문구처럼 동작 변경이 없는 작업은 diff 확인만으로 승인할 수 있다.
- 상태 변경, 외부 I/O, 동시성, 새 경계를 포함하는 작업은 테스트 또는 명확한 실행 근거가 필요하다.
- 변경 범위에 기존 테스트가 있는데 실행하지 않았다면 제한 사항으로 보고한다.
- 같은 변경 안에 추가 또는 수정된 테스트는 통과만으로 검증 근거가 되지 않는다. 구현 diff와 함께 회귀 케이스를 실제로 다루는지 확인한다.
- 검증 조건 완화나 케이스 삭제처럼 검증력을 낮춘 변경은 `correctness`로 reject한다.
- 변경 diff의 주석이 `skills/implement/SKILL.md`의 주석 작성 기준을 위반하면 `style/minor`로 reject한다.

## 완료 보고
- 출력 구조에 따라 승인/거절 판단을 먼저 말한다.
