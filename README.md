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

- 파일 변경은 기본적으로 `implement`의 Per-Request로 진행한다. main이 범위를 확정하고 직접 구현하거나 worker에게 맡긴다.
- Phased는 `spec-init` → `design-init` → `implement-init` → `implement` → `verify` 순서로 진행한다. 여러 Task나 기능 전체 구현은 `implement-loop`가 조정한다.
- 분석·설명·설정 감사·맥락 복원은 해당 읽기 전용 skill에서 끝낸다.
- 승인 상태·무효화·이력은 `docs/phased-state.md`, 진행 상태 인수인계는 `context-save`가 소유한다.

## 사용자 정의 skill

- `analyze`: 원인·영향·구조·대안을 읽기 전용으로 분석
- `explain`: 코드·변경·시스템의 작동 방식을 근거와 함께 설명
- `cross-analyze`: 명시적으로 요청한 질문을 여러 읽기 전용 subagent로 교차검증
- `project-init`: 프로젝트 README·ROADMAP과 필요한 프로젝트 문서 구성
- `spec-init`: 기능 spec과 상태 README 작성
- `design-init`: 승인된 spec 기반 design 작성
- `implement-init`: 승인된 design 기반 Task 작성
- `implement`: Per-Request 직접 구현·위임 또는 Phased Task 구현 조정
- `implement-loop`: 여러 Task의 순차 구현·검증·재시도 조정
- `verify`: 구현의 승인·거절 판정
- `config-review`: 설정의 역할·호출·참조·상태 정합성을 읽기 전용으로 감사
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

## Prowl workflow 규약

Prowl은 이 설정이 만드는 작업 문서와 Phased 보고의 형식을 규약 v1로 정한다. 작업 문서는 현재 읽으며, Codex 세션 보고 수집은 아직 지원하지 않는다. Prowl은 산출물을 읽고 전역 설정을 수정하지 않는다.

설명 문구는 다듬을 수 있지만, 아래 표지 단어·값·위치와 문서 형식은 규약에 기대는 자리다. 형식 변경은 Prowl이 새 규약 버전과 읽기 계약으로 정하고, 이 설정이 그 뒤에 맞춘다.

- 규약 명세: [zipkero/prowl의 workflow 규약 v1](https://github.com/zipkero/prowl/blob/main/docs/workflow-contract/v1.md). 작업 문서는 §1·§2, 보고는 §1·§3, 요청 종류를 가리는 이름은 §4를 따른다. 표지 단어와 값 목록은 §5가 소유한다.
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
