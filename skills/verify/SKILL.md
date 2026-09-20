---
name: verify
description: "Approve or reject an implementation against requirements, diff, and verification evidence."
---

# Verify

## 컨텍스트 로딩
- main은 `~/.codex/skills/verify/references/acceptance.md`의 판정 계약을 적용한다.
- `Phased` 작업은 `~/.codex/skills/verify/references/phased.md`의 입력 조건·판정 기준·상태 전환을 추가로 적용한다.
- `Per-Request` 작업은 사용자가 검증을 명시적으로 요청했거나 독립 검증이 필요한 변경의 직전 구현 대상이 단일하게 식별되어야 한다.
  Phased 전용 절차와 기능 상태 갱신은 적용하지 않는다.

## 대상과 변경 범위
- main이 검증 대상을 하나로 확정하고 변경 범위는 다음 우선순위로 정한다.
  1. 사용자가 지정한 commit, 파일 목록 또는 비교 범위
  2. 같은 흐름의 implement worker가 반환한 `Changed files`와 diff
  3. 복원 작업의 `CONTEXT.md`에 기록된 변경 파일, branch와 기준 HEAD
  4. working tree와 Git history에서 수집한 후보
- 대상이나 범위를 확정할 수 없으면 후보·근거·필요한 입력을 사용자에게 제시하고, 확인 전에는 verifier 호출과 판정을 보류한다.

## verifier agent 사용 기준
- 독립 검증을 명시했거나 보안·권한·데이터 손실·복구 곤란 변경처럼 오류 영향이 크거나, 복잡한 상태·동시성·경계·컴포넌트 상호작용으로
  자체 판단이 어려우면 이름 있는 custom agent `verifier`를 사용한다. 파일 수와 무관하며 한 파일도 중대하면 사용한다.
  영향이 제한되고 diff와 필요한 실행 결과로 확인되는 변경은 main이 직접 검증할 수 있다.
- verifier 호출에는 작업 유형, 검증 대상·범위, 선행 문서, 프로젝트 `AGENTS.md`,
  `~/.codex/skills/verify/references/acceptance.md`의 실제 절대 경로와 실행 근거 위치를 포함한다.
  코드 검증이면 `~/.codex/docs/languages.md`와 해당 언어 문서의 실제 절대 경로도 전달한다.
  Phased일 때만 `~/.codex/skills/verify/references/phased.md`, `~/.codex/docs/phased-state.md`의 실제 절대 경로와 기능 문서를 추가하고,
  verifier가 지정된 원본·지침 파일을 직접 읽어 적용하도록 명시한다. main용 진입 절차를 위임하지 않는다.

## 출력과 상태 전환
- main은 판정 계약에 따라 직접 수집한 근거나 verifier의 후보 근거를 검토해 최종 `approved`·`rejected`를 확정한다.
  Phased 상태 전환은 이후 main만 Phased 참조의 `상태 전환`에 따라 수행한다.
