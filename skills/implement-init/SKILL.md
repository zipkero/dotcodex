---
name: implement-init
description: "Create or update docs/<feature-dir>/implement.md from an existing analysis.md. Use when the user asks to create an implementation checklist, prepare implementation tasks, define verification criteria, or run implement-init."
---

# Implement Init

## 목적
- `analysis.md`를 실행 가능한 구현 체크리스트로 변환한다.
- `implement.md`는 구현 항목, 실제 목적, 접근, 항목별 검증 조건의 기준 문서이다.
- 설계 판단은 `analysis.md`에 두고, `implement.md`에는 확정된 설계를 실행 가능한 Task로 나눈 내용만 둔다.
- 공통 안전 기준은 `AGENTS.md`의 불변 규칙을 따른다.

## 전제 조건
- feature 문서 디렉터리에 `spec.md`와 `analysis.md`가 있어야 한다.
- 둘 중 하나가 없으면 필요한 선행 단계가 무엇인지 보고하고 중단한다.
- 기존 `implement.md`가 있으면 덮어쓰기 전에 사용자에게 확인한다. 기존 체크박스 상태는 덮어쓰기 시 사라질 수 있다.
- `analysis.md`에 미해결 Decision Point가 있으면 사용자에게 알리고, 사용자가 명시적으로 진행을 원할 때만 계속한다.

## 작성 규칙
- 체크리스트는 검증 가능한 단위로 나눈다.
- 각 Task는 한 번에 구현하고 검증할 수 있는 최소 단위로 작성하되, 도메인 모델, 저장소 경계, 서비스 흐름, API/UI, 테스트처럼 검증 기준이 다른 작업은 필요하면 분리한다.
- Task ID는 `TASK-001`처럼 3자리 zero-padding을 사용하고, 신규 문서는 `TASK-001`부터 순서대로 작성한다.
- 기존 문서 갱신 시 Task ID는 재번호 매기지 않는다. 새 Task는 가장 큰 ID의 다음 번호를 사용하고, 삭제/병합된 ID는 재사용하지 않는다.
- 각 항목은 최소 하나의 `spec.md` 완료 조건을 근거 문서로 연결해야 한다.
- 각 항목에는 목적, 근거 문서, 대상, 접근, 검증 조건을 둔다.
- `목적`에는 문서 매핑이 아니라 사용자가 얻는 결과나 Task가 완성해야 하는 동작을 적는다.
- `근거 문서`에는 관련 `SPEC 완료 조건 N`과 `ANALYSIS`의 실제 섹션명을 함께 적어 검증자가 기준을 찾을 수 있게 한다.
- `접근`에는 `analysis.md`에서 확정된 설계를 구현하는 방법만 적고, 새로운 타입, 상태값, 저장소 경계, API 계약, 데이터 흐름을 새로 결정하지 않는다.
- `검증 조건`은 `결과`와 `확인`으로 작성한다. `결과`에는 Task 완료 후 성립해야 하는 동작, 출력, 파일 내용, 설정 상태를 적고, `확인`에는 테스트, 빌드, lint, diff, 수동 확인 등 해당 결과를 검증하는 방법을 적는다.
- 대상 파일이나 모듈을 알 수 있으면 `대상`에 적는다.
- 테스트 Task 포함 기준은 `verify` skill의 테스트 규칙을 따른다.

## 금지 사항
- Decision Point를 `implement.md`에 두지 않는다. 설계 결정은 `analysis.md`에 둔다.
- `spec.md` 완료 조건에 매핑되지 않은 Task를 만들지 않는다.
- `spec.md` 완료 조건을 `implement.md`에서 약화하거나 확장하지 않는다.
- `analysis.md`에 없는 설계 판단이 필요한 항목은 Task로 확정하지 말고 `analysis.md` 갱신 필요 사항으로 보고한다.
- `정리`, `개선`, `리팩터링`처럼 목적, 대상, 접근, 검증 조건이 불명확한 Task를 만들지 않는다.

## implement.md 형식
```markdown
# <기능명> Implement

## 체크리스트

- [ ] TASK-001: <작업 항목>
  - 목적:
  - 근거 문서: SPEC 완료 조건 N, ANALYSIS <섹션명>
  - 대상:
  - 접근:
  - 검증 조건:
    - 결과:
    - 확인:

## 제외 항목
```

## README.md 갱신
- 이력에 `- <yyyy-MM-dd>: IMPLEMENT 체크리스트 작성`을 추가한다.
- 이 단계에서는 `IMPLEMENT`를 `[x]`로 변경하지 않는다. `IMPLEMENT`는 실제 구현 완료를 뜻한다.

## 완료 조건
- `implement.md`가 위 형식과 작성 규칙에 맞게 생성 또는 갱신되어야 한다.

## 완료 보고
- 작업 항목 수
- 검증 기준 요약
