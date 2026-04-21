---
name: implement
description: "Implement documentation-first work from docs/<feature-name>/implement.md. Use when the user asks to implement, fix, build, or modify code for an active documented feature scope."
---

# Implement

## 목적
- `implement.md` 체크리스트를 기준으로 최소 범위의 코드를 변경한다.
- 구현 상태는 문서에 남아야 하며, 체크박스는 완료 증거가 있을 때만 갱신한다.

## 선행 확인
- 문서 우선 대상 작업이면 `spec.md`, `plan.md`, `implement.md`가 있는지 확인한다.
- 문서가 없으면 구현하지 말고 필요한 init 단계를 안내한다.
- 단순 오타나 아주 작은 요청은 사용자의 명시 범위 안에서 문서 플로우를 생략할 수 있다.

## 구현 규칙
- 기존 코드 패턴과 프로젝트 관례를 우선한다.
- 요청 범위 밖 리팩터링, 설계 변경, 새 의존성 추가를 피한다.
- 설계 변경이 필요하면 먼저 `plan.md` 갱신이 필요하다고 보고한다.
- 파일 수정 전 어떤 변경을 할지 짧게 설명한다.

## 문서 갱신
- 완료한 `implement.md` 항목만 `[x]`로 변경한다.
- 검증 명령이나 확인 결과가 있으면 해당 항목의 구현 메모에 간단히 남긴다.
- 모든 항목이 `[x]`가 되면 `README.md`의 `IMPLEMENT`를 `[x]`로 변경하고 이력에 `- <yyyy-MM-dd>: IMPLEMENT 완료`를 추가한다.
- `VERIFY`는 verify 단계 전에는 체크하지 않는다.

## 완료 보고
- 변경한 파일
- 완료한 체크리스트 항목
- 실행한 검증
- 남은 항목 또는 리스크
