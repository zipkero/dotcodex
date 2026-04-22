---
name: implement
description: "Execute the next Task from docs/<feature-name>/implement.md. Use when the user asks to implement, fix, build, or modify code for an active documented feature scope."
---

# Implement

## 목적
- `docs/<feature-name>/implement.md`의 첫 번째 미완료 Task를 최소 범위로 구현한다.
- 체크박스와 README 상태는 변경하지 않는다.

## 선행 확인
- 문서 우선 대상 작업이면 `spec.md`, `analysis.md`, `implement.md`를 확인한다.
- 문서가 없으면 구현하지 말고 필요한 init 단계를 안내한다.
- 단순 오타나 아주 작은 요청은 사용자의 명시 범위 안에서 문서 플로우를 생략할 수 있다.

## 구현 규칙
- 기존 코드 패턴과 프로젝트 관례를 우선한다.
- 요청 범위 밖 리팩터링, 설계 변경, 새 의존성 추가를 피한다.
- 설계 변경이 필요하면 먼저 `analysis.md` 갱신이 필요하다고 보고한다.
- 파일 수정 전 어떤 변경을 할지 짧게 설명한다.
- 구현은 `implement.md`의 첫 번째 미완료 Task 단위로 진행한다.
- 테스트 코드는 `implement.md`에 테스트 Task가 있거나, `verify` skill의 테스트 규칙에서 허용하는 버그 회귀 예외에 해당할 때만 작성한다.

## 완료 보고
- 변경한 파일
- 실행한 Task
- 실행한 확인
- 남은 항목 또는 리스크
- 다음 단계로 해당 Task 검증이 필요함을 알린다.
