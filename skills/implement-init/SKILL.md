---
name: implement-init
description: "Draft or revise Phased implement.md Tasks and verification criteria from approved design.md."
---

# Implement Init

## 역할과 전제
- 승인된 `design.md`를 순서 있는 구현 Task와 검증 조건으로 나눈다.
- 기능 `README.md`의 `SPEC`과 `DESIGN`이 모두 `[x]`여야 한다. 충족하지 않으면 해당 소유 단계로 반환한다.
- 승인 유지·취소와 상태는 `~/.codex/docs/phased-state.md`를 따른다.

## analyzer 호출
- main은 `analyzer`에게 `implement.md` 후보 작성을 맡긴다.
- 호출에는 feature dir, 기능 문서, 적용되는 프로젝트 `AGENTS.md`, 코드 조사 출발점과 이 skill·공통 상태 계약의 절대 경로를 필요한 범위에서 전달한다.
- analyzer는 신규 문서는 전체 후보로, 기존 문서의 국소 의미 변경은 patch로 반환한다. 의미와 의존 관계를 국소 patch로 정확히 표현하기 어려우면 이유와 함께 전체 후보를 반환한다.
- 반환은 미커밋 변경까지 구별할 수 있는 원본 식별, patch의 적용 위치, 변경 이유와 관련 `SPEC §5.N`·`DESIGN §X.Y`, 직접·의존 영향과 판단하지 못한 영향을 포함한다. 입력이나 사용자 결정이 부족하면 근거와 영향을 반환한다.
- main은 원본·관련 상위 문서·승인 범위의 최신성을 확인한 뒤 후보를 적용한다. 불일치나 모호함이 있으면 현재 원본으로 analyzer를 다시 호출하며, main은 Task 의미를 작성하지 않는다.
- 일부만 적용한 경우 실제 부분 상태와 남은 작업을 인계하고 완료 처리하지 않는다. 적용 후 Task 순서·참조·완료 조건 매핑을 확인하고 main이 상태와 이력을 갱신한다.

## Task 규칙
- Task는 한 번의 `verify`로 승인 또는 거절할 수 있는 결과 단위다. 의존 순서는 문서 항목 위치로 표현한다.
- ID는 `task-001`부터 3자리로 부여한다. 기존 ID를 재번호·재사용하지 않고 새 ID는 가장 큰 번호 뒤에 추가한다.
- 각 적용 중인 `SPEC §5.N`은 하나 이상의 Task에 매핑되어야 한다. 설계가 부족하면 관련 완료 조건과 필요한 결정을 `design-init`으로 반환한다.
- 기본 필드는 목적, 접근, 검증 조건과 참조다. `시도`, `최근 reject`, `승인 근거`는 구현·검증 단계가 관리한다.
- Task와 관련 원본만으로 새 worker가 착수하고 완료 여부를 판단할 수 있게 작성한다.

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
  - 참조: SPEC §5.N, DESIGN §X.Y
```

## 완료 보고
- Task 수, 완료 조건 매핑과 검증 기준을 보고한다. 상위 문서 결정이 부족하면 수정 소유 단계와 영향을 보고한다.
