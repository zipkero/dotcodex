---
name: spec-init
description: "Create or reset docs/<feature-dir>/spec.md and feature README.md for documentation-first work."
---

# Spec Init

## 목적
- feature 문서 디렉터리를 만들고 요구사항의 기준 문서를 작성한다.
- `spec.md`는 요구사항 레벨에서 범위, 목표, 제약, 제외 범위, 완료 조건을 고정하는 기준 문서이다.
- `docs/<feature-dir>/README.md`는 feature 문서 세트의 개요와 상태판이다.
- 설계, 데이터 흐름, 구현 순서, 체크리스트는 다루지 않는다.

## 입력 판단
- 사용자가 feature 이름을 명시하면 사용하고, 없지만 대화에서 명확하면 kebab-case로 추론하고 가정을 밝힌다.
- feature 이름이 불명확하면 문서 생성을 진행하지 말고 짧게 질문한다.
- feature 이름은 산출물의 디렉터리명일 뿐이며 요구사항 범위를 결정하는 근거로 쓰지 않는다.
- 요구사항의 출처나 정리 방식이 아니라 사용자가 요청했거나 입력 근거에서 확인되는
  최종 사용 가능 상태를 기준으로 spec 범위를 판단한다.
- 입력 문서에 나타난 구현 단위나 진행 순서를 그대로 spec 범위로 채택하지 않는다.
- 요구사항 범위가 불명확하면 문서를 만들기 전에 질문한다.

## 생성/갱신 규칙
- 새 feature 디렉터리는 `docs/<yyyyMMdd>-<nnn>-<feature-name>/` 형식을 사용한다.
- 날짜는 `spec-init` 시작일 기준이고, `nnn`은 해당 날짜의 feature 생성 순번이다.
- 같은 날짜의 기존 feature 디렉터리를 확인해 가장 큰 `nnn`의 다음 번호를 사용한다.
- 같은 날짜의 같은 `<feature-name>` 디렉터리가 이미 있으면 새 번호를 만들지 않고 기존 디렉터리를 재사용한다.
- 기존 하위 문서(`analysis.md`, `implement.md`)가 있으면 덮어쓰기 전에 사용자에게 확인한다.
- `spec.md`를 덮어쓰면 하위 문서가 무효가 될 수 있음을 사용자에게 알린다.
- 사용자의 요청 밖 목표, 설계, 구현 방식을 새 요구사항으로 추가하지 않는다.
- 최종 사용 가능 상태를 판단하되, 입력 근거에서 확인되지 않은 목표를 요구사항 범위에 추가하지 않는다.
- `spec.md`의 `범위`, `목표`, `제약`, `제외 범위`, `완료 조건`은 최종 사용 가능 상태를 기준으로 작성한다.
- `spec.md`는 독자가 요구사항과 완료 기준을 바로 판단할 수 있게 쓰고, 용어는 `AGENTS.md`의 문서 용어 선택을 따른다.
- `docs/<feature-dir>/README.md`의 `개요`는 feature의 목적과 배경을 1-3문장으로 요약하고,
  세부 요구사항이나 설계 판단은 반복하지 않는다.

## feature README.md 형식
```markdown
# <기능명>

## 개요
<feature의 목적과 배경을 1-3문장으로 요약>

## 상태
- [x] SPEC
- [ ] ANALYSIS
- [ ] IMPLEMENT

## 문서
- [spec.md](./spec.md)
- [analysis.md](./analysis.md) (ANALYSIS 단계에서 생성)
- [implement.md](./implement.md) (IMPLEMENT 단계에서 생성)

## 이력
- <yyyy-MM-dd>: SPEC 작성
```

## spec.md 형식
```markdown
# <기능명> 명세

## 범위

## 목표

## 제약

## 제외 범위

## 완료 조건
1. <관찰 가능한 완료 조건>
2. <관찰 가능한 완료 조건>
```

## 완료 조건 작성 기준
- 완료 조건은 feature가 완성되었음을 외부에서 확인할 수 있는 관찰 가능한 결과로 쓴다.
- UI는 화면과 상호작용, CLI는 stdout/stderr/exit code, API는 응답과 status code,
  library는 반환값과 예외, data pipeline은 파일·DB row·event,
  infra는 health endpoint·metric·log signal처럼 대상 유형에 맞는 관찰 지점을 사용한다.
- 완료 조건 번호는 영구 식별자다. 하위 문서는 `SPEC §5.N`으로 참조한다.
- 기존 완료 조건을 재배열, 삭제, 재번호하지 않는다. 새 조건은 마지막 번호 뒤에 추가한다.
- 내부 실행 절차를 완료 조건으로 쓰지 않는다. 내부 설계와 구현 순서는 `analysis.md` 또는 `implement.md`로 분리한다.

## 스킬 완료 조건
- `docs/<feature-dir>/README.md`와 `spec.md`가 생성 또는 갱신되어야 한다.
- `spec.md`에는 위 형식의 5개 섹션만 둔다.
- 각 `SPEC §5.N`은 관찰 가능한 결과여야 한다.
- `제외 범위`에는 의도적으로 하지 않을 변경을 적는다.

## 완료 보고
- 생성/갱신된 파일
- 문서 생성을 막은 미확정 요구사항이 있었다면 질문한 내용
