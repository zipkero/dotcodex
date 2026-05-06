---
name: analyze
description: "Analyze code, debug behavior, explain architecture, identify root causes, and determine scope before documentation-first implementation. Use when the user asks for analysis, investigation, root cause, impact assessment, or system understanding."
---

# Analyze

## 목적
- 구현 전에 동작, 원인, 영향 범위를 근거 기반으로 파악한다.
- 분석과 구현을 분리한다.
- 이 skill은 조사 도구이며 문서 단계가 아니다. 문서 단계의 분석 산출물은 `analyze-init`이 작성하는 `analysis.md`이다.

## 원칙
- 파일을 수정하지 않는다.
- 이 skill이 활성화된 턴에서는 사용자가 수정 가능성을 언급했더라도 먼저 분석 결과와 수정 범위를 보고한다.
- 실제 코드, 로그, 에러, 문서를 근거로 삼는다.
- 추정은 근거와 분리해 명시한다.
- root cause, 영향 범위, 미확인 가정을 분리한다.
- 결과가 구현 범위, 동작 변경, 리스크, 검증 증거에 영향을 주면 `Phased(문서 우선)` 대상인지 판단한다.
- 구현이 필요해 보여도 바로 수정하지 말고 다음 단계를 제안한다.

## 출력 구조
- 요약: 1-2문장 결론.
- 분류: `Phased(문서 우선)` 대상 / Per-Request 가능 / 추가 입력 필요 중 하나.
- 근거: 확인한 파일, 로그, 명령, 관찰 사항.
- 흐름: 관련 실행 흐름, 데이터 흐름, 상태 변화.
- 원인: 확인된 root cause 또는 아직 좁혀지지 않은 지점.
- 다음 단계: 필요한 문서 단계(`spec-init`, `analyze-init`, `implement-init`) 또는 구현/검증 제안.

## Blocker
- `scope undefined`: 분석 대상이 불명확함
- `infeasible`: 현재 제약에서 불가능함
- `needs input`: 사용자 결정이나 외부 정보가 필요함

Blocker는 반드시 확인한 근거와 해소 조건을 함께 제시한다.
