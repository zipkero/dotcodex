# Codex 전역 설정

이 디렉터리는 Codex가 일관된 방식으로 작업하도록 만드는 사용자 정의 전역 설정과 사용자 정의 skill만 관리한다.
로컬 실행 상태, 인증 정보, 로그, 캐시는 관리 대상이 아니다.

## 관리 대상

- `AGENTS.md`: 모든 Codex 작업에 적용되는 전역 지침
- `docs/**`: 전역 지침에서 참조하는 보조 기준 문서
- `features/**`: 문서 우선 작업에서 생성되는 기능 문서
- 추적 허용 목록에 포함된 `agents/*.toml`: 특정 역할의 custom subagent 정의
- 추적 허용 목록에 포함된 `skills/*/`: 특정 작업 유형에서만 로드되는 사용자 정의 skill
- `.editorconfig`, `.gitattributes`: 텍스트 포맷 기준
- `.gitignore`: 로컬 상태 파일을 제외하는 추적 허용 목록 규칙

## 기능 문서 구조

문서 우선 작업에서 생성되는 기능 문서 세트는 `features/<feature-dir>/` 아래 다음 산출물로 구성된다.

- `README.md`: 기능 상태와 이력
- `spec.md`: 요구사항, 범위, 완료 조건
- `analysis.md`: 분석, 설계 결정, 영향 범위
- `implement.md`: 구현 체크리스트와 항목별 검증 기준

별도 `verify.md`는 만들지 않는다.

## Skill 구성

실제 관리 대상은 `.gitignore`의 추적 허용 목록에 포함되고 `SKILL.md`가 있는 사용자 정의 skill 디렉터리이다.

- `skills/analyze`: 코드 분석, 원인 파악, 영향 범위 확인, 설계 선택지 비교
- `skills/explain-change`: 코드 변경의 배경, 핵심 생각, 관련 흐름과 구현 판단 설명
- `skills/project-init`: 프로젝트 루트 `README.md`, `ROADMAP.md`와 필요한 `docs/product.md`, `docs/design.md` 구성
- `skills/spec-init`: `spec.md`와 기능 `README.md` 초기화
- `skills/analyze-init`: `spec.md` 기반 `analysis.md` 작성
- `skills/verify-analysis`: `analysis.md`를 실제 코드·설정과 대조해 승인·거절 후보 판단
- `skills/implement-init`: `analysis.md` 기반 `implement.md` 체크리스트 작성
- `skills/implement`: 문서화된 Task 또는 작은 Per-Request 변경 구현
- `skills/implement-loop`: 남은 Task의 구현, 검증, 재시도와 상태 전환 순서를 조정하고 사용자 판단이 필요하면 중단
- `skills/verify`: 구현 결과 승인/거절 판단과 근거 보고
- `skills/config-review`: 전역 설정, 역할 프롬프트, 책임 경계, 추적 허용 목록의 관리 대상 사용자 정의 skill 정합성 점검
- `skills/context-save`: 현재 작업 맥락과 다음 작업을 프로젝트 루트 `CONTEXT.md`에 저장
- `skills/context-restore`: `CONTEXT.md`와 원본 문서를 대조해 저장된 작업 맥락을 읽기 전용으로 복원

Codex 제공 skill과 추적 허용 목록 밖의 로컬 skill은 위치와 관계없이 현재 전역 설정 관리 대상이 아니다.
이들 skill은 로컬 Codex 런타임에 활성 상태로 노출될 수 있다.

## Agent 구성

custom agent 정의는 `agents/*.toml`에 둔다.
실제 관리 대상은 `.gitignore`의 추적 허용 목록에 포함된 standalone TOML 파일이며, 구현 agent는 custom 정의를 두지 않는다.

- `agents/analyzer.toml`: `Phased` 작업의 `analysis.md`와 `implement.md` 완성 본문을 반환하는 읽기 전용 subagent 정의
- built-in agent의 사용 여부와 역할 선택은 `AGENTS.md`를 따르며, `worker` 호출 계약은 `skills/implement/SKILL.md`가 소유
- `agents/verifier.toml`: `verify-analysis`와 필요한 구현 `verify`에서 후보 판단을 반환하는 읽기 전용 검증 subagent 정의

## 정책 위치

- 전역 원칙과 라우팅은 `AGENTS.md`, 언어별 세부 기준은 `docs/languages/**`에 둔다.
- 단계별 절차와 판단 기준은 해당 `skills/*/SKILL.md`, custom analyzer·verifier의 실행 성격은 `agents/*.toml`이 소유한다.
- 이 README는 관리 대상과 구조만 설명한다.

## Git 관리 정책

`.gitignore`는 기본적으로 전체를 무시한 뒤 필요한 파일만 허용한다.

추적 대상:

- `.gitignore`
- `.editorconfig`
- `.gitattributes`
- `README.md`
- `AGENTS.md`
- `.gitignore`의 추적 허용 목록에 포함된 `features/**`
- `.gitignore`의 추적 허용 목록에 포함된 `docs/**`
- `.gitignore`의 추적 허용 목록에 포함된 `agents/*.toml`
- `.gitignore`의 추적 허용 목록에 포함된 사용자 정의 `skills/*/**`

비추적 대상:

- `config.toml`
- `auth.json`
- `history.jsonl`
- `logs_*.sqlite`, `state_*.sqlite`
- `cache/`, `sessions/`, `tmp/`, `.tmp/`
- `skills/.system/`

이 구조는 로컬 실행 상태와 공유 가능한 Codex 작업 정책을 분리하기 위한 것이다.
