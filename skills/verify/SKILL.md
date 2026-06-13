---
name: verify
description: "Judge implemented work against SPEC and implement.md verification criteria."
---

# Verify

## 목적
- 방금 구현한 단일 Task 또는 Per-Request 변경이 완료 기준을 만족하는지 판단한다.
- Phased mode의 verify는 Task-level 판단 도구이며, feature 단위 통합 검증 단계가 아니다.
- 판단은 `approved` 또는 `rejected`로 반환하고, 근거를 함께 보고한다.
- 별도 `verify.md`를 만들지 않는다.

## 컨텍스트 로딩
1. Phased mode:
   - 진입 조건은 `implement` skill의 Phased mode와 동일하다.
   - `implement.md`에서 대상 Task의 목적, 검증 조건, 참조를 읽는다.
   - `spec.md` 완료 조건과 필요한 `analysis.md` 설계 결정을 확인한다.
   - 사용자가 `task-<nnn>`을 지정하면 해당 Task를 검증한다.
   - 지정이 없으면 직전 구현 대상이 단일하게 식별될 때만 검증한다.
2. Per-Request mode:
   - feature 문서를 읽거나 갱신하지 않는다.
   - 사용자 요청, 변경 diff, 실행 결과를 기준으로 판단한다.
3. 대상이 모호하면 판단하지 않고 식별 가능한 후보와 필요한 입력을 요청한다.

## 근거 원칙
- 판단은 대화 기억이나 구현 의도가 아니라 파일, diff, 테스트 결과, 실행 로그, 산출물 확인에 근거한다.
- 최소 근거는 변경 diff이다. 동작 변경이 있으면 가능한 한 테스트나 실행 결과를 함께 확인한다.
- 내부 계산, 조건, 변환만 바뀌고 정확성이 diff에서 확인되는 경우에는 diff 기반 추론도 근거가 될 수 있다.
- “전에 논의했음”은 근거가 아니다. 필요한 파일이나 테스트를 다시 확인한다.
- `SPEC §5.N`과 `ANALYSIS §X.Y`는 추적 메타데이터이며, 승인 근거는 실제 산출물에서 가져온다.
- 근거만으로 정확성을 판단할 수 없으면 `rejected`로 판단하고 부족한 근거를 명시한다.

## 판단 규칙
- 1차 기준은 대상 Task의 `검증 조건`이다.
- 2차 기준은 관련 `SPEC §5.N`을 위반하지 않는지 여부이다.
- 3차 기준은 `analysis.md`의 설계 결정과 제외 범위를 벗어나지 않는지 여부이다.
- Per-Request mode에서는 사용자 요청과 변경 diff만 기준으로 삼는다.
- `approved`: 검증 조건을 충족하고 상위 기준을 위반하지 않는다고 근거로 판단할 수 있다.
- `rejected`: 충족하지 못하거나, 근거가 부족하거나, 범위나 설계 의도를 벗어난다.
- 문서 매핑이 맞는지만으로 승인하지 않는다.
- 실행하지 못한 검증은 성공으로 간주하지 않는다.

## 출력 구조
1. Status: `approved` 또는 `rejected`
2. Target: Phased mode의 `task-<nnn>` 제목 또는 Per-Request 요청
3. Validation:
   - 기준 일치
   - 범위와 동작 정확성
   - 검증 근거
4. `rejected`인 경우 Issues:
   - Category: `style/minor` | `correctness` | `design/scope`
   - 실제 근거와 함께 구체적 문제를 적는다.
5. `approved`인 경우 Explanation:
   - 무엇이 어떻게 충족되었는지 2-3문장으로 적는다.
   - 남은 리스크가 있으면 적는다.

## reject 분류
- `style/minor`: 명명, 주석, 포맷 같은 비동작 문제지만 Task 승인 전 수정이 필요하다.
- `correctness`: 요구 동작, 검증 조건, invariant, 출력이 맞지 않는다.
- `design/scope`: 설계 결정에서 이탈했거나 요청 범위를 초과 또는 미달했다.

## 상태 전환
- 검증 단계는 먼저 `approved` 또는 `rejected` 판단과 근거를 확정한다.
- Phased mode에서 `approved`인 경우에만 대상 Task 하나를 `[x]`로 변경한다.
- 모든 Task가 `[x]`가 되면 `docs/<feature-dir>/README.md`의 `IMPLEMENT`를 `[x]`로 변경하고 `- <yyyy-MM-dd>: IMPLEMENT 완료` 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지한다.
- 이미 승인된 Task를 재검증해 실패한 경우에는 `[x]`를 `[ ]`로 되돌린다.
- 그 결과 모든 Task 완료 상태가 아니게 되면 README의 `[x] IMPLEMENT`를 `[ ] IMPLEMENT`로 되돌리고, 이력에 재검증 실패로 IMPLEMENT 상태를 되돌렸다는 한 줄을 추가한다.
- Per-Request mode는 문서나 체크박스를 갱신하지 않는다.

## 테스트 검증 근거 규칙
- `verify`는 검증 근거 수집 목적으로만 테스트를 실행한다. 테스트, 운영 코드, 문서는 수정하지 않는다.
- 문서, 오타, 정적 설정 문구처럼 동작 변경이 없는 작업은 diff 확인만으로 승인할 수 있다.
- 상태 변경, 외부 I/O, 동시성, 새 경계를 포함하는 작업은 테스트 또는 명확한 실행 근거가 필요하다.
- 변경 범위에 기존 테스트가 있는데 실행하지 않았다면 제한 사항으로 보고한다.
- 같은 변경 안에 추가 또는 수정된 테스트는 통과만으로 검증 근거가 되지 않는다. 구현 diff와 함께 회귀 케이스를 실제로 다루는지 확인한다.
- assertion 완화나 케이스 삭제처럼 검증력을 낮춘 변경은 `correctness`로 reject한다.

## 완료 보고
- 출력 구조에 따라 승인/거절 판단을 먼저 말한다.
