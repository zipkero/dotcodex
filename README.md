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

- `README.md`: feature 상태와 이력
- `spec.md`: 요구사항, 범위, 완료 조건
- `analysis.md`: 분석, 설계 결정, 영향 범위
- `implement.md`: 구현 체크리스트와 항목별 검증 기준

별도 `verify.md`는 만들지 않는다.

## Skill 구성

사용자 정의 skill은 `skills/.system` 밖에 둔다.
실제 관리 대상은 `.gitignore` allowlist에 포함되고 `SKILL.md`가 있는 사용자 정의 skill 디렉터리이다.

- `skills/analyze`: 코드 분석, 원인 파악, 영향 범위 확인, 설계 선택지 비교
- `skills/project-init`: 프로젝트 루트 `README.md`와 `ROADMAP.md` 초기화
- `skills/spec-init`: `spec.md`와 feature `README.md` 초기화
- `skills/analyze-init`: `spec.md` 기반 `analysis.md` 작성
- `skills/implement-init`: `analysis.md` 기반 `implement.md` 체크리스트 작성
- `skills/implement`: 문서화된 Task 또는 작은 Per-Request 변경 구현
- `skills/verify`: 구현 결과 승인/거절 판단과 근거 보고
- `skills/config-review`: 전역 설정, 역할 프롬프트, 책임 경계, allowlist 관리 대상 사용자 정의 skill 정합성 점검

`skills/.system`은 Codex 제공 내장 skill 영역이므로 직접 관리하지 않는다.
`.gitignore` allowlist에 포함되지 않은 로컬 skill이나 런타임/캐시성 디렉터리는 현재 전역 설정 관리 대상이 아니다.
다만 로컬 Codex 런타임에는 활성 skill로 노출될 수 있다.

## Custom Agent 구성

custom agent는 `agents/*.toml`에 둔다.
실제 관리 대상은 `.gitignore` allowlist에 포함된 standalone TOML 파일이다.

- `agents/verifier.toml`: `skills/verify`가 필요할 때 사용할 수 있는 읽기 전용 검증 subagent 정의

## 정책 소유 위치

전역 지침과 사용자 정의 skill은 요청 범위가 과하게 확장되지 않도록 소유 위치를 나눈다.

- `AGENTS.md`는 항상 적용되어야 하는 언어, 응답, 요청 해석, 범위, 안전, skill 라우팅 원칙만 소유한다.
- `docs/languages.md`는 언어별 작업 기준의 진입점을 소유하고, 세부 기준은 `docs/languages/*.md`가 소유한다.
- `features/**` Markdown 줄바꿈 기준은 `AGENTS.md`와 `.editorconfig`가 소유한다.
- 단계별 실행 절차는 각 `skills/*/SKILL.md`가 소유한다.
- custom subagent의 역할과 실행 성격은 각 `agents/*.toml`이 소유한다.
- 테스트 Task 작성은 `implement-init`, 테스트 코드 작성은 `implement`, 승인 판단과 Task 완료 후처리는 `verify`가 소유한다.
- 구현 중 네이밍, 주석, 공개 contract 보존, 구현 품질 가드는 `implement`가 소유한다.
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
