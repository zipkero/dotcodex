---
name: verify
description: "Judge whether an implemented Task should be approved or rejected against SPEC and implement.md verification criteria. Use when the user asks to verify, validate, approve, reject, or make a pass/fail judgment on implemented work."
---

# Verify

## 목적
- 구현 결과가 `implement.md`의 검증 조건을 충족하는지 먼저 검증한다.
- `spec.md`와 `analysis.md`는 상위 요구사항, 설계 제약, 범위 확인 기준으로 사용한다.
- `verify`는 승인/거절 판단과 근거를 사용자에게 보고한다.
- 별도 `verify.md`를 만들거나 갱신하지 않는다.
- 공통 안전 기준은 `AGENTS.md`의 언어, 판단과 변경 원칙, 도구와 안전을 따른다.

## 선행 확인
- `implement.md`에서 대상 Task의 목적, 근거 문서, 검증 조건을 확인한다.
- `spec.md`와 `analysis.md`를 읽고 대상 Task가 상위 요구사항과 설계 제약을 벗어나지 않는지 확인한다.
- 사용자가 `TASK-NNN` 형식의 Task ID를 지정하면 해당 Task를 검증 대상으로 한다.
- Task ID가 지정되지 않으면 현재 변경 내용, `implement.md` 상태, 대화 근거로 하나의 Task가 명확할 때만 검증 대상으로 추론한다.
- 대상 Task가 없거나 둘 이상 가능하거나 실제 산출물만으로 특정할 수 없으면 판단하지 말고 사용자에게 `TASK-NNN` 지정을 요청한다.
- 검증은 명령 실행, 코드 확인, 산출물 확인 같은 실제 근거를 기반으로 한다.

## 근거 원칙
- 판단은 대화 기억이나 구현 의도가 아니라 실제 산출물에 근거한다.
- 근거만으로 정확성을 판단할 수 없으면 `rejected`로 판단하고 부족한 근거를 명시한다.

## 판단 규칙
- 1차 기준은 대상 Task의 `검증 조건`이다.
- 2차 기준은 `SPEC 완료 조건`을 위반하지 않는지 여부이다.
- 3차 기준은 `analysis.md`의 설계 결정, 리스크, 제외 범위를 벗어나지 않는지 여부이다.
- `approved`: Task가 `implement.md` 검증 조건을 충족하고, `spec.md`와 `analysis.md`의 상위 기준을 위반하지 않는다고 근거로 판단할 수 있다.
- `rejected`: 충족하지 못하거나, 근거가 부족하거나, 범위나 설계 의도를 벗어난다.
- 문서 매핑이 맞는지만으로 승인하지 않는다.
- 실행하지 못한 검증은 성공으로 간주하지 않는다.

## 상태 전환
- 검증 단계는 먼저 `approved` 또는 `rejected` 판단과 근거를 확정한다.
- `approved`인 경우에만 그 다음 작업으로 해당 `implement.md` Task 하나를 `[x]`로 변경한다.
- 모든 Task가 `[x]`가 되면 `docs/<feature-dir>/README.md`의 `IMPLEMENT`를 `[x]`로 변경하고 `- <yyyy-MM-dd>: IMPLEMENT 완료` 이력을 추가한다.
- `rejected`이면 대상 Task를 `[ ]`로 유지한다. 이미 승인된 Task를 재검증해 실패한 경우에만 `[x]`를 `[ ]`로 되돌린다.

## 테스트 규칙
- 테스트 관련 판단 기준은 이 skill이 소유한다. `verify`는 테스트 필요성, 실행 근거의 충분성, 승인/거절 여부를 판단하며, 테스트 코드를 직접 작성하지 않는다.
- 테스트는 기본 검증 방법이 아니라, 리스크가 있거나 기존 테스트 체계가 있는 경우의 검증 방법이다.
- `analysis.md`의 리스크나 데이터 흐름이 상태 변경, 외부 I/O, 동시성, 새 경계를 포함하면 `implement.md`에 명시적 테스트 Task가 있는지 확인한다.
- `implement`는 명시적 테스트 Task가 있거나 버그 수정의 단일 회귀 테스트 예외에 해당할 때만 테스트 코드를 작성한다.
- 테스트 작성이 필요한 경우 일반 구현 Task의 부가 조건으로 숨기지 않고 별도 테스트 Task로 둔다.
- Per-Request 성격의 작은 변경에서는 테스트를 조용히 추가하지 않는다. 회귀 리스크가 있으면 사용자에게 테스트 공백으로 보고한다.
- 최소 근거는 코드 diff이다. 가능하면 테스트 실행 결과를 함께 사용한다.
- 문서, 오타, 정적 설정 문구처럼 동작 변경이 없는 작업은 diff 확인만으로 승인할 수 있다.
- 상태 변경, 외부 I/O, 동시성, 새 경계를 포함하는 작업은 테스트 또는 명확한 실행 근거가 필요하다.
- 변경 범위에 기존 테스트가 있는데 실행하지 않았다면 제한 사항으로 보고한다.
- 같은 변경에서 테스트도 수정했다면 그 테스트만으로 승인하지 않는다. assertion 완화나 케이스 삭제처럼 검증력을 낮춘 변경은 rejected로 판단한다.

## 완료 보고
- 승인/거절 판단을 먼저 말한다.
- 대상 Task, 주요 근거, 실패 사유, 남은 리스크를 간단히 정리한다.
