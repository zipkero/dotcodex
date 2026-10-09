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

## 작업 흐름

- 요청 분류·단계 순서·역할과 승인 경계: `~/.codex/AGENTS.md`
- Phased 승인 상태·무효화·이력: `~/.codex/docs/phased-state.md`
- 진행 상태 인수인계: `~/.codex/skills/context-save/SKILL.md`

## 사용자 정의 skill

- `analyze`: 원인·영향·구조·대안을 읽기 전용으로 분석
- `explain`: 전체 흐름 속 대상의 위치와 상세 동작을 다이어그램·근거로 쉽게 설명. 짧은 용어 질문·후속 확인은 제외
- `commit-push`: 이번 작업 변경만 스테이징·커밋하고 요청 시 푸시. 메시지 추천만 요청하면 제목 후보만 제시
- `cross-analyze`: 명시적으로 요청한 질문을 여러 읽기 전용 subagent로 교차검증
- `project-init`: 프로젝트 README·ROADMAP과 필요한 프로젝트 문서 구성
- `spec-init`: 기능 spec과 상태 README 작성
- `design-init`: 승인된 spec 기반 design 작성
- `implement-init`: 승인된 design 기반 Task 작성
- `implement`: Per-Request 직접 구현·위임 또는 Phased Task 구현 조정
- `implement-loop`: 여러 Task의 순차 구현·검증·재시도 조정
- `verify`: 구현의 승인·거절 판정
- `config-review`: 설정의 역할 경계·중복·방어 지침·명확성·흐름과 README를 읽기 전용으로 감사
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

단계별 절차는 해당 skill, agent 실행 성격은 agent TOML에서 정의한다.

## 출력 형식 계약

이 설정이 만드는 작업 문서와 Phased 보고는 다른 도구가 읽는 형식이다. 설명 문구는 다듬을 수 있지만, 아래 표지 단어·값·위치와 문서 형식을 바꾸면 동작 변경으로 다룬다.

- 작업 문서 표시: `skills/spec-init/SKILL.md` §README.md 형식·§spec.md 형식, `skills/design-init/SKILL.md` §design.md 형식, `skills/implement-init/SKILL.md` §implement.md 형식, `skills/project-init/SKILL.md` §산출물의 ROADMAP 첫 줄 `<!-- prowl-workflow: v1 -->`.
- feature·spec 형식: `skills/spec-init/SKILL.md` §생성과 갱신의 feature 폴더 이름, §README.md 형식의 `## 상태`와 SPEC·DESIGN·IMPLEMENT 체크박스, §spec.md 형식의 §1 첫 문단과 §5 완료 조건 번호 항목.
- design·Task 형식: `skills/design-init/SKILL.md` §design.md 형식의 절 번호, `skills/implement-init/SKILL.md` §Task 규칙·§implement.md 형식의 Task 줄·필드 이름·참조 필드 ` / ` 구분·번호만 나열.
- ROADMAP 형식: `skills/project-init/SKILL.md` §산출물의 마일스톤 제목과 작업 후보 줄.
- verify 보고: `skills/verify/references/phased.md` §입력과 판정의 첫 줄 표시, `skills/verify/references/acceptance.md` §출력의 번호 항목 경계, `Status`·`Target`의 Task ID·`Completed requirements` 줄 형식·`Category`의 네 값과 backtick·`Repair stage`·`Resolution`.
- implement 보고: `skills/implement/references/worker.md` §반환의 첫 줄 표시, `Status` 값과 `Target`의 Task ID.
- implement-loop 연동: `skills/implement-loop/SKILL.md` §재시도와 기록·§중단의 근거 부족 재검증 규칙, §완료 보고의 첫 줄 표시와 `Stopped at`·`Stop reason` 값·backtick·`Resolution`.
- 요청 종류 식별: skill 이름 `implement`·`verify`·`implement-loop`, agent 이름 `verifier`.

## Git

`.gitignore`는 공유 가능한 관리 파일만 추적하도록 구성한다. `config.toml`, 인증 정보, history, logs, state, cache, sessions, tmp와 `skills/.system/`은 추적하지 않는다.
