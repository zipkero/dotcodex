# Codex 전역 설정

이 디렉터리는 Codex가 일관된 방식으로 작업하도록 만드는 사용자 정의 전역 설정과 사용자 정의 skill만 관리한다.
로컬 실행 상태, 인증 정보, 로그, 캐시는 관리 대상이 아니다.

## 관리 대상

- `AGENTS.md`: 모든 Codex 작업에 적용되는 전역 지침
- allowlist에 포함된 `skills/*/`: 특정 작업 유형에서만 로드되는 사용자 정의 skill
- `.editorconfig`, `.gitattributes`: 텍스트 포맷 기준
- `.gitignore`: 로컬 상태 파일을 제외하는 allowlist 규칙

## 문서 우선 개발 플로우

요구사항 확정, 설계 판단, 다중 단계 검증이 필요한 작업은 `docs/<feature-dir>/` 문서 플로우를 사용한다.
새 feature 디렉터리는 `spec-init` 기준에 따라 `docs/<yyyyMMdd>-<nnn>-<feature-name>/` 형식으로 만든다.

기본 순서:

`spec-init` -> `analyze-init` -> `implement-init` -> `implement` -> `verify`

필수 산출물은 다음과 같다.

- `README.md`: feature 상태와 이력
- `spec.md`: 요구사항, 범위, 완료 조건
- `analysis.md`: 분석, 설계 결정, 영향 범위, 리스크
- `implement.md`: 구현 체크리스트와 항목별 검증 기준

이 문서들은 범위, 결정 사항, 구현 상태의 기준이다.
검증은 `verify` skill이 사용자에게 승인/거절 판단과 근거를 보고하며, 별도 `verify.md`를 만들지 않는다.

`analyze` skill은 필요 시 사용하는 조사 도구이며 문서 단계가 아니다.
문서 단계의 분석 산출물은 `analyze-init`이 작성하는 `analysis.md`이다.

## Skill 구성

사용자 정의 skill은 `skills/.system` 밖에 둔다.
실제 관리 대상은 `.gitignore` allowlist에 포함되고 `SKILL.md`가 있는 사용자 정의 skill 디렉터리이다.

- `skills/analyze`: 코드 분석, 원인 파악, 영향 범위 확인
- `skills/spec-init`: `spec.md`와 feature `README.md` 초기화
- `skills/analyze-init`: `spec.md` 기반 `analysis.md` 작성
- `skills/implement-init`: `analysis.md` 기반 `implement.md` 체크리스트 작성
- `skills/implement`: `implement.md` 기반 구현 수행
- `skills/verify`: 구현 결과 승인/거절 판단과 근거 보고
- `skills/flow`: 선택된 코드의 실행 흐름, 데이터 흐름, 책임 흐름 설명
- `skills/global-review`: 전역 설정과 사용자 정의 skill 정합성 점검

`skills/.system`은 Codex 제공 내장 skill 영역이므로 직접 관리하지 않는다.
`skills/hatch-pet`처럼 `.gitignore` allowlist에 포함되지 않은 로컬 skill은 현재 전역 설정 관리 대상이 아니다.
다만 로컬 Codex 런타임에는 활성 skill로 노출될 수 있다.
`SKILL.md`가 없는 런타임/캐시성 디렉터리는 관리 대상 skill로 보지 않는다.

## 정책 소유 위치

전역 지침과 사용자 정의 skill은 요청 범위가 과하게 확장되지 않도록 소유 위치를 나눈다.

- 작업 분류, 변경 원칙, 언어 기준은 `AGENTS.md`가 소유한다.
- Markdown 줄바꿈 기준은 `AGENTS.md`와 `.editorconfig`가 소유한다.
- 단계별 실행 절차는 각 `skills/*/SKILL.md`가 소유한다.
- README는 관리 대상 파일, 구조, 설계 의도만 설명한다.

## Git 관리 정책

`.gitignore`는 기본적으로 전체를 무시한 뒤 필요한 파일만 허용한다.

추적 대상:

- `.gitignore`
- `.editorconfig`
- `.gitattributes`
- `README.md`
- `AGENTS.md`
- `.gitignore` allowlist에 포함된 사용자 정의 `skills/*/**`

비추적 대상:

- `config.toml`
- `auth.json`
- `history.jsonl`
- `logs_*.sqlite`, `state_*.sqlite`
- `cache/`, `sessions/`, `tmp/`, `.tmp/`
- `skills/.system/`

이 구조는 로컬 실행 상태와 공유 가능한 Codex 작업 정책을 분리하기 위한 것이다.
