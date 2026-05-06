---
name: spec-init
description: "Create or reset docs/<yyyyMMdd>-<nnn>-<feature-name>/spec.md and feature README.md for documentation-first feature work. Use when the user asks to initialize a spec, start a feature documentation flow, define requirements, or run spec-init."
---

# Spec Init

## 목적
- feature 문서 디렉터리를 만들고 요구사항의 기준 문서를 작성한다.
- `spec.md`는 요구사항, 범위, 완료 조건의 기준 문서이다.
- `docs/<feature-dir>/README.md`는 feature 문서 세트의 개요와 상태판이다.

## 입력 판단
- 사용자가 feature 이름을 명시하면 사용하고, 없지만 대화에서 명확하면 kebab-case로 추론하고 가정을 밝힌다.
- feature 이름이 불명확하면 문서 생성을 진행하지 말고 짧게 질문한다.
- 요구사항이 부족하면 임의로 채우지 말고 `열린 질문`에 남긴다.
- feature 이름은 산출물의 디렉터리명일 뿐이며 요구사항 범위를 결정하는 근거로 쓰지 않는다.
- 요구사항의 출처나 정리 방식이 아니라 사용자가 요청했거나 입력 근거에서 확인되는 최종 사용 가능 상태를 기준으로 spec 범위를 판단한다.
- 입력 문서에 나타난 구현 단위나 진행 순서를 그대로 spec 범위로 채택하지 않는다.

## 생성/갱신 규칙
- 새 feature 디렉터리는 `docs/<yyyyMMdd>-<nnn>-<feature-name>/` 형식을 사용한다.
- 날짜는 `spec-init` 시작일 기준이고, `nnn`은 해당 날짜의 feature 생성 순번이다.
- 같은 날짜의 기존 feature 디렉터리를 확인해 가장 큰 `nnn`의 다음 번호를 사용한다.
- 기존 하위 문서(`analysis.md`, `implement.md`)가 있으면 덮어쓰기 전에 사용자에게 확인한다.
- `spec.md`를 덮어쓰면 하위 문서가 무효가 될 수 있음을 사용자에게 알린다.
- 사용자의 요청 밖 목표, 설계, 구현 방식을 새 요구사항으로 추가하지 않는다.
- 최종 사용 가능 상태를 판단하되, 입력 근거에서 확인되지 않은 목표를 요구사항 범위에 추가하지 않는다.
- 요구사항 범위가 불명확하면 임의로 축소하거나 확장하지 말고 `열린 질문`에 남긴다.
- `spec.md`의 `범위`와 `SPEC 완료 조건`은 구현 순서가 아니라 최종적으로 사용자가 관찰할 수 있는 작동 상태를 기준으로 작성한다.
- 구현 단위와 진행 순서는 spec의 요구사항이 아니라 후속 문서에서 다룰 계획 정보로 분리한다.
- `docs/<feature-dir>/README.md`의 `개요`는 feature의 목적과 배경을 1-3문장으로 요약하고, 세부 요구사항이나 설계 판단은 반복하지 않는다.

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
- [analysis.md](./analysis.md)
- [implement.md](./implement.md)

## 이력
- <yyyy-MM-dd>: SPEC 작성
```

## spec.md 형식
```markdown
# <기능명> Spec

## 범위

## 목표

## 제약

## 비범위

## 완료 조건
1. <관찰 가능한 완료 조건>
2. <관찰 가능한 완료 조건>

## 열린 질문
```

## 스킬 완료 조건
- `docs/<feature-dir>/README.md`와 `spec.md`가 생성 또는 갱신되어야 한다.
- `spec.md`의 `완료 조건`은 번호 목록으로 작성한다. 하위 문서는 `SPEC 완료 조건 N` 형식으로 이 번호를 참조한다.
- 각 `SPEC 완료 조건`은 사용자가 보거나 명령/테스트/API 응답/파일/DB row/로그 등으로 확인할 수 있는 관찰 가능한 결과여야 한다.
- `비범위`에는 의도적으로 하지 않을 변경을 적고, 설계/구현 순서/체크리스트는 `analysis.md` 또는 `implement.md`에 둔다.

## 완료 보고
- 생성/갱신된 파일
- 남은 열린 질문
