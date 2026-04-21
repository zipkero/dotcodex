---
name: verify
description: "Verify documentation-first work and append evidence to docs/<feature-name>/verify.md. Use when the user asks to verify, review, validate, test, approve, reject, or record evidence for an implemented feature."
---

# Verify

## 목적
- 구현 결과를 spec, plan, implement 기준으로 검증한다.
- `verify.md`는 append-only 검증 증거 로그이다.

## 선행 확인
- `spec.md`, `plan.md`, `implement.md`를 읽고 검증 기준을 확인한다.
- 구현 체크리스트가 비어 있거나 완료되지 않았다면 그 상태를 보고한다.
- 검증은 실제 명령 실행, 코드 확인, 산출물 확인 같은 근거를 기반으로 한다.

## verify.md 규칙
- 기존 내용을 절대 덮어쓰지 않는다.
- 새 검증 시도는 파일 끝에 append한다.
- 실패 기록도 보존한다.

## 단계별 검증
- `implement.md`의 개별 체크리스트 항목 단위로 검증할 수 있다.
- 단계별 검증 결과는 `verify.md`에 append-only로 기록한다.
- 단계별 검증이 approved여도 전체 `README.md`의 `VERIFY`는 모든 구현 항목이 완료되고 필요한 검증이 승인된 뒤에만 `[x]`로 변경한다.

## 검증 항목 형식
```markdown
## <yyyy-MM-dd HH:mm> 검증

### 대상

### 실행한 확인

### 결과

### 판단
- 상태: approved | rejected
- 근거:

### 남은 리스크
```

## 승인 처리
- 검증이 승인되고 모든 `implement.md` 항목이 `[x]`이면 `README.md`의 `VERIFY`를 `[x]`로 변경한다.
- 이력에 `- <yyyy-MM-dd>: VERIFY 완료`를 추가한다.

## 거절 처리
- 실패 원인과 재현 또는 확인 근거를 `verify.md`에 기록한다.
- 실패로 인해 완료가 무효가 된 `implement.md` 항목은 `[ ]`로 되돌린다.
- `README.md`의 `IMPLEMENT` 또는 `VERIFY` 상태가 더 이상 유효하지 않으면 체크를 해제한다.

## 완료 보고
- 승인/거절 판단을 먼저 말한다.
- 주요 근거, 실패 항목, 남은 리스크를 간단히 정리한다.
