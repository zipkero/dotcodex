# Codex 역할·Skill 경계 정리 명세

> 이 문서는 당시 완료 조건의 기록이다. 구현 위임의 현재 기준은 루트 `AGENTS.md`와 `skills/implement/SKILL.md`를 따른다.

## 1. 범위

### 1.1 입력 맥락

- 루트 `AGENTS.md`의 문서 우선 흐름과 agent 라우팅
- 루트 `README.md`와 `.gitignore`의 관리 대상 설명
- `agents/analyzer.toml`, `agents/implementer.toml`, `agents/verifier.toml`
- `skills/implement`, `skills/implement-loop`, `skills/verify`, `skills/verify-analysis`, `skills/implement-init`, `skills/spec-init`
- `skills/context-save`, `skills/context-restore`, `skills/implement-loop/agents/openai.yaml`
- Codex 0.146.0의 현재 `spawn_agent` 호출 규격과 최신 공식 Subagents 문서
- OpenAI Codex 공식 저장소의 모델 재정의·전체 이력 fork 관련 구현 및 문제 보고
- Codex 개발자들이 사용하는 명시적 모델 호출, custom agent 고정과 외부 작업자 우회 사례
- 이번 대화에서 확정한 built-in `worker`와 모델 선택의 관계, 브랜치 참고 정보

## 2. 목표

- built-in `worker`의 구현 역할을 바꾸지 않고 Task 구현 모델만 `gpt-5.6-sol`, 추론 수준은 `medium`으로 선택한다.
- Task 하나의 구현, 여러 Task의 반복 실행, 검증과 최종 상태 변경의 책임을 각각 하나의 주체에 둔다.
- `analysis.md`는 별도 verifier가 원본과 독립 대조하는 현재 검증 강도를 유지한다.
- 실제 구조와 어긋난 README·Skill 설명과 직접 중복된 규칙을 정리한다.
- 작업 맥락을 복원할 때 저장 당시 브랜치와 기준 커밋의 차이를 안전하게 식별할 수 있게 한다.
- 이 저장소에서 생성하는 기능 문서가 Git 관리 대상에 포함되도록 한다.

## 3. 제약

- 구현 agent의 행동을 모델 선택 목적으로 새로 정의하지 않고 Codex가 제공하는 built-in `worker`를 사용한다.
- main은 `worker` 호출 시 `model = "gpt-5.6-sol"`, `reasoning_effort = "medium"`과 전체 이력을 사용하지 않는
  `fork_turns` 값을 명시한다.
- 모델 재정의 입력이 현재 호출 기능에 없거나 지정한 호출이 실패하면 다른 모델이나 custom agent로 조용히 대체하지 않는다.
- fresh 또는 일부 대화만 전달하는 worker가 구현할 수 있도록 Task, 범위, 기준 문서, 적용할 Skill과 검증 조건을 호출 메시지에 포함한다.
- `analysis.md` 작성 기준과 `verify-analysis`의 독립 검증 기준은 축소하거나 약화하지 않는다.
- `verify`의 모든 거절 범주가 승인을 막는 엄격한 정책은 유지한다.
- analyzer와 verifier의 모델·추론 수준, Skill 설치 위치와 `nickname_candidates`는 변경하지 않는다.
- 브랜치와 커밋 정보는 참고와 불일치 감지에만 사용하며 자동 전환이나 권위 기준으로 사용하지 않는다.
- 설정과 기능 문서는 UTF-8, LF로 작성하고 `features/**` Markdown의 한 줄 표시폭은 150칸을 넘기지 않는다.

## 4. 제외 범위

- built-in `worker`의 역할 지침 복제 또는 custom `worker` 재정의
- 모델 선택을 위한 별도 `implementer` 역할 유지
- `[agents].default_subagent_model`을 통한 모든 subagent의 전역 모델 변경
- 호출 기능이 모델 선택을 지원하지 않을 때 별도 `codex exec` 프로세스로 우회
- analyzer·verifier의 `nickname_candidates` 추가·삭제·변경
- Skill 설치 위치 이전 또는 심볼릭 링크 구성
- 문서 우선 단계 자체의 추가·삭제
- Git 브랜치 자동 전환, 커밋 또는 작업 공간 변경 복구
- `config-review`, `project-init`, `analyze`, `explain-change`의 일반적인 압축이나 설명 변경
- `docs/languages/**`의 광범위한 용어 정리

## 5. 완료 조건

1. 단일 Task는 main이 명시적 모델·추론 수준과 전체 이력을 사용하지 않는 `fork_turns`로 호출한 built-in `worker`가 구현하고,
   여러 Task나 전체 구현은 main이 `implement-loop`로 반복하며 최종 승인·거절과 문서 상태 변경은 main만 수행한다고 관련 기준이 일치한다.
2. worker 호출 메시지는 fresh 또는 일부 대화만 전달받아도 구현할 수 있는 Task, 범위, 기준 문서, 적용할 Skill과 검증 조건을 포함하고,
   모델 재정의를 지원하지 않거나 호출이 실패하면 다른 모델·agent로 대체하지 않고 main이 중단 사유를 보고한다.
3. 사용하지 않는 `agents/implementer.toml`이 제거되고 README와 `skills/implement-loop/agents/openai.yaml`이
   `analyzer`·built-in `worker`·`verifier` 구성과 반복 책임을 정확히 설명한다.
4. `verify-analysis`의 선행 문서 누락 안내, `SPEC §5.N` 표시, Task 분해 문구와 `verify`의 차단 범주 이름이 명확하고 일관된다.
5. `context-save`는 저장 당시 브랜치와 기준 커밋을 참고 정보로 기록하고, `context-restore`는 현재 값과 비교해 불일치를 보고하되
   파일이나 브랜치를 변경하지 않는다.
6. `.gitignore`와 README의 Git 관리 정책이 `features/**` 기능 문서를 추적 대상으로 일치시킨다.
