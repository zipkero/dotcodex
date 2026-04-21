---
name: plan-init
description: "Create or update docs/<feature-name>/plan.md from an existing spec.md. Use when the user asks to create an implementation plan, design plan, technical plan, or run plan-init."
---

# Plan Init

## 목적
- `spec.md`를 근거로 구현 전 설계와 작업 방향을 문서화한다.
- `plan.md`는 설계 결정, 영향 범위, 변경 대상, 검증 전략의 source of truth이다.

## 전제 조건
- `docs/<feature-name>/spec.md`가 있어야 한다.
- spec이 없으면 먼저 `spec-init`이 필요하다고 보고하고 중단한다.
- 기존 `implement.md` 또는 `verify.md`가 있으면 downstream 영향이 있으므로 overwrite 전에 사용자 확인을 받는다.

## 작성 규칙
- `spec.md`의 요구사항과 수용 기준을 기준으로 작성한다.
- 불확실한 설계 결정은 열린 질문 또는 결정 필요 항목으로 남긴다.
- 문서는 한국어로 작성한다.
- 불필요한 리팩터링이나 범위 외 개선을 계획에 넣지 않는다.

## plan.md 형식
```markdown
# <기능명> Plan

## 요약

## 설계 결정

## 영향 범위

## 변경 대상

## 구현 전략

## 검증 전략

## 리스크와 대응

## 열린 질문
```

## README.md 갱신
- `PLAN`을 `[x]`로 변경한다.
- 이력에 `- <yyyy-MM-dd>: PLAN 작성`을 추가한다.
- `IMPLEMENT`와 `VERIFY`는 이 단계에서 체크하지 않는다.

## 완료 조건
- `plan.md`가 spec에 근거해 생성 또는 갱신되어야 한다.
- 응답에는 핵심 설계 결정, 변경 대상, 남은 질문만 간단히 보고한다.
