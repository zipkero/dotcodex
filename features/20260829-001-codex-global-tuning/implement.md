# Codex 전역 설정 조정 구현

## 체크리스트

- [x] task-001: `Phased` 진입 기준 조정
  - 목적: 완료·검증 단위가 여러 개라는 이유만으로 `Phased`를 권장하지 않고, 문서화가 필요한 설계 판단이나 외부 영향이 없다면 요청을 독립 단위로 나눠 `Per-Request`로 진행하게 한다.
  - 접근: `AGENTS.md`의 문서 우선 흐름에서 `Phased` 권장 조건을 구현 전 설계 선택, 되돌리기 어려운 외부 영향, 완료 조건·영향 범위의 문서 고정 필요로 한정한다. 사용자의 명시적 `Phased` 선택 규칙은 유지하고, 단위 수만 많은 요청은 독립적인 완료·검증 단위로 분해한다는 기준을 명시한다.
  - 검증 조건:
    - 결과: `AGENTS.md`가 완료·검증 단위의 개수와 `Phased` 진입 필요성을 구분하며, 구현 전 결정이나 문서화 필요가 없는 다단위 요청은 `Per-Request`에서 독립 단위로 진행하도록 규정한다. 사용자가 명시적으로 선택한 `Phased`와 기존 feature 진행 규칙은 유지된다.
    - 확인: `rg -n 'Per-Request|Phased|완료·검증|설계 선택|되돌리기 어려운|완료 조건|영향 범위|독립.*단위' AGENTS.md`로 진입 조건과 분해 기준을 확인하고, `git diff --check -- AGENTS.md` 및 `git diff -- AGENTS.md`로 문구 변경과 기존 명시 진입 규칙 보존을 검토한다. `git diff --name-only -- AGENTS.md skills agents config.toml hooks.json`로 설정 변경 범위가 승인된 직접 변경 대상 밖으로 확장되지 않았는지 확인한다.
  - 참조: SPEC §5.1, DESIGN §1.1, DESIGN §2.1, DESIGN §3.1, DESIGN §4.1, DESIGN §5.1

- [x] task-002: 하네스와 모델 적합성 감사 기준 추가
  - 목적: 설정 감사가 문구 유사성이나 모델의 일반 역량만으로 지침을 제거하지 않고, 실제 소비 주체별 로딩 범위와 기존 실패 가설 및 대표 작업 근거를 바탕으로 지침의 유지·축소 여부를 판정하게 한다.
  - 접근: `skills/config-review/SKILL.md`의 감사 기준에 `하네스와 모델 적합성` 관점을 추가한다. 소비 주체·모델·호출 경로·로딩 시점 확인, 정책·하네스 중복·역량 보상형 후보 구분, 기존 실패 가설과 통제된 대표 작업 비교, 근거 부족 시 `검증 필요` 처리 및 관련 출력 항목을 기존 감사 흐름에 통합한다.
  - 검증 조건:
    - 결과: 같은 소비 주체와 같은 로딩 범위에서 동일 행동이 강제될 때만 하네스 중복 후보로 판정하며, 모델 역량 보상형 후보는 기존 실패 가설과 관찰 가능한 대표 작업의 기대 결과 및 비교 근거를 요구한다. 제거 안전성을 뒷받침하는 근거가 부족하면 해당 후보만 `검증 필요`로 분류해 현재 지침을 유지하고, 감사 결과에는 소비 주체, 실제 로딩 범위, 후보 유형, 실패 가설, 대표 작업·기대 결과, 비교 결과와 `유지`·`축소/제거 후보`·`검증 필요` 처분이 포함된다.
    - 확인: `rg -n '하네스와 모델 적합성|소비 주체|로딩 범위|후보 유형|실패 가설|대표 작업|기대 결과|비교 결과|유지|축소/제거 후보|검증 필요' skills/config-review/SKILL.md`로 입력·판정·근거 부족 처리·출력 계약을 확인하고, `git diff --check -- skills/config-review/SKILL.md` 및 `git diff -- skills/config-review/SKILL.md`로 기존 역할·권한, 흐름·상태, 문구·컨텍스트 감사 기준이 보존된 채 새 관점이 통합됐는지 검토한다. `git diff --name-only -- AGENTS.md skills agents config.toml hooks.json`로 설정 변경 범위가 승인된 직접 변경 대상 밖으로 확장되지 않았는지 확인한다.
  - 참조: SPEC §5.2, DESIGN §1.1, DESIGN §1.3, DESIGN §2.2, DESIGN §3.2, DESIGN §4.1, DESIGN §5.2

- [x] task-003: implement worker 추론 수준 하향
  - 목적: built-in implement worker가 기존 모델과 구현 계약을 유지하면서 `medium` 추론 수준으로 호출되어 구현 추론 비용을 낮춘다.
  - 접근: `skills/implement/SKILL.md`의 worker 호출 계약에서 `model = "gpt-5.6-sol"`은 유지하고 `reasoning_effort` 값만 `high`에서 `medium`으로 변경한다. `fork_turns`, 필수 입력, `blocked` 처리, 대체 금지와 반환 계약은 바꾸지 않는다.
  - 검증 조건:
    - 결과: built-in `worker` 호출 계약이 `model = "gpt-5.6-sol"`, `reasoning_effort = "medium"`을 명시하며, worker의 나머지 호출·실패·반환 계약과 analyzer, verifier, explorer의 기존 모델·`high` 추론 수준은 유지된다.
    - 확인: `rg -n -C 2 'worker 호출에는|model = "gpt-5\.6-sol"|reasoning_effort = "(high|medium)"' skills/implement/SKILL.md`로 worker 값을 확인하고, `rg -n 'model = "gpt-5\.6-terra"|reasoning_effort = "high"|explorer' AGENTS.md` 및 `rg -n '^(model|model_reasoning_effort) = ' agents/analyzer.toml agents/verifier.toml`로 explorer·analyzer·verifier 설정 보존을 확인한다. `git diff --check -- skills/implement/SKILL.md`와 `git diff -- skills/implement/SKILL.md`로 이 파일의 실질 변경이 worker 추론 수준 한 값뿐인지 검토하고, `git diff --name-only -- AGENTS.md skills agents config.toml hooks.json`로 전체 설정 변경이 세 승인 대상에 한정됐는지 확인한다.
  - 참조: SPEC §5.3, DESIGN §1.1, DESIGN §2.3, DESIGN §3.3, DESIGN §4.1, DESIGN §4.2, DESIGN §5.3
