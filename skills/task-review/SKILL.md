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

설명은 파일 변경 목록보다 다음 내용을 우선한다.

- Task의 목표와 실제 구현 범위
- 주요 코드 요소의 필요성, 역할과 책임
- 주요 함수, 타입, 설정의 입력과 출력
- 다른 구성요소와 연결되는 방식
- 테스트가 확인한 계약, 관찰 가능한 동작, 오류 경계, 회귀 위험
- 설계상 trade-off와 이번 Task에서 미룬 범위
- 실행한 검증 명령과 결과
- 남은 리스크 또는 후속 Task 범위

## 형식

- 한국어로 작성한다. 단, 코드 식별자, 파일명, 명령, config key는 원문을 유지한다.
- 관련 파일은 클릭 가능한 절대 경로 링크로 연결한다.
- 필요한 경우 짧은 코드 조각이나 실행 흐름 예시를 포함한다.
- “승인”, “반려”, “완료 조건 충족” 같은 verify 판단은 하지 않는다.
- 사용자가 짧게 요청하지 않은 한 단순 변경 요약으로 축소하지 않는다.
