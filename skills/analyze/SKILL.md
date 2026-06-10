---
name: analyze
description: "Analyze code, debug behavior, architecture, root causes, and scope without writing files."
---

# Analyze

## 목적
- 즉석 조사, 디버깅, 코드 이해를 대화 안에서 수행한다.
- 파일을 만들거나 수정하지 않는다.
- `analyze-init`과 다르다. 이 skill은 문서 단계가 아니며, `analysis.md`는 `analyze-init`이 작성한다.

## 컨텍스트 로딩
- 사용자가 `docs/<feature-dir>/` 또는 하위 파일을 지정하면 feature 범위로 보고, 질문에 필요한 `spec.md`, `analysis.md`, `implement.md`만 읽는다.
- 사용자가 특정 파일, 심볼, 에러, 로그를 지정하면 그 대상과 필요한 주변 맥락을 읽는다.
- 범위가 비어 있으면 대화 맥락에서 충분한 신호가 있을 때만 명시적 가정으로 진행한다.
- 대상 시스템이나 영역을 결정할 수 없으면 `scope undefined`로 중단한다.

## 분석 원칙
- 이 skill이 활성화된 턴에서는 사용자가 수정 가능성을 언급했더라도 먼저 분석 결과와 수정 범위를 보고한다.
- 실제 코드, 로그, 에러, 문서를 근거로 삼고 추정은 근거와 분리해 명시한다.
- root cause, 영향 범위, 미확인 가정을 분리한다.
- 일반 보안, 성능, 컴플라이언스, SLO 체크리스트를 대상에 투사하지 않는다. 보고 전에 이 프로젝트의 코드, spec, 로그로 확인한다.
- 구현이 필요해 보여도 바로 수정하지 않고 다음 단계를 제안한다.

## 출력 구조
- 요약: 1-2문장 결론.
- 근거: 확인한 파일, 로그, 명령, 관찰 사항.
- 관련 흐름: 실행 흐름, 데이터 흐름, 상태 변화.
- 원인: 확인된 root cause 또는 아직 좁혀지지 않은 지점.
- 분류: `Phased(문서 우선)` 대상 / Per-Request 가능 / 추가 입력 필요 중 하나. 구현 범위에 영향이 있을 때만 포함한다.
- 다음 단계: 필요한 문서 단계(`spec-init`, `analyze-init`, `implement-init`) 또는 구현/검증 제안.

## Blocker
- `scope undefined`: 분석 대상이 불명확함
- `infeasible`: 현재 제약에서 불가능함
- `needs input`: 사용자 결정이나 외부 정보가 필요함

Blocker는 반드시 확인한 근거와 해소 조건을 함께 제시한다. 근거 없이 가정만으로 선언하지 않는다.
