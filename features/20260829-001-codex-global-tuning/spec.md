# Codex 전역 설정 조정 명세

## 1. 범위

### 1.1 입력 맥락
- `AGENTS.md`: Per-Request와 Phased 분류 및 built-in explorer 호출 계약
- `skills/config-review/SKILL.md`: Codex 전역 설정 감사 기준
- `skills/implement/SKILL.md`: built-in worker 호출 계약
- `~/.claude/CLAUDE.md`, `~/.claude/commands/config-review.md`, `~/.claude/agents/implementer.md`: 비교한 Claude 전역 설정
- 양쪽 저장소의 2026-08-28 최신 Git 이력과 OpenAI GPT-5.6·AGENTS.md·Skills 공식 문서

## 2. 목표
- 완료·검증 단위가 여러 개라는 이유만으로 Phased를 권장하지 않도록 진입 조건을 좁힌다.
- 설정 감사에서 하네스 중복과 이전 모델의 약점을 보완하던 지침의 현재 필요성을 검토하게 한다.
- 구현 worker의 추론 수준을 `medium`으로 낮추고 analyzer, verifier와 explorer의 현재 추론 수준은 유지한다.

## 3. 제약
- Codex의 전역 분기와 라우팅은 `AGENTS.md`, 설정 감사 절차는 `skills/config-review/SKILL.md`, worker 호출 계약은 `skills/implement/SKILL.md`가 각각 소유한다.
- GPT-5.6용 감사 기준은 반복 지침을 무조건 제거하지 않고 대표 작업 검증과 기존 실패 가설을 요구해야 한다.
- 변경 뒤 관련 문구 검색과 Git diff로 승인 범위 밖 설정이 바뀌지 않았음을 확인한다.

## 4. 제외 범위
- Claude의 `rules/feature-docs.md` 또는 같은 경로 glob 기반 규칙 체계 추가
- `hooks.json`의 `PostCompact` 추가와 Orca 백엔드 변경
- analyzer, verifier, explorer의 모델 또는 추론 수준 변경
- Claude 전용 model, plugin, permission, agent frontmatter 이식

## 5. 완료 조건
1. 완료·검증 단위가 여러 개인 것만으로는 Phased를 권장하지 않고 독립 단위로 나눠 진행한다는 기준이 전역 지침에 명시된다.
2. 설정 감사가 하네스 중복과 모델 역량 보상형 지침을 실제 로딩 범위와 대표 작업 검증에 근거해 판정한다.
3. built-in implement worker는 `gpt-5.6-sol`과 `reasoning_effort = "medium"`으로 호출되고 analyzer, verifier와 explorer의 현재 추론 수준은 유지된다.
