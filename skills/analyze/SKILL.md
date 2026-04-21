---
name: analyze
description: "Analyze code, debug behavior, explain architecture, identify root causes, and determine scope before documentation-first implementation. Use when the user asks for analysis, investigation, root cause, impact assessment, or system understanding."
---

# Analyze

## 목적
- 구현 전에 시스템 동작, 원인, 영향 범위를 근거 기반으로 파악한다.
- 분석과 구현을 분리한다.

## 원칙
- 파일을 수정하지 않는다.
- 실제 코드, 로그, 에러, 문서를 근거로 삼는다.
- 추정은 근거와 분리해 명시한다.
- 분석 결과가 구현 범위, 동작 변경, 리스크, 검증 증거에 영향을 줄 수 있으면 문서 우선 플로우 대상인지 판단한다.
- 분석 중 구현이 필요해 보이면 바로 수정하지 말고 필요한 다음 단계를 제안한다.

## 출력 구조
- 요약: 1-2문장 결론.
- 근거: 확인한 파일, 로그, 명령, 관찰 사항.
- 흐름: 실행 흐름, 데이터 흐름, 상태 변화 중 관련 내용.
- 원인: 확인된 root cause 또는 아직 좁혀지지 않은 지점.
- 다음 단계: 필요한 문서 단계 또는 구현/검증 제안.

## Blocker
- `scope undefined`: 분석 대상이 불명확함
- `infeasible`: 현재 제약에서 불가능함
- `needs input`: 사용자 결정이나 외부 정보가 필요함

Blocker는 반드시 확인한 근거와 해소 조건을 함께 제시한다.
