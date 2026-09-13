---
name: config-review
description: "Audit Codex configuration and instruction consistency without edits; not for simple lookups."
---

# Config Review

## 목적
- 지정된 설정을 읽기 전용으로 감사해 역할·흐름의 충돌, 부족한 기준, 중복·모호함과 현재 실행 방식에 맞지 않는 전제를 찾는다.
  사용자 정책을 재정의하거나 새로운 작업 흐름을 설계하지 않는다.

## 컨텍스트 로딩
- 특정 파일이나 skill 감사에서는 대상과 판단에 직접 필요한 권위 문서·참조만 읽는다.
- 전역 감사에서는 `~/.codex/AGENTS.md`, `~/.codex/README.md`, `~/.codex/.gitignore`, `~/.codex/.editorconfig`, `~/.codex/.gitattributes`와
  추적 허용 목록의 `~/.codex/docs/**`, `~/.codex/agents/*.toml`, `~/.codex/skills/*/SKILL.md`, `~/.codex/skills/*/agents/openai.yaml`을 읽는다.
  Codex 제공·플러그인·미관리 로컬 skill은 설치되어 있다는 이유만으로 포함하지 않고, 감사 대상의 실제 호출·로딩 근거에 필요할 때 확인한다.
- 실제 관리 범위는 `.gitignore` 문구만으로 추정하지 말고 `git ls-files`, `git check-ignore` 등 Git 결과로 확인한다.
  예외적으로 추적되는 설정 집합은 `추적 허용 목록`으로 표현한다.

## 외부 기준 사용
- 제품 동작·모델 특성은 `openai-docs`로 최신 공식 자료의 게시일과 적용 대상을 확인한다.
  현재 환경의 관찰을 공식 보장이나 다른 환경의 동작으로 일반화하지 않는다.
- 로컬 역할·정책·흐름은 사용자 목적과 로컬 권위 문서로 판단한다. 문구·링크·소유권의 정적 비교에는 외부 조회를 요구하지 않는다.

## 감사 기준 선택
- 모든 감사에서 `~/.codex/skills/config-review/references/structure.md`를 적용한다.
- 전역 전체 감사, 모델·추론 수준 변경 검토, 하네스 중복 또는 모델 역량 보상형 지침의 축소·제거 판단에는
  `~/.codex/skills/config-review/references/model-harness.md`를 추가로 적용한다.
- 그 외에는 구조 기준만 사용하되 모델·하네스 관련 후보를 발견하면 해당 기준을 추가한다.
- 대상 skill의 역할·작업 유형에 필요한 참조와 실제 호출 경로를 직접 확인한다. 전역 전체 감사에서는 모든 관리 대상 참조를 확인한다.

## 판단 방법
- 행동·명확성·유지보수성의 구체적인 개선을 제안하고, 선호·일반론만으로 규칙을 바꾸지 않는다.
  압축할 때는 주체·조건·행동·중단·근거·출력의 보존 여부와 줄어드는 표현을 확인한다.

## 출력
- 요약과 확인한 범위
- 전체 판정: `정상`, `과함`, `부족`, `충돌`을 해당하는 만큼 복합 병기한다.
- 역할 계약을 검토했으면 전체적인 충분성을 한 번 요약한다.
  부족·과함·충돌이 발견된 역할만 발견 사항에서 개별 판정과 근거를 설명한다.
- 우선순위별 발견 사항
  - 중요한 발견은 위치·근거, 행동 영향, 변경 실익, 제안 방향과 영향받는 파일을 포함한다.
  - 하네스 또는 모델 적합성 관련 발견은 `~/.codex/skills/config-review/references/model-harness.md`의 보고 기준을 적용한다.
  - 의미를 보존하는 압축 후보도 포함하되 성격과 제안이 같은 표현·반복은 묶어 보고한다.
  - README 동기화는 실제 영향이 있는 발견에만 적는다.
- 발견 사항이 없으면 그 사실과 남은 검증 한계를 적는다.
