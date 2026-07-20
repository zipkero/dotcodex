---
name: repo-readiness
description: "Audit whether a repository provides enough discoverable, evidence-backed context for Codex to start or resume work in a fresh session without relying on prior conversation. Use when users ask whether a repo is agent-ready, cold-start ready, self-describing, or suitable for reliable long-running Codex work."
---

# Repo Readiness

## 목적

- 이전 대화가 없는 새 세션에서 저장소 문서와 설정만으로 작업 범위, 기준, 실행 방법과 현재 상태를 복원할 수 있는지 점검한다.
- 파일을 수정하지 않고 확인 결과와 보완 위치만 보고한다.

## 확인 범위

- 적용되는 루트 및 하위 `AGENTS.md`
- 루트 `README.md`와 직접 연결된 주요 문서
- manifest, build, test, lint, formatter 설정
- 사용자가 지정했거나 단일하게 식별되는 활성 feature 문서
- 문서가 가리키는 경로와 명령의 실제 존재 여부

필요한 진입점부터 읽고, 발견 가능성 확인에 필요하지 않은 전체 문서나 코드는 탐색하지 않는다.

## 판단 기준

- 진입점: `AGENTS.md`와 `README.md`에서 작업에 필요한 문서를 찾을 수 있는가.
- 실행 기준: build, test, lint, 실행 명령을 실제 설정으로 확인할 수 있는가.
- 정책 소유권: 프로젝트·도메인 규칙의 기준 위치와 적용 범위가 명확한가.
- 작업 복원: 활성 작업의 목표, 제약, 현재 상태, 남은 Task와 검증 기준을 대화 없이 복원할 수 있는가.
- 정합성: 중첩 지침, 문서, 설정 사이에 충돌하거나 오래된 설명이 없는가.

문서 수나 형식 자체를 평가하지 않는다. 실제 작업 판단에 영향을 주는 누락, 충돌과 발견성 문제만 보고한다.

## 독립 검사

- 사용자가 새 세션 또는 cold-start 검사를 요청하면 가능한 경우 새 읽기 전용 explorer에게 저장소 경로만 전달해 독립적으로 확인한다.
- 이전 대화, 예상 결과나 main의 판단은 전달하지 않는다.
- 독립 검사를 사용할 수 없으면 main이 같은 범위를 확인하고 독립 검사가 아니었음을 보고한다.

## 판정

- `ready`: 필요한 맥락과 명령을 찾을 수 있고 작업을 막는 충돌이 없다.
- `partial`: 작업은 시작할 수 있지만 일부 판단이나 검증 근거가 부족하다.
- `not-ready`: 범위, 정책, 실행 방법 또는 현재 작업을 신뢰할 수 있게 복원할 수 없다.

## 출력

1. Status: `ready`, `partial`, `not-ready`
2. Entry points: 확인한 문서 진입점
3. Recovered context: 복원한 프로젝트·작업·검증 정보
4. Gaps: 누락되거나 근거가 부족한 정보
5. Conflicts: 충돌하거나 오래된 기준
6. Evidence: 확인한 파일과 설정
7. Next action: 보완할 소유 문서와 최소 변경 방향

## 제외

- 파일 생성·수정
- 범용 문서나 Issue template 강제
- 코드 구현·검증
- PR, commit, push
- 인증 정보와 추적 제외된 로컬 상태 조사
