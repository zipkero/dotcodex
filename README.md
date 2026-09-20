# Codex 전역 설정

이 디렉터리는 Codex의 사용자 정의 운영 지침, agent와 skill을 관리한다. 인증 정보, 세션, 로그와 캐시는 관리하지 않는다.

## 관리 대상
- `AGENTS.md`: 전역 작업 흐름과 역할·승인 경계
- `docs/phased-state.md`: Phased 승인 상태와 의미·의존 관계 기반 무효화
- `agents/*.toml`: analyzer와 verifier 정의
- `skills/*/`: 사용자 정의 skill과 필요한 참조
- `features/**`: Phased 기능 문서
- `.editorconfig`, `.gitattributes`, `.gitignore`: 저장소 관리 설정

## Phased 문서
`features/<feature-dir>/`에는 다음 문서를 둔다.
- `README.md`: 기능 상태와 이력
- `spec.md`: 요구사항·범위·완료 조건
- `design.md`: 구조·설계 결정·영향 범위
- `implement.md`: Task와 검증 조건

별도 `verify.md`는 만들지 않는다.

## 사용자 정의 skill
- `analyze`: 원인·영향·구조·대안을 읽기 전용으로 분석
- `explain`: 코드·변경·시스템의 작동 방식을 근거와 함께 설명
- `cross-analyze`: 여러 subagent의 독립 분석을 교차검증
- `project-init`: 프로젝트 README·ROADMAP과 필요한 프로젝트 문서 구성
- `spec-init`: 기능 spec과 상태 README 작성
- `design-init`: 승인된 spec 기반 design 작성
- `implement-init`: 승인된 design 기반 Task 작성
- `implement`: Per-Request 또는 Phased Task 구현 조정
- `implement-loop`: 여러 Task의 순차 구현·검증·재시도 조정
- `verify`: 구현의 승인·거절 판정
- `config-review`: 설정의 역할·호출·참조·상태 정합성 감사
- `context-save`: 작업 인수인계를 `CONTEXT.md`에 저장
- `context-restore`: 저장된 맥락을 읽기 전용으로 복원

추적 허용 목록 밖의 로컬·Codex 제공·plugin skill은 이 저장소의 관리 대상이 아니다.

## Agent와 참조
- `agents/analyzer.toml`: `design.md`와 `implement.md` 후보를 반환하는 읽기 전용 agent
- `agents/verifier.toml`: 구현의 독립 후보 판정을 반환하는 읽기 전용 agent
- `skills/implement/references/worker.md`: worker 실행·반환 계약
- `skills/implement/references/phased.md`: Phased 구현 진입·Task 선택·문서 처리
- `skills/verify/references/acceptance.md`: 구현 승인 판정 계약
- `skills/verify/references/phased.md`: Phased 완료 조건 판정·상태 전환
- `skills/config-review/references/structure.md`: 설정 구조 감사 기준

전역 참조는 `~/.codex/...`로 적고 agent 호출에는 홈을 확장한 절대 경로를 전달한다. 단계별 절차는 해당 skill, agent 실행 성격은 agent TOML, 최종 적용·판정·상태 변경은 main이 소유한다.

## Git
`.gitignore`는 공유 가능한 관리 파일만 추적하도록 구성한다. `config.toml`, 인증 정보, history, logs, state, cache, sessions, tmp와 `skills/.system/`은 추적하지 않는다.
