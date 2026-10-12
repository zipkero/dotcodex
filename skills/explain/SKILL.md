---
name: explain
description: "Explain code, changes, or systems through accessible, detailed walkthroughs that place the requested part in the overall flow, with diagrams and source evidence. Use for understanding existing behavior; not for brief term questions, follow-up clarification, cause investigation, new design decisions, implementation, or formal approval."
---

# Explain

## 역할
- 지정된 코드·변경·Task·시스템을 읽기 전용으로 설명해 사용자가 동작과 그 맥락을 이해하도록 돕는다.
- 원인 조사나 새 설계 판단은 `analyze`, 구현과 정식 판정은 해당 skill의 역할이다.

## 근거 확인
- 요청한 대상과 앞뒤 연결을 파악하는 데 필요한 원본 문서, 현재 코드, 호출부, 관련 미커밋 diff, 테스트와 실행 근거를 확인한다.
- 저장된 실행 근거는 현재 코드와 대응하는지 확인한다. 새 실행은 설명에 필요한 읽기 전용 범위에서만 수행한다.
- 확인한 동작과 추론을 구분하고, 설계 이유는 문서나 코드에서 확인되는 범위로 한정한다.

## 설명 구성
1. **목적과 전체 맥락:** 사용자에게 주는 결과와 전체 흐름 속 위치를 설명한다. 입력과 결과를 사용하는 다음 단계를 연결한다.
2. **대상의 상세 동작:** 대표 사례를 따라 처리·데이터·상태·출력을 설명한다. 주요 분기·실패 경로·경계 조건·제약과 변경 전후 영향을 포함한다.
3. **근거와 한계:** 설명 가까이에 파일·심볼 링크와 필요한 코드 발췌를 둔다. 확인하지 못한 부분과 이유를 밝힌다.

## 표현과 시각화
- 배경지식은 사용자의 질문에 맞추고, 별도 정보가 없으면 전문용어는 첫 등장 때 뜻을 풀어 쓴다. 식별자나 코드를 제시하기 전에 역할을 일상적인 말로 설명한다.
- 여러 단계나 구성 요소의 연결은 설명 목적에 맞는 다이어그램으로 보여주고 요청한 대상을 표시한다.
- 도식은 짧은 라벨과 주요 요소의 관계로 구성하고, 세부 조건과 처리 이유는 본문에서 설명한다. 두 표현의 명칭과 흐름을 맞추고, 변경 전후나 여러 대상의 차이는 표로 비교한다.
- 제목과 분량은 질문에 맞추고, 요청한 부분을 중복 없이 상세하게 설명한다.
