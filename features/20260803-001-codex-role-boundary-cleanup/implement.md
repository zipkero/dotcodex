# Codex 역할·Skill 경계 정리 구현

> 이 문서는 당시 승인된 구현의 기록이다. 구현 위임의 현재 기준은 루트 `AGENTS.md`와 `skills/implement/SKILL.md`를 따른다.

## 체크리스트

- [x] task-001: built-in worker 구현·검증 라우팅 일원화
  - 목적: 단일 Task와 작은 변경은 지정된 모델의 built-in `worker`가 구현하고, 여러 Task의 반복과 사용자 확인,
    최종 승인·거절 및 문서 상태 변경은 main이 일관되게 소유한다.
  - 접근: `AGENTS.md`, `README.md`, `skills/implement/SKILL.md`, `skills/implement-loop/SKILL.md`,
    `skills/implement-loop/agents/openai.yaml`, `skills/verify/SKILL.md`를 명시적 worker 호출 계약에 맞추고
    `agents/implementer.toml`을 삭제한다. 일반 구현 기준은 `implement`에만 두고 built-in 역할은 재정의하지 않는다.
  - 검증 조건:
    - 결과: 모든 단일 Task 호출은 `agent_type = "worker"`, `model = "gpt-5.6-sol"`,
      `reasoning_effort = "medium"`, `fork_turns = "none"` 또는 양의 정수 문자열을 명시하며 생략이나 `"all"`을 사용하지 않는다.
      호출 메시지는 Task의 목적·접근·검증 조건, 변경 범위, 기준 문서, `implement` Skill, 반환 형식을 자체 완결적으로 포함한다.
      worker는 공유 작업 공간의 다른 변경을 되돌리지 않고 현재 변경에 맞춰 작업하며, 사용자 판단이 필요하면 수정 없이
      `blocked`를 반환한다. 필요한 override가 노출되지 않거나 호출이 실패하면 다른 모델·agent, main 직접 구현이나
      `codex exec`로 대체하지 않고 main이 Task 상태를 유지한 채 중단 사유를 보고한다. verifier는 후보 판단만 반환하고,
      main만 최종 판단과 상태를 변경하며 `quality`, `correctness`, `design/scope`는 모두 차단 범주로 유지된다.
    - 확인: 현재 `spawn_agent` 입력 규격과 대상 파일의 diff를 대조하고, 삭제된 `agents/implementer.toml`과
      custom `implementer`, 전체 이력 fork, 조용한 대체 경로를 가리키는 잔여 문구가 없는지 검색한다.
      worker 메시지 필수 항목, 공유 작업 공간 경계, 호출 실패 보고, main의 상태 소유와 세 차단 범주를 확인한 뒤
      UTF-8·LF 형식과 `git diff --check` 결과를 확인한다.
  - 참조: SPEC §5.1, SPEC §5.2, SPEC §5.3, SPEC §5.4, ANALYSIS §1.1, ANALYSIS §1.2,
    ANALYSIS §2.1, ANALYSIS §2.2, ANALYSIS §3.1, ANALYSIS §3.2, ANALYSIS §3.3, ANALYSIS §4

- [x] task-002: 문서·추적 계약과 읽기 전용 agent 설정 보존
  - 목적: 구현 agent 전환 뒤에도 단계별 문서 계약, 기능 문서 Git 추적 정책과 analyzer·verifier의 읽기 전용 설정이 유지되고,
    README가 실제 agent 구성을 정확히 안내한다.
  - 접근: Task 1의 `README.md` 결과와 현재 `.gitignore`, `agents/analyzer.toml`, `agents/verifier.toml`,
    `skills/verify-analysis/SKILL.md`, `skills/implement-init/SKILL.md`, `skills/spec-init/SKILL.md`를 보존 계약과 대조한다.
    이미 충족한 문서·추적 규칙과 analyzer·verifier 설정은 다시 설계하거나 변경하지 않는다.
  - 검증 조건:
    - 결과: README는 custom `analyzer`, built-in `worker`, custom `verifier`를 구분하고 custom implementer를 남기지 않는다.
      README와 `.gitignore`는 모두 `features/**`를 관리·추적 대상으로 유지한다. analyzer와 verifier의 읽기 전용 경계,
      모델·추론 수준과 기존 `nickname_candidates` 유무·값은 유지된다. `verify-analysis`의 선행 문서 안내와 여덟 판단 기준,
      `spec-init`의 번호 표시와 `SPEC §5.N`, `implement-init`의 한 번에 검증 가능한 Task 분해 기준도 유지된다.
    - 확인: README의 agent·Git 설명과 `.gitignore`의 `!/features/`, `!/features/**`를 상호 대조하고,
      대표 feature 문서가 Git 무시 대상이 아닌지 `git check-ignore`로 확인한다. analyzer·verifier 설정과 세 단계 Skill을
      승인된 분석의 보존 계약에 맞춰 직접 확인하고, 검증 강도 약화, 별도 `verify.md`, Skill 위치나 설정 변경이 없는지 점검한다.
      UTF-8·LF 형식, feature Markdown 150칸 제한과 `git diff --check` 결과를 확인한다.
  - 참조: SPEC §5.3, SPEC §5.4, SPEC §5.6, ANALYSIS §1.3, ANALYSIS §2.4,
    ANALYSIS §3.3, ANALYSIS §3.4, ANALYSIS §4

- [x] task-003: Git 참고 정보 저장·복원 계약 보존
  - 목적: 맥락 저장 시 브랜치와 기준 커밋만 참고 정보로 남기고, 복원 시 현재 값과의 차이만 안전하게 확인하며
    파일·브랜치와 작업 공간은 변경하지 않는다.
  - 접근: 현재 `skills/context-save/SKILL.md`와 `skills/context-restore/SKILL.md`를 승인된 읽기 전용 조회,
    필드별 `확인 불가` 정규화와 차이 보고 계약에 맞춰 재검증하고 기존 동작을 변경하지 않는다.
  - 검증 조건:
    - 결과: 저장 당시 브랜치와 전체 `HEAD` 커밋만 기록하며 Git 저장소 아님, detached HEAD, 최초 커밋 전과 필드별 조회 실패를
      정해진 `확인 불가` 값으로 처리한다. 복원은 다른 필드만 저장값과 현재 값을 함께 표시하고, 두 필드가 같으면 `없음`으로 보고한다.
      Git 차이는 복원 차단이나 권위 기준으로 사용되지 않으며 status·diff 기록, checkout·reset·브랜치 전환,
      파일 복구·커밋이나 작업 공간 변경을 수행하지 않는다.
    - 확인: 두 Skill의 저장 형식, 조회 실패 처리와 복원 보고 형식을 상호 대조한다. 정상 저장소, 비저장소, detached HEAD,
      최초 커밋 전, 한쪽 또는 양쪽 필드가 `확인 불가`인 경우를 모두 판정할 수 있는지 확인하고,
      금지된 Git 조사·자동 전환·복구 지시가 없는지 검색한다. UTF-8·LF 형식과 `git diff --check` 결과를 확인한다.
  - 참조: SPEC §5.5, ANALYSIS §1.3, ANALYSIS §2.3, ANALYSIS §3.4, ANALYSIS §4
