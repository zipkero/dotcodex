---
name: verify
description: "Approve or reject an implementation against requirements, diff, and verification evidence."
---

# Verify

main은 직접 검증과 verifier 결과 검토에 `~/.codex/skills/verify/references/acceptance.md`를 읽어 적용한다.

## 대상
- Per-Request는 사용자가 검증을 요청했거나 독립 검증이 필요한 직전 구현 대상을 하나로 확정한다.
- Phased는 `~/.codex/skills/verify/references/phased.md`와 `~/.codex/docs/phased-state.md`의 입력·판정·상태 계약을 추가로 적용한다.
- 변경 범위는 사용자가 지정한 범위, 직전 worker의 `Changed files`, `CONTEXT.md` 인계, 현재 작업 트리 순서로 확인한다. 대상을 확정할 수 없으면 필요한 입력을 보고하고 판정을 보류한다.

## verifier 호출
- Phased 판정은 custom agent `verifier`를 사용한다.
- Per-Request는 독립 검증이 요청됐거나 권한·데이터·복구·상태 경계의 영향이 큰 변경에 `verifier`를 사용하고, 그 밖의 제한된 변경은 main이 직접 검증할 수 있다.
- 호출에는 대상과 범위, 기준 문서, 프로젝트 `AGENTS.md`, `~/.codex/skills/verify/references/acceptance.md`와 실행 근거를 전달한다. Phased일 때만 Phased 참조·공통 상태 계약과 기능 문서를 추가한다.

## 판정
- main은 acceptance 계약에 따라 최종 `approved` 또는 `rejected`를 확정한다.
- verifier가 근거 부족을 반환하면 main은 해당 문제의 `Resolution`에 따라 현재 권한 안에서 보완을 조정한다. 미해소 항목은 `evidence` 문제로 남겨 거절한다.
