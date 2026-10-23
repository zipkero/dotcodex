---
name: implement-init
description: "Draft or revise Phased implement.md Tasks and verification criteria from approved design.md."
---

# Implement Init

## 역할과 전제
- 승인된 `design.md`를 순서 있는 구현 Task와 검증 조건으로 나눈다.
- 기능 `README.md`의 `SPEC`과 `DESIGN`이 모두 `[x]`여야 한다. 충족하지 않으면 해당 소유 단계로 반환한다.
- 승인 유지·취소와 상태는 `~/.codex/docs/phased-state.md`를 따른다.

## main 조정
- main은 `analyzer`에게 `implement.md` 후보 작성을 맡긴다.
- 호출에는 이번 요청의 목적·변경 범위, feature dir, 기능 문서, 적용되는 프로젝트 `AGENTS.md`, 코드 조사 출발점과 이 skill·공통 상태 계약의 절대 경로를 필요한 범위에서 전달한다.
- main은 후보를 원본·승인 범위·작성 기준에 대조해 적용하거나 analyzer에게 수정을 요청한다.
- main은 적용된 계획이 §Task 규칙을 모두 충족하면 계획 작성을 완료하고 상태와 이력을 갱신한다. 부분 적용은 작성 중인 계획과 남은 작업을 인계한다.

## analyzer 반환
- 관련 참조는 `SPEC §5.N`·`DESIGN §X.Y`로 적는다.

## Task 규칙
- Task는 같은 확인으로 함께 판정할 수 있는 동작과 필요한 기반·연결 작업을 묶어, 한 번의 `verify`로 승인 또는 거절할 수 있는 결과 단위다. 의존 순서는 문서 항목 위치로 표현한다.
- ID는 `task-001`부터 3자리로 부여한다. 기존 ID를 재번호·재사용하지 않고 새 ID는 가장 큰 번호 뒤에 추가한다.
- 각 적용 중인 `SPEC §5.N`은 하나 이상의 Task에 매핑되어야 한다. 설계가 부족하면 관련 완료 조건과 필요한 결정을 `design-init`으로 반환한다.
- 기본 필드는 목적, 접근, 검증 조건과 참조다. `최근 reject`, `승인 근거`는 구현·검증 단계가 관리한다.
- 참조는 `SPEC §5.N / DESIGN §X.Y` 형식으로 적고, 같은 문서의 여러 번호는 쉼표로 나열한다.
- Task와 관련 원본만으로 새 worker가 착수하고, 기준을 충족한 구현이면 그대로 통과하는 `확인`으로 완료 여부를 판단할 수 있게 작성한다.
- spec.md §2의 품질·규모 수준은 관련 Task의 `결과`에, §3의 지정 검증 방법은 `확인`에 반영한다.
- `확인`은 목적의 동작을 직접 확인할 수 있는 방법으로 정한다. 실행 동작이 바뀌는 Task는 빌드·lint·diff만으로 끝내지 않는다.

## implement.md 형식
```markdown
# <기능명> 구현

## 체크리스트

- [ ] task-001: <작업 항목>
  - 목적: <완성할 결과>
  - 접근: <승인된 설계의 구현 접근>
  - 검증 조건:
    - 결과: <완료 후 상태>
    - 확인: <판정 방법과 근거>
  - 참조: SPEC §5.N / DESIGN §X.Y
```

## 완료 보고
- Task 수, 완료 조건 매핑과 검증 기준을 보고한다. 상위 문서 결정이 부족해 Task나 통과할 수 있는 `확인`을 정할 수 없으면 확정하지 않고 수정 소유 단계와 영향을 보고한다.
