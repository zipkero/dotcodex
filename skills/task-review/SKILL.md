---
name: task-review
description: "Explain a completed documented implementation task, including code behavior, verification evidence, trade-offs, and remaining risks, without making an approval or rejection decision. Use when the user asks for a task review, implementation explanation, or review-style walkthrough."
---

# Task Review

## 목적

완료된 `implement.md` Task를 사람이 검토할 수 있는 형태로 설명한다. 이 skill은 구현이나 승인 판단을 하지 않는다.

## 범위

- 방금 완료했거나 사용자가 지정한 `implement.md` Task 하나를 대상으로 한다.
- `verify`처럼 SPEC 충족 여부를 승인 또는 반려하지 않는다.
- 새 코드 변경, 리팩터링, 문서 상태 변경을 수행하지 않는다.
- 정보가 부족하면 추측하지 말고 확인한 근거와 남은 불확실성을 구분한다.

## 작성 기준

설명은 파일 변경 목록보다 실제 코드 동작을 우선한다. 대상 Task의 변경 파일, 관련 interface, caller, test를 확인하고, 근거가 부족한 부분은 추측하지 않는다.

다음 관점을 포함한다.

- Task의 목표와 실제 구현 범위
- 변경 파일별 역할과 구성요소 간 연결 관계
- 주요 type, function, method의 역할, 입력, 출력, 상태 변화, 오류 경계
- 호출 순서와 데이터 흐름
- 테스트가 검증한 계약과 연결된 코드 경로
- 설계상 trade-off, 이번 Task에서 제외한 범위, 남은 리스크
- 실행한 검증 명령과 결과

## 출력 구조

사용자가 짧게 요청하지 않은 한 다음 섹션을 기본으로 작성한다.

- 요약
- 대상 Task와 변경 파일
- 주요 코드 상세
- 실행 흐름과 데이터 흐름
- 오류 처리와 경계 조건
- 테스트와 검증 근거
- 설계 trade-off
- 남은 리스크와 후속 범위

## 형식

- 한국어로 작성한다. 단, 코드 식별자, 파일명, 명령, config key는 원문을 유지한다.
- 관련 파일은 클릭 가능한 절대 경로 링크로 연결하고, 필요한 경우 line reference를 포함한다.
- 필요한 경우 짧은 코드 조각이나 실행 흐름 예시를 포함한다.
- “승인”, “반려”, “완료 조건 충족” 같은 verify 판단은 하지 않는다.
