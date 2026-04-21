---
name: implement-init
description: "Create or update docs/<feature-name>/implement.md from an existing plan.md. Use when the user asks to create an implementation checklist, prepare implementation tasks, define verification criteria, or run implement-init."
---

# Implement Init

## 목적
- `plan.md`를 실행 가능한 구현 체크리스트로 변환한다.
- `implement.md`는 구현 항목, 완료 조건, 항목별 검증 기준의 source of truth이다.

## 전제 조건
- `docs/<feature-name>/spec.md`와 `docs/<feature-name>/plan.md`가 있어야 한다.
- 둘 중 하나가 없으면 필요한 선행 단계가 무엇인지 보고하고 중단한다.
- 기존 `verify.md`가 있으면 검증 기록과 충돌할 수 있으므로 overwrite 전에 사용자 확인을 받는다.

## 작성 규칙
- 체크리스트는 검증 가능한 단위로 나눈다.
- 각 항목에는 완료 조건과 검증 기준을 함께 둔다.
- 구현 중 체크박스는 증거가 있을 때만 `[x]`로 변경한다.
- 문서는 한국어로 작성한다.

## implement.md 형식
```markdown
# <기능명> Implement

## 체크리스트

- [ ] <작업 항목>
  - 완료 조건:
  - 검증 기준:

## 구현 메모

## 제외 항목
```

## README.md 갱신
- 이력에 `- <yyyy-MM-dd>: IMPLEMENT 체크리스트 작성`을 추가한다.
- 이 단계에서는 `IMPLEMENT`를 `[x]`로 변경하지 않는다.

## 완료 조건
- `implement.md`가 plan에 근거해 생성 또는 갱신되어야 한다.
- 응답에는 작업 항목 수와 검증 기준 요약만 간단히 보고한다.
