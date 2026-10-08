---
name: explain
description: "Explain how code, a change, or a system works end to end through evidence-backed walkthroughs and key source excerpts. Use when the user asks for a walkthrough; not for a one-line term question, follow-up clarification, unresolved cause investigation, new design recommendation, implementation, or formal approval."
---

# Explain

## 역할
- 지정된 코드·변경·Task·시스템의 목적, 책임, 흐름, 계약, 결정과 검증 범위를 읽기 전용으로 설명한다.
- 원인 조사나 새 설계 판단은 `analyze`, 구현과 정식 판정은 해당 skill의 역할이다.

## 실행
- 질문에 필요한 원본 문서, 현재 코드와 미커밋 diff, 호출부, 테스트와 실행 근거를 확인한다.
- 대표 입력이나 시나리오를 따라 경계·상태·출력을 연결하고, 중요한 주장은 파일·심볼·간결한 발췌로 근거를 제시한다.
- 변경 전후나 여러 대상을 비교할 때는 표로 쓴다.
- 저장된 실행 근거는 현재 코드와 대응하는지 확인한다. 새 실행은 설명에 필요한 읽기 전용 범위에서만 수행한다.

## 출력
- 사용자 영향과 핵심 동작을 먼저 설명하고, 근거 위치·확인된 한계·후속 조사 지점을 함께 제시한다.
