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

## 작성 기준

단순 변경 요약이나 평가표가 아니라, 코드를 처음 읽는 사람이 구현을 따라갈 수 있는 코드별 상세 해설로 작성한다. 대상 Task의 변경 파일, 관련 interface, caller, test를 확인하고, 근거가 부족한 부분은 추측하지 않는다.

- 먼저 전체 구조와 기존 interface 또는 caller와의 관계를 설명한다.
- 변경 파일별 역할을 설명한 뒤, 핵심 파일은 type, function, method 단위로 나누어 설명한다.
- 각 함수 설명은 쉬운 말로 입력, 처리 순서, 주요 분기, 오류 처리, 반환값을 코드 흐름대로 풀어쓴다.
- 테스트는 테스트 이름 나열이 아니라 어떤 동작을 어떤 조건에서 확인하는지 설명한다.
- 저장, 조회, 검색, 실행 같은 주요 흐름은 필요한 경우 짧은 text flow로 정리한다.
- 설계 선택, 이번 Task에서 제외한 범위, 남은 리스크는 코드에서 확인되는 근거와 연결해 설명한다.

## 출력 구조

사용자가 짧게 요청하지 않은 한 다음 섹션을 기본으로 작성한다.

- 요약
- 전체 구조
- 변경 파일별 역할
- 코드별 상세 분석
- 테스트가 확인하는 동작
- 한 번에 보는 실행 흐름
- 설계 선택과 남은 리스크

## 형식

- 한국어로 작성한다. 단, 코드 식별자, 파일명, 명령, config key는 원문을 유지한다.
- 관련 파일은 클릭 가능한 절대 경로 링크로 연결하고, 필요한 경우 line reference를 포함한다.
- 필요한 경우 짧은 코드 조각이나 실행 흐름 예시를 포함한다.
- “승인”, “반려”, “완료 조건 충족” 같은 verify 판단은 하지 않는다.
