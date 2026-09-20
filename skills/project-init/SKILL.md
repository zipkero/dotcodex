---
name: project-init
description: "Initialize or restructure project README, ROADMAP, and supporting product or design docs."
---

# Project Init

## 역할
- 프로젝트 루트의 `README.md`와 `ROADMAP.md`를 만들거나 재구성한다.
- 확정된 사용자 관점 상세는 필요한 경우 `docs/product.md`, 프로젝트 수준 설계 상세는 `docs/design.md`에 둔다.
- 특정 기능 요구사항은 작성하지 않고 `spec-init`으로 넘긴다.

## 입력과 판단
- 대상 프로젝트 루트, 기존 프로젝트 문서, 주요 manifest와 사용자 입력을 확인한다.
- 최종 결과물·대상 사용자·프로젝트 완료 기준·포함 범위가 결과를 바꿀 정도로 미확정이면 main이 사용자에게 확인한다.
- 기존 정보는 책임 문서에 보존하고, 현재 요청이 승인하지 않은 기준 변경은 적용하지 않는다.

## 산출물
- `README.md`: 현재 프로젝트의 역할, 제공 범위와 시작 경로
- `ROADMAP.md`: 최종 결과물, 프로젝트 완료 기준, 포함 범위, 검증 가능한 마일스톤과 의존 관계, 필요한 후속 기능 후보
- `docs/product.md`: README와 ROADMAP에 담기 어려운 확정된 사용자 흐름·정책이 있을 때만 생성 또는 갱신
- `docs/design.md`: 확정된 프로젝트 수준 구조·경계·상태 흐름·인터페이스가 있을 때만 생성 또는 갱신

`README.md`와 `ROADMAP.md`는 항상 존재해야 한다. 선택 문서는 필요한 상세가 있을 때만 만들고, 같은 상세를 여러 문서에 반복하지 않는다. 후속 기능 후보는 kebab-case로 적되 `features/`는 만들지 않는다.

## 완료
- 생성·갱신하거나 변경 없이 확인한 파일과 각 문서의 책임을 보고한다.
- 다음 세션이 문서만으로 프로젝트 목표, 현재 상태와 후속 기능 경계를 복원할 수 있어야 한다.
