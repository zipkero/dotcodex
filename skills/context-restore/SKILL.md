---
name: context-restore
description: >-
  Restore architecture, design, or delivery context from a project-root CONTEXT.md, verify it against linked source and
  Phased documents, and continue from its next action when requested. Use when resuming work in a new Codex session,
  picking up saved context, revisiting an unexpected design change, or asking what to do next during spec, analysis,
  planning, or implementation work.
---

# Context Restore

## 목적

- 프로젝트 루트의 `CONTEXT.md`에서 설계 또는 delivery 작업의 현재 위치를 복원한다.
- 연결된 원본 문서를 확인해 오래되거나 어긋난 맥락을 그대로 이어가지 않는다.
- 사용자가 이어서 진행하라고 요청하면 저장된 다음 작업부터 계속한다.

## 적용 경계

- 복원만 요청받았으면 읽기 전용으로 동작하고 파일을 수정하지 않는다.
- Phased 문서가 있으면 Task 상태, 요구사항과 구현 계획은 해당 문서를 기준으로 복원한다.
- `CONTEXT.md`는 작업을 중단시킨 설계 변경, 현재 논점과 다음 행동을 보완한다.
- `문서 반영 필요`에 기록된 확정 사항은 원본 문서보다 나중에 결정됐을 수 있으므로 단순 충돌로 폐기하지 않는다.
- 프로젝트 루트를 하나로 결정할 수 없으면 사용자에게 대상 프로젝트를 확인한다.
- Git branch, commit, status, diff는 조사하지 않는다.

## 복원 절차

1. 프로젝트 루트의 `CONTEXT.md`를 전체 읽는다.
2. 파일이 없으면 저장된 맥락이 없다고 알리고 추정으로 재구성하지 않는다.
3. `현재 작업 문서`, `먼저 읽을 문서`와 `확정된 결정`에서 참조한 프로젝트 문서를 읽는다.
4. 참조 파일이 없거나 `CONTEXT.md`와 원본 또는 Phased 문서가 충돌하면 그 차이를 먼저 보고한다.
5. `문서 반영 필요`에 명시되지 않은 충돌은 현재 원본 문서를 우선한다.
6. `문서 반영 필요`에 명시된 충돌은 확정됐지만 아직 반영되지 않은 변경으로 보고 다음 작업에서 문서부터 정렬한다.
7. 현재 목표, 현재 상태, 현재 작업 문서, 확정된 결정, 미확정 판단, 다음 작업과 완료 기준을 짧게 복원한다.
8. 사용자가 복원만 요청했으면 여기서 멈춘다.
9. 사용자가 이어서 진행하라고 명시했으면 다음 작업을 현재 요청 범위로 삼아 적절한 분석·문서·구현 흐름을 적용한다.

## 진행 규칙

- 다음 작업이 분석이나 설계이면 파일을 수정하지 않고 조사와 판단부터 수행한다.
- 다음 작업이 문서 작성이나 구현이면 사용자의 현재 요청이 변경을 허용하는지 확인한 뒤 수행한다.
- `CONTEXT.md`에 없는 새 설계 결정을 임의로 확정하지 않는다.
- 완료된 다음 작업을 자동으로 `CONTEXT.md`에 저장하지 않는다.
  사용자가 `$context-save`를 호출할 때만 인수인계 상태를 갱신한다.

## 복원 보고

다음 순서로 간결하게 보고한다.

1. 현재 목표
2. 확정된 결정
3. 미확정 판단
4. 바로 시작할 다음 작업과 완료 기준
5. 누락되거나 원본 문서와 충돌한 맥락

이어 진행하는 요청이면 복원 보고 후 같은 턴에서 다음 작업을 시작한다.
