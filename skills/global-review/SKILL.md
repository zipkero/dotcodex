---
name: global-review
description: "Audit global Codex configuration for rule consistency, ambiguity, duplication, README accuracy, and trimming opportunities."
---

# Global Review

## 목적
`AGENTS.md`, `README.md`, `skills/**`를 읽고 전역 설정의 정합성을 점검한다. 파일은 수정하지 않고 분석 결과만 보고한다.

## 범위
- 규칙 간 의미 충돌
- 모호한 표현
- 같은 규칙의 중복 정의
- 의미 손실 없이 줄일 수 있는 표현
- README의 실제 구조 불일치
- 현재 설정에 없는 기능이나 폐기된 설명

## 제외
- 요청 없는 자동 수정
- agents 또는 외부 플로우 설계 확장
- 선호나 일반론만 근거로 한 문구 변경 제안

## 출력
- 요약
- 우선순위별 발견 사항
- 위치
- 현재 문제
- 제안 방향
- 영향받는 파일
