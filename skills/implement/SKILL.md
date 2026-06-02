---
name: implement
description: "Execute one documented Task or a small per-request code change within the requested scope."
---

# Implement

## 목적
- Phased mode에서는 `implement.md`의 Task 하나를 최소 범위로 구현한다.
- Per-Request mode에서는 사용자가 요청한 작은 변경을 문서 플로우 없이 처리한다.

## 컨텍스트 로딩
1. Phased mode로 진입하는 경우:
   - 사용자가 `docs/<feature-dir>/` 또는 `docs/<feature-dir>/implement.md`를 지정했다.
   - 현재 대화에서 해당 feature에 대해 `spec-init`, `analyze-init`, `implement-init`이 실행되었거나,
     사용자가 구현 의도로 feature를 명시했다.
2. Phased mode 동작:
   - `spec.md`, `analysis.md`, `implement.md`를 읽는다.
   - 사용자가 `task-<nnn>`을 지정하면 해당 Task를 잡고, 지정하지 않으면 위에서부터 첫 미완료 Task를 잡는다.
   - Task가 없거나 이미 완료되었거나 둘 이상으로 해석되면 구현하지 않고 범위를 요청한다.
3. Per-Request mode:
   - Phased mode 조건이 없고 요청이 작고 명확하며 되돌리기 쉬운 변경이면 바로 처리한다.
   - `docs/<feature-dir>/`를 만들지 않는다.
   - 범위 확장, 새 의존성, 공개 API 변경, 설계 판단이 필요하면 진행 전 사용자 결정을 요청한다.

## 구현 규칙
- 기존 코드 패턴과 프로젝트 관례를 우선한다.
- 파일 수정 전 어떤 변경을 할지 짧게 설명한다.
- 구현은 대상 Task 단위로 진행하고, 한 턴에 하나의 Task만 구현한다.
- Task 밖에서 발견한 문제는 수정하지 말고 보고만 한다.
- 요청이나 Task가 요구하지 않는 신규 인터페이스, 추상화, public API, 경계, 의존성, 인접 리팩터링은 추가하지 않는다.
- 이 확장이 요청 충족에 필요하면 코드를 쓰기 전에 이유, 영향, 대안을 보고하고 사용자 결정을 받는다.
- Phased mode에서 설계 변경이 필요하면 `analysis.md` 갱신 필요 사항으로 보고하고 구현을 멈춘다.
- 테스트 코드는 명시적 테스트 Task가 있거나, 버그 수정의 단일 회귀 테스트 예외에 해당할 때만 작성한다.
- Per-Request mode에서는 테스트를 조용히 추가하지 않는다. 회귀 위험은 완료 보고의 한계로 남긴다.
- 테스트 실패를 해결할 때도 테스트가 기대하는 내부 구조에 운영 코드를 맞추지 말고,
  외부 관찰 가능한 동작, 공개 contract, `analysis.md`의 설계 결정을 기준으로 원인을 판단한다.
- 테스트를 통과하려면 구조, 경계, API, 상태 소유권 변경이 필요해 보이는 경우에는
  구현하지 말고 설계 변경 필요 사항으로 보고한다.
- 테스트, 포맷, 빌드 명령은 변경 범위를 확인하는 데 필요한 수준으로 실행한다.
- 검증 단계에서 `approved`로 판단하기 전에는 `implement.md` 체크박스와
  `docs/<feature-dir>/README.md`의 `IMPLEMENT` 상태를 변경하지 않는다.

## 주석 작성 기준
- 이름과 구조만으로 의도가 드러나면 달지 않는다. 공개 경계에는 호출자가 알아야 하는 책임이나 계약만
  한 줄로 남기고, 내부 구현 절차는 쓰지 않는다.
- 코드만으로 드러나지 않는 도메인 제약, 불변식, 실패 조건, 외부 API 계약,
  바꾸면 깨지는 선택은 가장 가까운 위치에 짧게 남긴다.
- 변경 경위, 변경 전후 설명, 진행 단계나 계획 식별자 같은 구현 과정의 메타 정보는 넣지 않는다.
- 구조와 형식은 해당 언어와 프로젝트 컨벤션을 따르고, 주석 언어는 파일 컨벤션을 따른다.
  컨벤션이 없고 사용자가 다르게 요청하지 않았으면 한국어로 쓴다.
- 로직을 수정하면 관련 주석도 같은 turn에 갱신하거나 제거한다.

## 완료 보고
- 변경한 파일
- 실행한 Task 또는 Per-Request 요청
- 실행한 확인
- 남은 항목 또는 리스크
- Phased mode에서는 다음 단계로 해당 Task 검증이 필요함을 알린다.
