---
name: context-restore
description: >-
  Restore architecture, design, or delivery context from a project-root CONTEXT.md and verify it against linked source
  and Phased documents. Read-only: report the recovered goal, current state, decisions, unresolved questions, next
  action, and conflicts, then stop without modifying files or executing the next action. Use when reviewing or resuming
  orientation from saved context in a new session.
---

# Context Restore

## 목적

- 프로젝트 루트의 `CONTEXT.md`에서 설계 또는 전달 작업의 현재 위치를 복원한다.
- 연결된 원본 문서를 확인해 오래되거나 어긋난 맥락을 그대로 이어가지 않는다.

## 적용 경계

- 항상 읽기 전용으로 동작하고 파일을 수정하지 않는다.
- Phased 문서가 있으면 Task 상태, 요구사항과 구현 계획은 해당 문서를 기준으로 복원한다.
- `CONTEXT.md`는 작업을 중단시킨 설계 변경, 현재 논점과 다음 작업을 보완한다.
- `문서 반영 필요`에 기록된 확정 사항은 원본 문서보다 나중에 결정됐을 수 있으므로 단순 충돌로 폐기하지 않는다.
- 프로젝트 루트를 하나로 결정할 수 없으면 사용자에게 대상 프로젝트를 확인한다.
- Git branch, commit, status, diff는 조사하지 않는다.

## 복원 절차

1. 프로젝트 루트의 `CONTEXT.md`를 전체 읽는다.
2. 파일이 없으면 저장된 맥락이 없다고 알리고 추정으로 재구성하지 않는다.
3. `현재 목표`, `현재 상태`, `다음 작업` 또는 완료 기준이 누락됐으면 누락된 내용을 기록하고 추정으로 채우지 않는다.
4. `현재 작업 문서`, `먼저 읽을 문서`와 `확정된 결정`에서 참조한 프로젝트 문서를 읽는다.
5. 참조 파일이 없거나 `CONTEXT.md`와 원본 또는 Phased 문서가 충돌하면 그 차이를 먼저 보고한다.
6. `문서 반영 필요`에 명시되지 않은 충돌은 현재 원본 문서를 기준으로 복원한다.
7. `문서 반영 필요` 항목은 아직 원본에 반영되지 않은 확정 사항으로 복원한다.
   원본과 명시적으로 충돌하면 양쪽 내용을 함께 보고하고 어느 쪽이 현재 결정인지 확정하지 않는다.
8. 현재 목표, 현재 상태, 현재 작업 문서, 확정된 결정, 미확정 판단, 다음 작업과 완료 기준을 짧게 복원한다.

## 복원 보고

다음 순서로 간결하게 보고한다.

1. 현재 목표
2. 현재 상태와 현재 작업 문서
3. 확정된 결정
4. 미확정 판단
5. 저장된 다음 작업과 완료 기준
6. 누락되거나 원본 문서와 충돌한 맥락

복원 보고 후 종료하며 저장된 다음 작업을 자동으로 수행하지 않는다.
