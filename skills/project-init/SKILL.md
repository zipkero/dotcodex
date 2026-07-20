---
name: project-init
description: "Create initial project-level README.md and ROADMAP.md for a new project's top-level scope, roadmap, and feature entry points."
---

# Project Init

## 목적
- 프로젝트 최초 생성 시 루트 `README.md`와 `ROADMAP.md`를 초기 생성한다.
- `README.md`는 프로젝트의 정체성, 목적, 현재 사용 방법, 문서 진입점을 소유한다.
- `ROADMAP.md`는 최종 결과물, 단계별 범위, 보류·제외 범위, feature 문서 후보를 소유한다.
- feature별 `spec.md`, `analysis.md`, `implement.md`는 만들지 않는다.

## 범위 판단
- 사용자가 새 프로젝트의 최초 문서 생성 또는 루트 README/ROADMAP 초기 작성을 요청하면 이 skill을 사용한다.
- 기능 추가, 공개 contract 변경, 여러 Task로 나눠야 하는 구현 작업은 `spec-init`부터 시작한다.
- 이미 `ROADMAP.md`가 있고 특정 feature만 구체화하려는 요청이면 `spec-init`이 필요하다고 보고한다.
- README 문구만 고치는 작고 명확한 변경은 Per-Request로 처리할 수 있다.

## 입력 판단
- 기존 `README.md`, `ROADMAP.md`, 주요 manifest, 기존 문서, 코드 구조를 먼저 확인한다.
- 최종 결과물, 핵심 사용자, 운영 조건, 포함 범위, 보류 범위가 여러 방향으로 해석되면 문서 작성 전에 질문한다.

## 작성 규칙
- `README.md`와 `ROADMAP.md`는 다음 세션에서 이전 대화 없이 읽어도 프로젝트 목표와 현재 기준을 복원할 수 있게 쓴다.
- 최종 결과물과 단계별 실행 범위를 분리하고, 초기 구현 가능 범위를 최종 목표로 축소해 쓰지 않는다.
- 기술 선택, 명령어, 현재 동작은 확인한 파일이나 실행 결과에 근거해서만 단정한다.
- 확인되지 않은 설치 방법, 실행 명령, 지원 기능, 성능 목표, 운영 보장은 추정으로 쓰지 않는다.
- `ROADMAP.md`의 단계는 구현 순서의 기준일 뿐이며, feature별 완료 조건과 검증 기준은 이후 `spec-init`에서 확정한다.
- 새 feature 문서 후보를 만들 수는 있지만 `features/<feature-dir>/` 디렉터리는 생성하지 않는다.
- 기존 `README.md`, `ROADMAP.md` 또는 feature 문서의 기준을 바꾸는 갱신은 영향 범위를 보고하고 사용자 확인을 받는다.

## README.md 기준
`README.md`에는 다음 정보를 프로젝트 성격에 맞게 간결하게 둔다.

- 프로젝트 이름과 한 줄 설명
- 해결하려는 문제와 대상 사용자
- 현재 상태
- 확인된 설치·실행·사용 방법
- 주요 문서 링크
- 라이선스나 운영상 주의가 확인된 경우 그 정보

세부 요구사항, 단계별 계획, 보류 항목의 판단 근거는 `ROADMAP.md`에 둔다.

## ROADMAP.md 기준
`ROADMAP.md`에는 다음 정보를 둔다.

- 최종 결과물: 사용자가 기대하는 완성 상태와 핵심 사용자 가치
- 포함 범위: 최종 결과물에 포함되는 주요 기능
- 단계별 범위: 초기 단계와 후속 단계의 범위, 다음 단계 착수 조건
- 보류·제외 범위: 제외 이유, 관련 위험과 다시 검토할 조건
- feature 문서 후보: 이후 `spec-init`으로 나눌 수 있는 feature 단위

## 산출물 형식
새 문서를 만들 때 기본 형식은 다음을 따른다. 일부 문서가 이미 있으면 의미 있는 구조를 보존하고 필요한 섹션만 작성한다.

```markdown
# <프로젝트명>

## 개요

## 현재 상태

## 사용 방법

## 문서
- [ROADMAP.md](./ROADMAP.md)
```

```markdown
# <프로젝트명> 로드맵

## 최종 결과물

## 포함 범위

## 단계별 범위

## 보류·제외 범위

## Feature 문서 후보
```

## 완료 보고
- 생성 또는 갱신한 파일
- 최종 결과물과 초기 범위의 구분
- 보류·제외 범위와 재검토 조건
- 다음 단계로 `spec-init`이 필요한 feature 후보
