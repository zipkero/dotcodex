---
name: analyze-init
description: "Create or update docs/<feature-dir>/analysis.md from an existing spec.md. Use when the user asks to create the analysis/design document for a documented feature scope."
---

# Analyze Init

## 목적
- `spec.md`를 근거로 분석과 설계 기준을 문서화한다.
- `analysis.md`는 구조, 데이터 흐름, 인터페이스, 영향 범위, 리스크, 검증 관점, 설계 결정의 기준 문서이다.
- `analysis.md`는 요구사항을 새로 만들지 않는다. 새 요구사항은 먼저 `spec.md`에 반영한다.

## 전제 조건
- feature 문서 디렉터리에 `spec.md`가 있어야 한다.
- spec이 없으면 `spec-init`이 필요하다고 보고하고 중단한다.
- 기존 `implement.md`가 있으면 하위 문서에 영향이 있으므로 덮어쓰기 전에 사용자에게 확인한다.

## 작성 규칙
- `spec.md`의 범위와 완료 조건을 기준으로 작성한다.
- 기존 코드, 문서, 명령 결과처럼 확인한 근거를 기준으로 작성한다.
- 확인하지 않은 구조나 동작은 추정으로 표시한다.
- 완료 조건을 복사해 반복하지 말고 필요한 곳에서 `SPEC 완료 조건 N`처럼 참조한다.
- 구현 순서, 작업 목록, 체크리스트는 작성하지 않는다. 해당 내용은 `implement.md`에 둔다.
- 인터페이스, 상태, 데이터 흐름, 저장 경계, 외부 연동처럼 구현 Task에 영향을 주는 설계 결정은 확인 근거와 함께 본문 섹션에 확정 내용으로 남긴다.
- 불확실하거나 합의되지 않은 설계 결정은 구현 계획처럼 쓰지 않고 Decision Points 또는 열린 질문으로 남긴다.
- 불필요한 리팩터링, 범위 외 개선, `spec.md`에 없는 요구사항은 설계 결정이나 구현 전제로 확정하지 않는다.

## analysis.md 형식
```markdown
# <기능명> Analysis

## 근거

## 구조

## 데이터 흐름

## 인터페이스

## 영향 범위

## 리스크

## 검증 관점

## Decision Points

## 열린 질문
```

## README.md 갱신
- `ANALYSIS`를 `[x]`로 변경한다.
- 이력에 `- <yyyy-MM-dd>: ANALYSIS 작성`을 추가한다.
- `IMPLEMENT`는 이 단계에서 체크하지 않는다.

## 완료 조건
- `analysis.md`가 spec에 근거해 생성 또는 갱신되어야 한다.

## 완료 보고
- 핵심 설계 결정
- 영향 범위
- 남은 질문
