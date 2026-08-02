# Codex 전역 설정

이 디렉터리는 Codex가 일관된 방식으로 작업하도록 만드는 사용자 정의 전역 설정과 사용자 정의 skill만 관리한다.
로컬 실행 상태, 인증 정보, 로그, 캐시는 관리 대상이 아니다.

## 관리 대상

- `AGENTS.md`: 모든 Codex 작업에 적용되는 전역 지침
- `docs/**`: 전역 지침에서 참조하는 보조 기준 문서
- allowlist에 포함된 `agents/*.toml`: 특정 역할의 custom subagent 정의
- allowlist에 포함된 `skills/*/`: 특정 작업 유형에서만 로드되는 사용자 정의 skill
- `.editorconfig`, `.gitattributes`: 텍스트 포맷 기준
- `.gitignore`: 로컬 상태 파일을 제외하는 allowlist 규칙

## Feature 문서 구조

문서 우선 작업에서 생성되는 feature 문서 세트는 `features/<feature-dir>/` 아래 다음 산출물로 구성된다.

단계는
`spec-init` → `verify-spec` → `analyze-init` → `verify-analysis` → `tasks-init` → `verify-tasks` → `implement` → `verify`
순서로 진행한다.

- `README.md`: feature 상태와 이력
- `spec.md`: 요구사항, 범위, 완료 조건
- `analysis.md`: 분석, 설계 결정, 영향 범위
- `implement.md`: 승인된 분석에서 파생한 구현 Task 체크리스트와 항목별 검증 기준

상태판의 `SPEC`, `ANALYSIS`, `TASKS`는 각각 현재 `spec.md`, `analysis.md`, `implement.md`가 대응 검증에서 승인됐음을 뜻한다.
`IMPLEMENT`는 승인된 Task가 모두 구현되고 구현 검증을 통과했음을 뜻한다. 검증 상세를 저장하는 별도 Markdown 문서는 만들지 않는다.
단계별 문서·Task 보존, 체크박스, 상태와 이력 전이는 해당 작성·검증 skill이 소유하고 feature README 상태판은 결과와 이력만 기록한다.

## Skill 구성

사용자 정의 skill은 `skills/.system` 밖에 둔다.
실제 관리 대상은 `.gitignore` allowlist에 포함되고 `SKILL.md`가 있는 사용자 정의 skill 디렉터리이다.

- `skills/analyze`: 코드 분석, 원인 파악, 영향 범위 확인, 설계 선택지 비교
- `skills/explain-change`: 코드 변경의 배경, 핵심 생각, 관련 흐름과 구현 판단 설명
- `skills/project-init`: 프로젝트 루트 `README.md`와 `ROADMAP.md` 초기화
- `skills/spec-init`: `spec.md`와 feature `README.md` 초기화
- `skills/verify-spec`: 사용자 요청과 프로젝트 근거에 따른 `spec.md` 승인·거절 후보 판단
- `skills/analyze-init`: 승인된 `spec.md` 기반 `analysis.md` 작성
- `skills/verify-analysis`: 코드·설정 직접 조사에 따른 `analysis.md` 승인·거절 후보 판단
- `skills/tasks-init`: 승인된 `spec.md`와 `analysis.md` 기반 `implement.md` 체크리스트 작성
- `skills/verify-tasks`: `implement.md`의 요구사항 매핑, Task 경계와 검증 가능성 승인·거절 후보 판단
- `skills/implement`: 문서화된 Task 또는 작은 Per-Request 변경 구현
- `skills/implement-loop`: 남은 Task를 구현, 검증, 상태 전환 순서로 반복하고 사용자 판단이 필요하면 중단
- `skills/verify`: 구현 결과 승인/거절 판단과 근거 보고
- `skills/config-review`: 전역 설정, 역할 프롬프트, 책임 경계, allowlist 관리 대상 사용자 정의 skill 정합성 점검
- `skills/context-save`: 현재 작업 맥락과 다음 작업을 프로젝트 루트 `CONTEXT.md`에 저장
- `skills/context-restore`: `CONTEXT.md`와 원본 문서를 대조해 저장된 작업 맥락을 읽기 전용으로 복원

`skills/.system`은 Codex 제공 내장 skill 영역이므로 직접 관리하지 않는다.
`.gitignore` allowlist에 포함되지 않은 로컬 skill이나 런타임/캐시성 디렉터리는 현재 전역 설정 관리 대상이 아니다.
다만 로컬 Codex 런타임에는 활성 skill로 노출될 수 있다.

## Custom Agent 구성

custom agent는 `agents/*.toml`에 둔다.
실제 관리 대상은 `.gitignore` allowlist에 포함된 standalone TOML 파일이다.

- `agents/verifier.toml`: 네 검증 skill 중 지정된 기준에 따라 원본을 조사하고 후보 판단만 반환하는 읽기 전용 subagent 정의
- `agents/analyzer.toml`: `analyze-init`과 `tasks-init`의 전체 문서 본문을 만드는 읽기 전용 subagent 정의

## 정책 소유 위치

전역 지침과 사용자 정의 skill은 요청 범위가 과하게 확장되지 않도록 소유 위치를 나눈다.

- `AGENTS.md`는 항상 적용되어야 하는 언어, 응답, 요청 해석, 범위, 안전, 공통 네이밍·주석 원칙과
  skill·언어별 기준 라우팅을 소유한다.
- `docs/languages.md`는 언어별 작업 기준의 진입점을 소유하고, 세부 기준은 `docs/languages/*.md`가 소유한다.
- `features/**` Markdown 줄바꿈 기준은 `AGENTS.md`와 `.editorconfig`가 소유한다.
- 단계별 실행 절차는 각 `skills/*/SKILL.md`가 소유한다.
- custom subagent의 역할과 실행 성격은 각 `agents/*.toml`이 소유한다.
- 문서 작성은 `spec-init`, `analyze-init`, `tasks-init`이 각각 소유하고 대응 승인의 세부 기준은
  `verify-spec`, `verify-analysis`, `verify-tasks`가 각각 소유한다.
- 테스트 Task 작성은 `tasks-init`, 테스트 코드 작성은 `implement`, 구현 승인 기준과 Task 완료 후처리 절차는 `verify`가 소유한다.
- main은 verifier의 후보 판단을 검토한 최종 승인·거절과 feature 상태 전환을 소유한다.
- 특정 코드 변경의 배경과 동작 이해를 돕는 해설은 `explain-change`가 소유한다.
- 구현 단계의 세부 네이밍·주석 기준, 공개 contract 보존과 구현 품질 가드는 `implement`가 소유한다.
- README는 관리 대상 파일, 구조, 설계 의도만 설명한다.

## Git 관리 정책

`.gitignore`는 기본적으로 전체를 무시한 뒤 필요한 파일만 허용한다.

추적 대상:

- `.gitignore`
- `.editorconfig`
- `.gitattributes`
- `README.md`
- `AGENTS.md`
- `.gitignore` allowlist에 포함된 `docs/**`
- `.gitignore` allowlist에 포함된 `agents/*.toml`
- `.gitignore` allowlist에 포함된 사용자 정의 `skills/*/**`

비추적 대상:

- `config.toml`
- `auth.json`
- `history.jsonl`
- `logs_*.sqlite`, `state_*.sqlite`
- `cache/`, `sessions/`, `tmp/`, `.tmp/`
- `skills/.system/`

이 구조는 로컬 실행 상태와 공유 가능한 Codex 작업 정책을 분리하기 위한 것이다.
