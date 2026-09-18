---
name: analyze
description: "Investigate causes and impacts, evaluate architecture and tradeoffs, or propose designs without editing files. Use when the requested outcome is diagnosis, analysis, a decision, or a design recommendation; not when the primary outcome is an explanation of how existing code, changes, or systems work."
---

# Analyze

## 목적
- 파일을 수정하지 않고 원인과 영향 범위를 조사하고, 구조·대안을 판단하거나 설계를 제안한다. Phased 설계 문서 작성은 `design-init`의 역할이다.
- 조사 결과를 전달하는 데 필요한 동작 설명은 포함할 수 있지만, 요청 산출물이 기존 코드·변경·시스템의 이해를 위한 설명이면 `explain`의 역할이다.

## 컨텍스트 로딩
- 사용자가 `features/<feature-dir>/` 또는 하위 파일을 지정하면 기능 범위로 보고, 질문에 필요한 기능 문서와 관련 코드·테스트를 확인한다.
- 사용자가 특정 파일, 심볼, 에러, 로그를 지정하면 그 대상과 필요한 주변 맥락을 읽는다.
- 변경 분석 요청은 지정된 diff, commit, branch나 파일 범위와 관련 호출부·테스트를 확인한다.
- 명시된 범위가 없으면 대화 근거로 가정을 밝히고, 대상 영역조차 정할 수 없으면 `scope undefined`로 필요한 입력을 요청한다.

## 분석 원칙
- 코드·로그·에러·문서의 확인 사실과 추정을 구분한다. 조사로 해소할 불확실성은 먼저 확인하고,
  결과를 바꾸는 사용자 의도·범위·성공 기준이 미확정이면 권장안·근거·장단점과 함께 질문한다.
- 원인 조사에서는 관련 흐름, 확인된 원인과 영향 범위, 아직 좁혀지지 않은 지점과 가정을 구분해 보고한다.
- diff·변경 분석에서는 기존 동작과 변경 후 동작을 근거로 영향, 위험, 필요한 후속 판단을 도출한다.
- 설계·구조 분석 요청에서는 문제 크기에 맞게 가능한 구조 선택지들을 펼치고,
  각 선택지의 장단점, 유지보수 영향, 구현 난이도, 검증 기준을 비교해 추천안 하나와 주요 대안을 제시한다.
- 코드, spec, 로그에서 확인되지 않은 일반론적 우려를 개선안처럼 제시하지 않는다.

## 출력 구조
- 결론·근거·필요한 후속 변경을 질문의 범위에 맞게 보고한다. 남은 미확정 판단은 있을 때만 적는다.
- 구현 범위에 영향이 있을 때만 `Phased 권장`, `Per-Request 가능`, `추가 입력 필요` 중 하나와 필요한 다음 단계를 적는다.
  Phased 권장 여부와 선택 확인은 `~/.codex/AGENTS.md`의 `문서 우선 흐름`을 따른다.

## Blocker
- `scope undefined`: 분석 대상이 불명확함
- `infeasible`: 현재 제약에서 불가능함
- `needs input`: 사용자 결정이나 외부 정보가 필요함

Blocker는 반드시 확인한 근거와 해소 조건을 함께 제시한다. 근거 없이 가정만으로 선언하지 않는다.
