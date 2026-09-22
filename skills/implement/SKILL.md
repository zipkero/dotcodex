---
name: implement
description: "Implement one approved Phased Task or a scoped Per-Request change."
---

# Implement

## 진입
- Per-Request는 범위가 확정된 변경 하나를 구현한다. Phased 절차나 기능 문서를 만들지 않는다.
- Phased는 `~/.codex/skills/implement/references/phased.md`의 진입·Task 선택·문서 처리 계약을 추가로 적용한다.

## worker 호출
- main은 승인된 Phased Task 하나 또는 범위가 확정된 Per-Request 하나를 worker에게 맡기고 직접 구현하지 않는다.
- 호출에는 목적·범위·성공 기준, 수정 범위, 승인된 기준 문서와 상태, 기존 diff·검증·미해결 문제, `~/.codex/skills/implement/references/worker.md`와 적용되는 프로젝트 `AGENTS.md`의 절대 경로를 전달한다. 프로젝트 `AGENTS.md`가 없으면 그 사실과 적용 기준을 전달한다.
- Phased의 확정 결정은 먼저 소유 문서에 반영하고 worker에게 승인된 원본을 전달한다.
- worker 호출은 `model = "gpt-6-sol"`, `reasoning_effort = "medium"`, `fork_turns = "none"` 또는 필요한 최소 양의 정수 turn 수를 사용한다.
- 지정한 호출 조건을 적용할 수 없거나 호출에 실패하면 상태를 유지하고 오류·영향을 보고하며 다른 agent나 main 구현으로 대체하지 않는다.

## 결과
- worker 반환의 변경과 검증 근거를 main이 검토한다. Phased의 Task 체크박스와 상태는 `verify` 승인 후에만 바꾼다.
- Phased는 `verify` 또는 `implement-loop`로 이어가고, Per-Request는 요청되었거나 독립 검증이 필요한 경우 `verify`로 이어간다.
- 결과·변경 파일·검증·남은 위험과 범위 밖 발견을 보고한다.
