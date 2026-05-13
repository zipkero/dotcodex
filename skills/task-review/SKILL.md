---
name: task-review
description: "Explain a completed documented implementation task as a source-by-source and method-by-method walkthrough, focusing on implementation flow, method responsibilities, tests, trade-offs, and remaining risks without making an approval or rejection decision. Use when the user asks for a task review, implementation explanation, code walkthrough, or detailed code-based review."
---

# Task Review

## 목적

완료된 `implement.md` Task 하나를 코드 흐름 중심으로 설명한다. 승인, 반려, SPEC 충족 여부 판단은 하지 않고, 구현된 코드가 어떤 소스와 메소드 흐름으로 동작하는지 설명한다.

## 범위

- 사용자가 지정한 `implement.md` Task 하나를 대상으로 한다.
- 관련 구현 파일, 주요 타입, 주요 메소드, 테스트를 설명한다.
- 코드 변경, 리팩터링, 문서 상태 변경은 수행하지 않는다.
- 정보가 부족하면 추측하지 말고 확인한 근거와 불확실한 부분을 구분한다.

## 출력 형식

한국어로 작성하고, 반드시 다음 섹션을 사용한다. 섹션별 책임을 섞지 않는다.

```md
**Task N 리뷰**

Task의 목적과 구현 범위를 짧게 설명한다. 승인/반려 판단은 쓰지 않는다.

**전체 플로우**

진입점에서 결과가 만들어질 때까지의 흐름을 큰 단계로 설명한다.
세부 로직은 여기서 풀지 말고 전체 이동 경로만 보여준다.

**메소드별 설명**

구현 흐름을 메소드 단위로 상세히 설명한다.
각 메소드 제목에는 파일 링크를 한 번만 붙인다.

각 메소드는 다음 형식으로 설명한다.

- 책임: 전체 플로우에서 맡는 일
- 코드 흐름: 실제 처리 순서, 호출하는 메소드, 그 결과를 다음 단계에서 쓰는 방식
- 연결: 다음 메소드나 계층으로 넘기는 값
- 의미: 경계 분리나 설계상 중요한 이유

Task의 중심 메소드는 반드시 `코드 흐름`을 단계 목록으로 풀어 쓴다.
보조 메소드는 중심 메소드 흐름 안에서 어떤 역할로 호출되는지 설명한다.
파일 책임과 계층 경계는 관련 메소드 설명 안에서 필요한 만큼만 다룬다.

**테스트와 검증**

테스트가 보장하는 플로우, 경계 조건, 오류 처리를 설명한다.
검증 명령을 실행했다면 결과를 함께 적는다.

**Trade-off와 남은 리스크**

구현 중 선택한 방식, 현재 Task 범위에서 적절한 이유, 후속 Task나 추가 검증이 필요한 부분을 설명한다.
```

파일 링크와 라인 링크는 메소드 또는 테스트의 첫 언급에만 사용한다. 같은 파일 링크를 반복하지 않는다. 파일 요약을 별도 목록처럼 나열하지 말고 메소드 흐름 중심으로 설명한다. 라인 번호를 따라가는 설명, git working tree 상태, untracked 여부, 승인/반려 판단은 쓰지 않는다.
