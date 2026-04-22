# Codex 전역 설정

이 디렉터리는 Codex가 일관된 방식으로 작업하도록 만드는 사용자 정의 전역 설정만 관리한다. 로컬 실행 상태, 인증 정보, 로그, 캐시는 관리 대상이 아니다.

## 관리 대상

- `AGENTS.md`: 모든 Codex 작업에 적용되는 전역 지침
- `skills/*/SKILL.md`: 특정 작업 유형에서만 로드되는 사용자 정의 skill
- `.editorconfig`, `.gitattributes`: 텍스트 포맷 기준
- `.gitignore`: 로컬 상태 파일을 제외하는 allowlist 규칙

## 문서 우선 개발 플로우

기능 개발, 동작 변경, 단순하지 않은 버그 수정, 다중 파일 수정, API/DB/auth/external integration 변경, 또는 증거가 중요한 작업은 `docs/<feature-name>/` 플로우를 사용한다.

기본 순서:

`spec-init` -> `analyze-init` -> `implement-init` -> `implement` -> `verify`

필수 산출물은 다음과 같다.

- `README.md`: feature 상태와 이력
- `spec.md`: 요구사항, 범위, 완료 조건
- `analysis.md`: 분석, 설계 결정, 영향 범위, 리스크
- `implement.md`: 구현 체크리스트와 항목별 검증 기준

이 문서들은 범위, 결정 사항, 구현 상태의 source of truth이다. 검증은 `verify` skill이 사용자에게 승인/거절 판단과 근거를 보고하며, 별도 `verify.md`를 만들지 않는다.

`analyze` skill은 필요 시 사용하는 조사 도구이며 문서 단계가 아니다. 문서 단계의 분석 산출물은 `analyze-init`이 작성하는 `analysis.md`이다.

## Skill 구성

사용자 정의 skill은 `skills/.system` 밖에 둔다.

- `skills/analyze`: 코드 분석, 원인 파악, 영향 범위 확인
- `skills/spec-init`: `spec.md`와 feature `README.md` 초기화
- `skills/analyze-init`: `spec.md` 기반 `analysis.md` 작성
- `skills/implement-init`: `analysis.md` 기반 `implement.md` 체크리스트 작성
- `skills/implement`: `implement.md` 기반 구현 수행
- `skills/verify`: 구현 결과 승인/거절 판단과 근거 보고

`skills/.system`은 Codex 제공 내장 skill 영역이므로 직접 관리하지 않는다.

## 언어 기준

- `AGENTS.md`와 `SKILL.md` 본문은 한국어로 작성한다.
- `SKILL.md` frontmatter의 `name`은 영어 ASCII kebab-case로 작성한다.
- `SKILL.md` frontmatter의 `description`은 Codex의 자동 트리거 안정성을 위해 영어로 작성한다.
- 설정 파일명, 디렉터리명, skill 이름, config key는 영어 ASCII를 우선한다.
- 사용자-facing 설명은 한국어로 작성하고, Codex가 작업 분류에 사용하는 짧은 메타데이터는 영어로 작성한다.

## Git 관리 정책

`.gitignore`는 기본적으로 전체를 무시한 뒤 필요한 파일만 허용한다.

추적 대상:

- `.gitignore`
- `.editorconfig`
- `.gitattributes`
- `README.md`
- `AGENTS.md`
- 사용자 정의 `skills/**`

비추적 대상:

- `config.toml`
- `auth.json`
- `history.jsonl`
- `logs_*.sqlite`, `state_*.sqlite`
- `cache/`, `sessions/`, `tmp/`, `.tmp/`
- `skills/.system/`

이 구조는 로컬 실행 상태와 공유 가능한 Codex 작업 정책을 분리하기 위한 것이다.
