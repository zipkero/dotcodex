---
name: spec-init
description: "Create or reset docs/<feature-name>/spec.md and README.md for documentation-first feature work. Use when the user asks to initialize a spec, start a feature documentation flow, define requirements, or run spec-init."
---

# Spec Init

## 목적
- `docs/<feature-name>/` 범위를 만들고 요구사항의 기준 문서를 작성한다.
- `spec.md`는 무엇을 해결해야 하는지, 어디까지가 범위인지, 어떤 조건이면 완료로 볼 수 있는지에 대한 source of truth이다.
- `README.md`는 feature 문서 세트의 상태판이다.

## 입력 판단
- 사용자가 feature 이름을 명시하면 그대로 사용한다.
- feature 이름이 없지만 대화에서 명확하면 kebab-case로 추론하고 가정을 밝힌다.
- feature 이름이 불명확하면 구현하지 말고 짧게 질문한다.

## 생성/갱신 규칙
- 대상 경로는 `docs/<feature-name>/`이다.
- 기존 downstream 문서(`analysis.md`, `implement.md`)가 있으면 overwrite 전에 사용자에게 확인한다.
- `spec.md`를 덮어쓰면 downstream 문서가 무효가 될 수 있음을 사용자에게 알린다.
- 문서는 한국어, UTF-8, LF 기준으로 작성한다.

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

## 열린 질문
```

## 완료 조건
- `README.md`와 `spec.md`가 생성 또는 갱신되어야 한다.
- `spec.md`에는 설계, 구현 순서, 체크리스트를 넣지 않는다. 해당 내용은 `analysis.md` 또는 `implement.md`에 둔다.
- 응답에는 생성/갱신된 파일과 남은 열린 질문만 간단히 보고한다.
