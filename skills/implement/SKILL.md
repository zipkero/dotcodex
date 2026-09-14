---
name: implement
description: "Implement one approved Phased Task or a scoped Per-Request change."
---

# Implement

## 컨텍스트 로딩
- main은 구현 조정과 결과 검토에 `~/.codex/skills/implement/references/implementation.md`와
  `~/.codex/skills/implement/references/worker.md`를 적용한다.
- `Phased` 작업은 `~/.codex/skills/implement/references/phased.md`의 진입 조건과 Task 선택·문서 처리 기준을 추가로 적용한다.
  기능 경로만 언급한 분석·설명·검토 요청을 구현으로 바꾸지 않는다.
- `Per-Request`에서는 Phased 조정 절차를 적용하지 않고 `features/<feature-dir>/`를 만들지 않는다.
  불확실성은 조사하고 의도·산출물·범위·성공 기준을 바꾸는 판단만 수정 전에 질문한다.

## worker 호출 계약
- main은 승인된 Phased Task 하나 또는 범위가 확정된 Per-Request 요청 하나를 worker에게 맡기고 직접 구현하지 않는다.
- main은 위임에 필요한 목적·범위·완료 기준과 근거 위치를 확인하고, 구현을 위한 상세 조사·파일 수정·실행 검증·수정 반복은 worker에게 맡긴다.
- worker 호출에는 `model = "gpt-5.6-sol"`, `reasoning_effort = "medium"`과
  `fork_turns = "none"` 또는 필요한 최소 최근 turn 수인 양의 정수 문자열을 명시하고, 생략하거나 `"all"`을 사용하지 않는다.
- 호출 메시지는 이전 대화 없이 실행할 수 있도록 목적·접근·검증 조건, 수정 범위, 승인된 기준 문서,
  `~/.codex/skills/implement/references/implementation.md`, `~/.codex/skills/implement/references/worker.md`와
  프로젝트 `AGENTS.md`의 절대 경로를 포함한다. 코드 변경이면 언어 기준 경로도 전달하고 지정한 원본·지침을 직접 읽게 한다.
- 기존 변경이 있는 작업을 위임하면 현재 diff·확정된 결정·검증 결과·미해결 문제·남은 작업을 호출 입력에 포함한다.
  worker는 기존 변경을 보존하며, main은 구현 수정이 필요하면 worker에게 맡긴다.
- 지정한 모델·추론·이력 범위를 적용할 수 없거나 호출에 실패하면 상태·문서를 유지하고 오류·영향을 보고하며 다른 모델·추론 수준·이력·main 구현으로 대체하지 않는다.

## 완료 보고
- main은 worker가 반환한 변경과 검증 근거를 검토하고 결과·변경 파일·검증·남은 위험과 범위 밖 발견을 보고한다.
  별도 verify가 필요 없는 Per-Request는 핵심 근거 중심으로 간결하게 보고할 수 있다.
- Phased는 문서 반영 후 `verify`나 `implement-loop`로 이어간다. Per-Request는 사용자가 검증을 명시했거나 `verify` 조건에 해당할 때 이어간다.
  구현만 요청받았거나 정지 조건이면 남은 검증과 근거를 알린다.
