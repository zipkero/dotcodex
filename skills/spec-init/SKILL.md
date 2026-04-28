---
name: spec-init
description: "Create or reset docs/<yyyy-MM-dd>-<feature-name>/spec.md and README.md for documentation-first feature work. Use when the user asks to initialize a spec, start a feature documentation flow, define requirements, or run spec-init."
---

# Spec Init

## 목적
- feature 문서 디렉터리를 만들고 요구사항의 기준 문서를 작성한다.
- `spec.md`는 요구사항, 범위, 완료 조건의 기준 문서이다.
- `README.md`는 feature 문서 세트의 상태판이다.

## 입력 판단
- 사용자가 feature 이름이나 경로를 명시하면 그대로 사용한다.
- feature 이름이 없지만 대화에서 명확하면 kebab-case로 추론하고 가정을 밝힌다.
- feature 이름이 불명확하면 구현하지 말고 짧게 질문한다.
- 요구사항이 부족하면 임의로 채우지 말고 `열린 질문`에 남긴다.

## 생성/갱신 규칙
- 새 feature 디렉터리는 기본적으로 `docs/<yyyy-MM-dd>-<feature-name>/` 형식을 사용한다.
- 날짜는 `spec-init`을 시작한 날짜를 기준으로 한다.
- 사용자가 명시적으로 경로 또는 feature 이름을 지정하면 그 값을 우선한다.
- 같은 날짜에 이름이 충돌하면 `docs/<yyyy-MM-dd>-<nnn>-<feature-name>/` 형식을 사용한다.
- 기존 하위 문서(`analysis.md`, `implement.md`)가 있으면 덮어쓰기 전에 사용자에게 확인한다.
- `spec.md`를 덮어쓰면 하위 문서가 무효가 될 수 있음을 사용자에게 알린다.
- 사용자의 요청 밖 목표, 설계, 구현 방식을 새 요구사항으로 추가하지 않는다.

## README.md 형식
```markdown
# <기능명>

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

## 완료 조건
- `README.md`와 `spec.md`가 생성 또는 갱신되어야 한다.
- `spec.md`의 `완료 조건`은 번호 목록으로 작성한다. 하위 문서는 `SPEC 완료 조건 N` 형식으로 이 번호를 참조한다.
- 각 완료 조건은 사용자가 보거나 명령/테스트/API 응답/파일/DB row/로그 등으로 확인할 수 있는 관찰 가능한 결과여야 한다.
- `비범위`에는 이번 작업에서 의도적으로 하지 않을 변경을 명확히 적는다.
- `spec.md`에는 설계, 구현 순서, 체크리스트를 넣지 않는다. 해당 내용은 `analysis.md` 또는 `implement.md`에 둔다.

## 완료 보고
- 생성/갱신된 파일
- 남은 열린 질문
