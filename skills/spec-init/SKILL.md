---
name: spec-init
description: "Create or reset a Phased feature spec.md and status README from confirmed requirements."
---

# Spec Init

## 역할과 입력
- 확정된 요구사항을 `features/<feature-dir>/spec.md`와 상태 `README.md`로 작성한다.
- 프로젝트 `README.md`, `ROADMAP.md`, 관련 `docs/product.md`, `docs/design.md`와 현재 원본을 필요한 범위에서 확인한다.
- 범위·목표·제약·제외 범위·완료 조건이 결과를 바꿀 정도로 미확정이면 문서를 확정하지 않고 main이 사용자에게 확인한다.

## 생성과 갱신
- 새 디렉터리는 `features/<yyyyMMdd>-<nnn>-<feature-name>/` 형식을 사용한다. 같은 날짜와 이름의 기존 디렉터리는 재사용한다.
- 기존 문서 갱신은 승인된 범위에서 수행하고, 승인 유지·취소와 상태는 `~/.codex/docs/phased-state.md`를 따른다.
- 사용자 결정은 해당 섹션에 반영하고, 채택되지 않은 제안은 요구사항으로 만들지 않는다.
- 기존 완료 조건 번호와 Task ID·순서·진행 기록을 보존한다.

## README.md 형식
```markdown
# <기능명>

## 개요
<기능의 목적과 배경>

## 상태
- [x] SPEC
- [ ] DESIGN
- [ ] IMPLEMENT

## 문서
- [spec.md](./spec.md)
- [design.md](./design.md) (DESIGN 단계에서 생성)
- [implement.md](./implement.md) (IMPLEMENT 단계에서 생성)

## 이력
- <yyyy-MM-dd>: SPEC 작성
```

## spec.md 형식
```markdown
# <기능명> 명세

## 1. 범위

### 1.1 입력 맥락
<다음 단계의 조사 출발점. 필요 없으면 생략>

## 2. 목표

## 3. 제약

## 4. 제외 범위

## 5. 완료 조건
1. <적용 조건과 기대 결과>
```

## 완료 조건과 상태
- 완료 조건 번호는 `SPEC §5.N` 영구 식별자다. 기존 번호를 재배열·재사용하지 않고 새 조건은 뒤에 추가한다.
- 더 이상 적용하지 않는 조건은 `[철회]` 또는 `[보류]`와 이유를 남긴다. 표시 없는 조건이 적용 중인 완료 조건이다.
- 확정된 요구사항과 일치할 때만 `SPEC`을 `[x]`로 두고, 하위 승인과 이력은 공통 상태 계약에 따라 갱신한다.

## 완료 보고
- 생성·갱신한 파일, 사용한 프로젝트 기준 문서와 승인 상태 변화를 보고한다.
