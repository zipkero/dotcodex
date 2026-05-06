---
name: implement
description: "Execute the next Task from docs/<feature-dir>/implement.md. Use when the user asks to implement, fix, build, or modify code for an active documented feature scope."
---

# Implement

## 목적
- feature 문서 디렉터리의 `implement.md`에 있는 지정된 Task 또는 첫 번째 미완료 Task를 최소 범위로 구현한다.
- 공통 안전 기준은 `AGENTS.md`의 불변 규칙을 따른다.

## 선행 확인
- `Phased(문서 우선)` 대상 작업이면 `spec.md`, `analysis.md`, `implement.md`를 확인한다.
- 문서가 없거나 활성 feature scope를 특정할 수 없으면 구현하지 말고 필요한 init 단계나 범위를 요청한다.
- 사용자가 `TASK-NNN`을 지정하면 해당 Task, 아니면 `implement.md`의 첫 번째 미완료 Task를 대상으로 한다.
- 대상 Task가 없거나, 이미 완료되었거나, 모호하면 구현하지 않고 명확화가 필요하다고 보고한다.
- `AGENTS.md` 기준으로 `Per-Request`에 해당하는 작은 변경은 활성 feature scope가 없어도 사용자의 명시 범위 안에서 처리할 수 있다.
- 단, 범위 확장, 새 의존성, 임의 리팩터링, 설계 판단이 필요한 변경은 문서 플로우를 따른다.

## 구현 규칙
- 기존 코드 패턴과 프로젝트 관례를 우선한다.
- 요청 범위 밖 리팩터링, 설계 변경, 새 의존성 추가를 피한다.
- 설계 변경이 필요하면 먼저 `analysis.md` 갱신이 필요하다고 보고한다.
- 파일 수정 전 어떤 변경을 할지 짧게 설명한다.
- 구현은 대상 Task 단위로 진행하고, 한 턴에 하나의 Task만 구현한다.
- Task 밖에서 발견한 문제는 수정하지 말고 보고만 한다.
- 테스트 코드는 `implement.md`에 테스트 Task가 있거나, `verify` skill의 테스트 규칙에서 허용하는 버그 회귀 예외에 해당할 때만 작성한다.
- 테스트, 포맷, 빌드 명령은 변경 범위를 확인하는 데 필요한 수준으로 실행한다.
- 검증 단계에서 `approved`로 판단하기 전에는 `implement.md` 체크박스와 `docs/<feature-dir>/README.md`의 `IMPLEMENT` 상태를 변경하지 않는다.

## 완료 보고
- 변경한 파일
- 실행한 Task
- 실행한 확인
- 남은 항목 또는 리스크
- 다음 단계로 해당 Task 검증이 필요함을 알린다.
