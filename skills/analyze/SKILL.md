---
name: analyze
description: "Investigate causes and impacts, evaluate architecture and tradeoffs, or propose designs without editing files. Use when the requested outcome is diagnosis, analysis, a decision, or a design recommendation; not when the primary outcome is an explanation of how existing code, changes, or systems work."
---

# Analyze

## 역할
- 파일을 변경하지 않고 원인·영향·구조·대안을 조사한다. Phased 설계 문서는 `design-init`이 작성한다.
- 기존 동작의 이해가 목적이면 `explain`, 구현이나 정식 판정이 목적이면 해당 skill로 넘긴다.

## 입력과 실행
- 사용자가 지정한 기능 문서, 파일, 심볼, 에러, 로그, diff와 관련 원본을 확인한다.
- 대상 범위를 정할 수 없으면 필요한 입력을 요청한다. 사용자 결정 없이 조사로 해소할 수 있는 불확실성은 읽기 전용으로 확인한다.
- 구조나 대안 판단을 요청받으면 선택지의 영향과 권장안을 제시한다.

## 출력
- 결론, 확인 근거, 영향 범위와 남은 사용자 결정을 보고한다.
- 구현 흐름 선택이 필요할 때만 `Phased 권장`, `Per-Request 가능`, `추가 입력 필요`와 다음 단계를 적는다.
