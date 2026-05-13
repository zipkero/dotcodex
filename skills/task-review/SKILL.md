---
name: task-review
description: "Explain a completed documented implementation task as a source-by-source and method-by-method walkthrough, focusing on implementation flow, method responsibilities, tests, trade-offs, and remaining risks without making an approval or rejection decision. Use when the user asks for a task review, implementation explanation, code walkthrough, or detailed code-based review."
---

# Task Review

## 목적

완료된 `implement.md` Task 하나를 코드 흐름 중심으로 설명한다.

이 skill은 승인 또는 반려 판단을 하지 않는다. `verify`처럼 SPEC 충족 여부를 판정하지 않고, 구현된 코드가 어떤 소스와 메소드 흐름으로 동작하는지 사용자가 이해할 수 있게 설명한다.

## 범위

- 사용자가 지정한 `implement.md` Task 하나를 대상으로 한다.
- 관련 소스 파일, 주요 타입, 주요 메소드, 테스트를 설명한다.
- 코드 변경, 리팩터링, 문서 상태 변경은 수행하지 않는다.
- 정보가 부족하면 추측하지 말고 확인한 근거와 불확실한 부분을 구분한다.

## 출력 형식

한국어로 작성한다.

권장 구조:

```md
**Task N 리뷰**

Task의 목적과 구현 범위를 짧게 설명한다.

**전체 플로우**

사용자 요청 또는 실행 진입점에서 시작해 주요 메소드와 계층을 거쳐 결과가 만들어지는 흐름을 단계로 설명한다.

**소스별 설명**

관련 구현 파일별로 이 파일이 전체 플로우에서 맡는 역할을 설명한다.
소스별 설명은 파일의 책임, 계층 경계, 다른 구현 파일과의 연결만 다룬다.
테스트 파일과 문서 파일은 여기서 길게 설명하지 말고 `테스트와 검증` 또는 필요한 경우 `문서 반영`에서 짧게 다룬다.

**주요 메소드 흐름**

핵심 메소드는 메소드별 walkthrough로 설명한다.
각 메소드는 다음 순서를 따른다.

- 책임: 이 메소드가 전체 플로우에서 맡는 일
- 입력: 사용하는 request 값, dependency, 전제 조건
- 처리: 실제 실행 순서
- 출력: 반환값, 오류, 다음 메소드로 넘기는 값
- 의미: 경계 분리나 설계상 중요한 이유

Task의 중심 메소드는 반드시 처리 흐름을 단계 목록으로 풀어 설명한다.
보조 메소드는 역할과 연결 지점만 짧게 설명한다.

**테스트와 검증**

테스트가 어떤 플로우, 경계 조건, 오류 처리를 보장하는지 설명한다.
검증 명령을 실행했다면 결과를 함께 적는다.

**Trade-off와 남은 리스크**

구현 중 선택한 방식, 그 선택이 현재 Task 범위에서 적절한 이유, 후속 Task나 추가 검증이 필요한 부분을 설명한다.
```

라인 번호를 따라가는 설명은 피한다. 파일 링크와 라인 링크는 근거가 필요한 첫 언급에만 사용하고, 같은 파일 링크를 반복하지 않는다. 같은 파일의 여러 메소드나 테스트를 설명할 때는 첫 파일 링크 뒤에 메소드명 또는 테스트 함수명만 사용한다. 구현 리뷰와 직접 관련 없는 git working tree 상태, untracked 여부, 승인/반려 판단은 쓰지 않는다.
