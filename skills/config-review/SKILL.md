---
name: config-review
description: "Audit Codex configuration and instruction consistency without edits; not for simple lookups."
---

# Config Review

## 역할과 범위
- Codex 설정을 읽기 전용으로 감사해 역할·호출·참조·상태 계약의 충돌, 누락과 중복을 찾는다.
- 특정 파일 감사는 대상 원본과 `README.md`의 관련 연동 설명을, 전역 감사는 추적 중인 `AGENTS.md`, `README.md`, agent, 사용자 정의 skill·참조와 설정 구조를 확인한다.
- 실제 관리 범위는 Git 추적 결과로 확인한다.

## 기준
- 모든 감사에 `~/.codex/skills/config-review/references/structure.md`를 적용한다.
- 사용자 정책과 운영 계약을 기준으로 판단하고, 발견마다 원본 근거를 제시한다. 동작 문제는 실행 근거가 있을 때만 보고한다.
- 반복되는 준수 요구는 중복·과잉으로 검토하고, 기존 지침의 조건·구조·표현을 다듬는 방향으로 제안한다.

## 출력
- 확인 범위와 전체 판정: `정상`, `과함`, `부족`, `충돌`
- 발견 위치, 행동 영향, 제안 방향과 영향받는 파일
- 발견이 없으면 남은 검증 한계
