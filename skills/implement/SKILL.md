---
name: implement
description: "Implement one approved Phased Task or a scoped Per-Request change."
---

# Implement

## 진입
- Per-Request는 범위가 확정된 변경 하나를 구현한다. Phased 절차나 기능 문서를 만들지 않는다.
- Phased는 `~/.codex/skills/implement/references/phased.md`의 진입·Task 선택·문서 처리 계약을 추가로 적용한다.

## 구현과 worker 호출
- main은 승인된 Phased Task 하나를 worker에게 맡긴다. 범위가 확정된 Per-Request는 worker 계약의 §구현 판단과 인계에 따라 직접 구현하거나 worker에게 맡긴다.
- worker 호출에는 다음을 전달한다.
  - 목적·범위·성공 기준
  - 예상 수정 대상
  - 승인된 기준 문서와 상태
  - 기존 diff·검증·미해결 문제
  - `~/.codex/skills/implement/references/worker.md`와 적용되는 프로젝트 `AGENTS.md`의 절대 경로
- 프로젝트 `AGENTS.md`가 없으면 그 사실과 적용 기준을 전달한다.
- Phased의 확정 결정은 먼저 소유 문서에 반영하고 worker에게 승인된 원본을 전달한다.
- worker 호출은 `model = "gpt-6-sol"`, `reasoning_effort = "medium"`, `fork_turns = "none"`을 사용한다.

## 결과
- 구현 결과의 변경과 검증 근거를 main이 검토한다.
- Phased는 `verify` 또는 `implement-loop`로 이어가고, Per-Request는 요청되었거나 독립 검증이 필요한 경우 `verify`로 이어간다.
- 사용자 보고는 결과를 먼저 밝히고 구현 완료와 검증 여부·판정을 구분한다. 변경 파일 정보는 worker 계약의 `Changed files` 정의를 따르고, 검증은 핵심 근거와 미실행 범위, 남은 위험과 범위 밖 발견을 요약한다.
