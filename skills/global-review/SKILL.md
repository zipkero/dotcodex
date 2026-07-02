---
name: global-review
description: "Audit global Codex configuration for consistency, ownership, and trimming opportunities."
---

# Global Review

## 목적
`AGENTS.md`, `README.md`, `docs/**`, allowlist에 포함된 `agents/*.toml`, 사용자 정의 `skills/*/SKILL.md`를 읽고 전역 설정의 정합성을 점검한다.
파일은 수정하지 않고 분석 결과만 보고한다.

이 skill의 근본 목적은 Codex 전역 설정이 모델의 자율 실행 성향 때문에 요청 범위를 과하게 확장하거나,
같은 절차를 여러 문서에 중복 정의하거나, Codex 실행 모델과 맞지 않는 역할·절차를 전제하는 일을 막는 것이다.

## 컨텍스트 로딩
- 사용자가 특정 파일이나 skill을 대상으로 지정하면 해당 대상과 판단에 직접 필요한 참조만 읽는다.
- 사용자가 전역 감사 또는 전체 점검을 요청한 경우에만 `AGENTS.md`, 루트 `README.md`,
  `.gitignore`, `docs/**`, allowlist에 포함된 `agents/*.toml`, 사용자 정의 `skills/*/SKILL.md` 전체를 읽는다.

## 범위
- 규칙 간 의미 충돌
- 모호한 표현
- 같은 규칙의 중복 정의
- 의미 손실 없이 줄일 수 있는 표현
- bullet 하나에 서로 다른 독립 규칙이 묶인 항목
- 구체 예시나 도구 나열이 원칙보다 커져 적용 범위를 좁히는 항목
- 의미 없이 부정형을 반복해 읽기 비용을 늘리는 항목
- 현재 설정에 없는 기능이나 폐기된 설명
- `docs/**`와 `features/**` 밖 Markdown 문서에 150칸 제한을 전역 규칙처럼 적용하는 항목
- 권위 위치 불일치
  - `AGENTS.md`는 전역 원칙만 소유한다.
  - `docs/languages.md`는 언어별 작업 기준의 진입점만 소유하고, 세부 기준은 `docs/languages/*.md`가 소유한다.
  - `agents/*.toml`은 custom subagent의 역할, 모델·추론·sandbox 성격, developer instructions를 소유한다.
  - phase별 실행 절차는 각 `skills/*/SKILL.md`가 소유한다.
  - 테스트 작성 범위는 `implement-init`과 `implement`가 소유한다.
  - 승인 판단, evidence 기준, Task 완료 후처리는 `verify` skill이 소유한다.
- 문서 우선 흐름의 단계 실행 기준
  - 다음 단계가 사용자 요청 없이 실행되도록 쓰여 있지 않은지 확인한다.
  - 사용자가 자동 진행을 명시 요청할 수 있는 여지는 막지 않는다.
- 구현 품질 가드 위치
  - 기본 설계 원칙은 `implement`가 소유한다.
  - 테스트 통과용 하드코딩, fixture 전용 특수분기, assertion 맞춤 동작 축소 금지는 `implement`에 있어야 한다.
  - 주석 작성 기준은 과도하게 보강하지 않고 현재 `implement` 기준과 충돌하지 않아야 한다.
- 문서 산출물 완결성
  - `analyze-init`은 이전 대화 맥락 없이 `spec.md`와 `analysis.md`만으로 구현 체크리스트를 만들 수 있게 하는 기준을 가져야 한다.
  - `implement-init`은 미매핑 `SPEC §5.N`을 조용히 넘기지 않고 Task 추가, 완료 조건 제거, 제외 범위 보류 중 하나로 결정하게 해야 한다.
- context health 저하 신호
  - `AGENTS.md`가 항상 로드되어야 할 전역 원칙보다 특정 skill의 실행 절차나 예시를 길게 설명한다.
  - 같은 규칙이 `AGENTS.md`와 여러 `SKILL.md`에 반복 정의되어 있다.
  - custom agent의 역할 지침이 `AGENTS.md`나 `SKILL.md`에 중복되어 있다.
  - 특정 skill에서만 필요한 운영 상세가 `AGENTS.md`처럼 항상 로드되는 파일에 있다.
  - `SKILL.md`가 자기 trigger 밖의 정책까지 설명한다.
  - 오래된 handoff, 임시 메모, 실험 기록이 공식 규칙처럼 남아 있다.
- Codex 실행 모델과 맞지 않는 역할·절차 전제
- `agents/*.toml` 필수 필드, 역할 범위, sandbox 설정, 지침 언어 기준 위반
- `SKILL.md` frontmatter의 `description`과 본문 역할 불일치
- 사용자-facing 한국어 문서와 작업 분류용 영어 메타데이터의 언어 기준 위반
- README의 실제 구조·기능 설명 정확성
  - 폐기되었거나 존재하지 않는 기능·파일이 남아 있는지 확인한다.
  - 새로 추가된 docs, skill, 정책 소유 위치, 문서 구조가 누락되었는지 확인한다.
  - 링크와 예시가 현재 파일 구조와 맞는지 확인한다.

## 제외
- 요청 없는 자동 수정
- `AGENTS.md` 정책 체계나 외부 플로우 설계 확장
- README에는 룰 정합성 기준을 적용하지 않는다. 다만 README가 실제 구조와 기능을 정확히 설명하는지는 별도 항목으로 점검한다.
- 전역 설정 수정 제안이 README의 관리 대상, skill 목록, 정책 소유 위치 설명과 어긋날 수 있으면 README 동기화 필요성을 영향받는 파일에 함께 안내한다.
- 선호나 일반론만 근거로 한 문구 변경 제안

## 출력
- 요약
- 전체 판정: `정상`, `과함`, `부족`, `충돌` 중 하나로 적고, 충돌이 없어도 context health가 나쁘면 `과함`으로 판단한다.
- 우선순위별 발견 사항
- 위치
- 현재 문제
- 제안 방향
- 영향받는 파일
- README 동기화 필요 여부
- 필요한 경우 before/after 수정안
- 발견 사항이 없으면 그렇게 말하되, 확인한 파일 범위와 남은 리스크를 함께 적는다.
