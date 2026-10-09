---
name: design-init
description: "Draft or revise a Phased feature design.md from approved spec.md."
---

# Design Init

## 역할과 전제
- 승인된 `spec.md`를 구현 Task 작성에 필요한 구조·흐름·인터페이스·영향과 설계 결정으로 구체화한다.
- 기능 `README.md`의 `SPEC`이 `[x]`여야 한다. 충족하지 않으면 `spec-init`으로 반환한다.
- 승인 유지·취소와 상태는 `~/.codex/docs/phased-state.md`를 따른다.

## main 조정
- main은 `analyzer`에게 `design.md` 후보 작성을 맡긴다.
- 호출에는 feature dir, `README.md`, `spec.md`, 기존 `design.md`·`implement.md`, 적용되는 프로젝트 `AGENTS.md`, 코드 조사 출발점과 이 skill·공통 상태 계약의 절대 경로를 필요한 범위에서 전달한다.
- main은 후보를 원본·승인 범위·작성 기준에 대조해 적용하거나 analyzer에게 수정을 요청한다.
- main은 적용된 전체 설계와 하위 영향을 확인하고 상태와 이력을 갱신한다. 부분 적용은 미완료 상태와 남은 작업을 인계한다.

## analyzer 반환
- patch 적용 위치, 변경 이유, 관련 `SPEC §5.N`, 직접·의존 영향과 확인하지 못한 영향을 포함한다.

## design.md 형식
```markdown
# <기능명> 설계

## 근거

## 1. 구조

## 2. 데이터 흐름

## 3. 인터페이스

## 4. 영향 범위

## 5. Decision Points
```

## 작성과 완료
- 적용 중인 각 `SPEC §5.N`을 관련 설계 본문에서 참조하고, spec과 관련 원본만으로 확인할 수 없는 설계 결정은 `Decision Points`에 둔다.
- spec §1 범위를 넘거나 요구사항·사용자 관찰 결과를 바꾸는 결정은 spec 소유 단계로 반환한다. 승인된 내부 결정은 보존하고 나머지 구현 방법은 Task와 worker의 재량으로 남긴다.
- spec, design과 관련 원본만으로 다음 단계가 Task를 작성할 수 있어야 한다.
- 핵심 결정, 영향 범위와 남은 결정이 있으면 완료 보고에 포함한다.
