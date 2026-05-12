---
name: task-review
description: "Explain a completed documented implementation task as a code-by-code walkthrough, including implementation flow, tests, trade-offs, and remaining risks, without making an approval or rejection decision. Use when the user asks for a task review, implementation explanation, code walkthrough, or detailed code-based review."
---

# Task Review

## 목적

완료된 `implement.md` Task를 사람이 검토할 수 있는 형태로 설명한다. 이 skill은 구현이나 승인 판단을 하지 않는다.

## 범위

- 방금 완료했거나 사용자가 지정한 `implement.md` Task 하나를 대상으로 한다.
- `verify`처럼 SPEC 충족 여부를 승인 또는 반려하지 않는다.
- 새 코드 변경, 리팩터링, 문서 상태 변경을 수행하지 않는다.
- 정보가 부족하면 추측하지 말고 확인한 근거와 남은 불확실성을 구분한다.

## 검토 기준

- Task의 목적과 구현 범위를 먼저 요약한다.
- 단순 변경 요약에 그치지 않고, 코드를 처음 읽는 사람이 이해할 수 있도록 코드별 역할과 의도를 설명한다.
- 변경된 파일별로 코드가 맡는 역할과 실행 흐름을 상세히 설명한다.
- 테스트나 검증 명령이 있으면 무엇을 확인했는지 설명하고, 없으면 확인하지 못한 범위를 밝힌다.
- 구현 중 선택한 trade-off와 대안을 구분한다.
- 남은 위험, 테스트 공백, 후속 확인이 필요한 부분을 확인된 근거와 함께 설명한다.

## 형식

- 한국어로 작성한다. 단, 코드 식별자, 파일명, 명령, config key는 원문을 유지한다.
- 관련 파일은 클릭 가능한 절대 경로 링크로 연결하고, 필요한 경우 line reference를 포함한다.
- 필요한 경우 짧은 코드 조각이나 실행 흐름 예시를 포함한다.
- “승인”, “반려”, “완료 조건 충족” 같은 verify 판단은 하지 않는다.
