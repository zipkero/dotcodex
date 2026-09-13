---
name: implement
description: "Implement one approved Phased Task or a scoped Per-Request change."
---

# Implement

## 컨텍스트 로딩
- 이 문서는 main의 진입점이다. 구현 worker는 `~/.codex/skills/implement/references/worker.md`를 직접 읽고 적용한다.
  main도 위임 경계와 반환 검토에 이 계약을 사용한다.
- `Phased` 작업은 `~/.codex/skills/implement/references/phased.md`의 진입 조건과 Task 선택·문서 처리 기준을 추가로 적용한다.
  기능 경로만 언급한 분석·설명·검토 요청을 구현으로 바꾸지 않는다.
- `Per-Request`에서는 Phased 조정 절차를 적용하지 않고 `features/<feature-dir>/`를 만들지 않는다.
  조사로 해소할 불확실성은 먼저 확인하고, 의도·산출물·범위·성공 기준을 바꾸는 미확정 판단이 남으면 수정 전에 질문한다.

## worker 호출 계약
- main은 승인된 Phased Task 하나 또는 범위가 확정된 Per-Request 요청 하나를 built-in `worker`에 맡긴다.
- worker 호출에는 `model = "gpt-5.6-sol"`, `reasoning_effort = "medium"`과
  `fork_turns = "none"` 또는 필요한 최소 최근 turn 수인 양의 정수 문자열을 명시하며, `fork_turns`를 생략하거나 `"all"`을 사용하지 않는다.
- 호출 메시지는 이전 대화 없이 실행할 수 있도록 목적·접근·검증 조건, 수정 범위, 승인된 기준 문서,
  `~/.codex/skills/implement/references/worker.md`와 프로젝트 `AGENTS.md`의 실제 절대 경로를 포함한다.
  코드 변경이면 `~/.codex/docs/languages.md`와 해당 언어 문서의 실제 절대 경로도 전달한다.
  worker가 지정된 원본·지침 파일을 직접 읽어 적용하도록 명시한다.
- 필요한 모델·추론 수준·이력 범위를 적용할 수 없거나 worker 호출에 실패하면 Task와 문서 상태를 유지한 채 오류와 영향을 보고하며,
  다른 모델·추론 수준·전체 이력 호출이나 main의 직접 구현으로 대체하지 않는다.

## 완료 보고
- main은 worker의 반환을 검토하고 구현 결과, 변경 파일, 실행한 검증, 남은 위험과 범위 밖 발견을 보고한다.
- Phased 문서 반영은 `~/.codex/skills/implement/references/phased.md`를 따른다.
- `completed`는 구현 반환이며 승인 판정이 아니다. main은 요청 범위의 `verify` 또는 `implement-loop`로 이어간다.
  구현 단계만 요청받았거나 정지 조건에 걸려 종료하면 남은 승인 검증과 정지 근거를 알린다.
