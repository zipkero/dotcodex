---
name: cross-analyze
description: "Cross-check one question with independent read-only agents when explicitly requested."
---

# Cross Analyze

## 입력과 호출
- 사용자가 명시적으로 요청했을 때 하나의 질문을 3~5개 독립 subagent에 맡긴다. 수가 없으면 3개를 사용하고 범위를 벗어나면 사용자에게 확인한다.
- built-in `default`를 `model = "gpt-5.6-sol"`, `reasoning_effort = "high"`, `fork_turns = "none"`으로 호출한다.
- 모든 agent에 같은 자체 완결적 질문, 관련 절대 경로, 읽기 전용 범위와 근거 반환 형식을 전달한다.
- 동시 실행 한도를 넘으면 배치로 나누고 모든 호출과 재시도가 끝난 뒤 결과를 종합한다.

## 실패와 종합
- 호출 실패·빈 결과·필수 근거 누락은 같은 입력으로 한 번만 재시도한다.
- 유효 결과가 2개 미만이면 `교차검증 불충분`으로 보고한다.
- main은 공통 결론, 불일치와 단독 발견의 근거를 확인해 종합하며 파일을 변경하지 않는다.

## 출력
- 요청 수, 유효 결과 수, 실패 수
- 결론별 합의 수와 공통 근거
- 불일치와 main 판정, 근거가 확인된 단독 발견
- 남은 사용자 결정이나 다음 조사 대상

`M/N`은 같은 결론의 유효 결과 수와 전체 유효 결과 수이며 정확도 확률이 아니다.
