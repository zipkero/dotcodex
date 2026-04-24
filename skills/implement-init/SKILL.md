---
name: implement-init
description: "Create or update docs/<feature-name>/implement.md from an existing analysis.md. Use when the user asks to create an implementation checklist, prepare implementation tasks, define verification criteria, or run implement-init."
---

# Implement Init

## 목적
- `analysis.md`를 실행 가능한 구현 체크리스트로 변환한다.
- `implement.md`는 구현 항목, 목적, 접근, 항목별 검증 기준의 기준 문서이다.
- 설계 판단은 `analysis.md`에 두고, `implement.md`에는 실행 가능한 Task만 둔다.

## 전제 조건
- `docs/<feature-name>/spec.md`와 `docs/<feature-name>/analysis.md`가 있어야 한다.
- 둘 중 하나가 없으면 필요한 선행 단계가 무엇인지 보고하고 중단한다.
- 기존 `implement.md`가 있으면 덮어쓰기 전에 사용자에게 확인한다. 기존 체크박스 상태는 덮어쓰기 시 사라질 수 있다.
- `analysis.md`에 미해결 Decision Point가 있으면 사용자에게 알리고, 사용자가 명시적으로 진행을 원할 때만 계속한다.

## 작성 규칙
- 체크리스트는 검증 가능한 단위로 나눈다.
- 각 Task는 한 번에 구현하고 검증할 수 있는 최소 단위로 작성한다.
- 각 항목은 최소 하나의 `spec.md` 완료 조건과 연결되어야 한다.
- 각 항목에는 목적, 대상, 접근, 검증 조건을 둔다.
- 대상 파일이나 모듈을 알 수 있으면 `대상`에 적는다.
- 설계 결정이 필요한 항목은 Task로 만들지 말고 `analysis.md`의 Decision Points로 되돌린다.
- `정리`, `개선`, `리팩터링`처럼 검증 기준이 불명확한 Task를 만들지 않는다.
- 테스트 Task 포함 기준은 `verify` skill의 테스트 규칙을 따른다. 여기서 별도의 테스트 규칙을 중복 정의하지 않는다.

## implement.md 형식
```markdown
# <기능명> Implement

## 체크리스트

- [ ] <작업 항목>
  - 목적: SPEC 완료 조건 N / ANALYSIS 섹션명
  - 대상:
  - 접근:
  - 검증 조건:

## 제외 항목
```

## README.md 갱신
- 이력에 `- <yyyy-MM-dd>: IMPLEMENT 체크리스트 작성`을 추가한다.
- 이 단계에서는 `IMPLEMENT`를 `[x]`로 변경하지 않는다. `IMPLEMENT`는 실제 구현 완료를 뜻한다.

## 완료 조건
- `implement.md`가 spec과 analysis에 근거해 생성 또는 갱신되어야 한다.

## 완료 보고
- 작업 항목 수
- 검증 기준 요약
