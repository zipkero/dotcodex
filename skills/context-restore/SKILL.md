---
name: context-restore
description: "Restore work from project CONTEXT.md by checking its sources and current workspace, without edits."
---

# Context Restore

## 역할
- 프로젝트 루트의 `CONTEXT.md`를 원본 문서와 현재 작업 트리에 대조해 읽기 전용으로 복원한다.
- Phased의 요구사항·설계·Task 상태는 기능 문서를, Per-Request는 저장된 범위와 현재 작업 트리를 기준으로 한다.

## 절차
1. `CONTEXT.md` 전체를 읽는다. 없으면 저장된 맥락이 없다고 보고한다.
2. 현재 작업 문서, 확정된 결정, 문서 반영 필요, 필수·변경 파일을 읽는다. 조건부 파일은 조건이 해당할 때만 읽는다.
3. 현재 Git 상태와 실제 변경을 저장된 상태에 대조한다. 저장된 실행 근거가 현재 코드와 대응하지 않으면 재검증 필요로 보고한다.
4. 누락·불일치, 현재 목표·상태·결정, 다음 작업과 완료 기준, 문서 반영 필요를 보고한다.

## 경계
- 복원만 요청받으면 파일·상태를 변경하지 않는다.
- 다음 작업 실행도 요청받았으면 복원 후 `AGENTS.md`와 해당 skill로 진행한다.
- 원본 미반영 결정이 다음 작업의 선행 계약이면 의존 작업 전에 해당 문서 소유 단계에서 해소한다.
