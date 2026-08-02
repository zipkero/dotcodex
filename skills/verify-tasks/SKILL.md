---
name: verify-tasks
description: >-
  Independently verify a feature implement.md against its approved spec.md and analysis.md. Use
  before implementation or when revalidating approved TASKS to assess Task coverage, boundaries,
  references, executable verification criteria, and context-dependent checkbox states.
---

# Verify Tasks

## 목적
- 현재 `implement.md`가 승인된 요구사항과 분석을 빠짐없이 실행 가능한 Task 체크리스트로 변환했는지 독립 검증한다.
- Task 작성과 승인을 분리해 `approved` 또는 `rejected` 후보와 근거만 main에 반환한다.
- 구현이나 Task 수정을 수행하지 않으며 별도 검증 결과 Markdown 문서를 만들지 않는다.

## 전제 조건과 입력
- 대상 feature에 현재 `spec.md`, `analysis.md`, `implement.md`, `README.md`가 있어야 한다.
- feature 상태판의 `SPEC`과 `ANALYSIS`가 모두 `[x]`여야 한다.
- 선행 승인이 없으면 대응하는 문서 작성과 검증 단계가 필요하다고 보고하고 중단한다. 파일 존재만으로 승인을 추정하지 않는다.
- 승인된 `spec.md`와 `analysis.md`, 현재 `implement.md`, 검증 명령의 실행 가능성을 확인하는 데 필요한
  코드·테스트·빌드·lint 설정을 직접 확인한다.
- main은 검증 맥락을 최초 승인, `tasks-init` 재작성 뒤 승인, 상위 문서 무효화 뒤 재승인,
  상위 문서 변경 없는 승인된 TASKS 자체 재검증 중 하나로 식별한다.
- 재검증이면 현재 승인 상태, Task 체크박스 상태, 승인 뒤 바뀐 상위 문서·TASKS 내용·설정 범위를 함께 확인한다.

## verifier agent 사용
- `agents/verifier.toml`의 읽기 전용 verifier에게 이 skill의 전체 경로, 대상 feature, 승인된 `spec.md`와 `analysis.md`,
  현재 `implement.md`, 코드·테스트 설정의 조사 출발점, 검증 맥락, 현재 Task 체크박스 상태와 변경 범위를 전달한다.
- verifier는 이 skill의 기준을 재정의하지 않고 원본을 독립적으로 조사해 후보 판단만 반환한다.
- main은 반환된 기준별 근거를 검토해 최종 승인·거절과 상태 전환을 결정한다.

## 판단 기준
다음 기준을 각각 독립적으로 판정한다.

1. 모든 `SPEC §5.N`이 하나 이상의 Task에 매핑되고 승인된 요구사항 밖의 범위가 추가되지 않았다.
2. 각 Task가 한 번의 `verify`로 승인 또는 거절할 수 있는 닫힌 책임 단위이다.
3. 각 `목적`이 문서 매핑이 아니라 Task 완료 후의 외부 관찰 가능한 결과를 설명한다.
4. 각 `접근`이 `analysis.md`의 확정된 설계만 실행하며 새 요구사항이나 설계 결정을 도입하지 않는다.
5. 각 `검증 조건`이 `결과`와 `확인`을 분리하고 테스트, 빌드, lint, diff, 수동 확인 등 실제 수행 가능한 근거를 지정한다.
6. 필요한 `ANALYSIS §X.Y` 참조, Task ID의 영속성, 항목 위치로 표현한 의존 순서가 일관된다.
7. 미해결 요구사항이나 미채택 설계 결정이 Task 범위, 접근 또는 검증 조건에 암묵적으로 들어오지 않았다.
8. 최초 승인, `tasks-init` 재작성 뒤 승인 또는 상위 문서 무효화 뒤 재승인이면 모든 Task가 `[ ]` 상태이다.
   승인된 상위 문서와 TASKS 내용이 바뀌지 않은 승인된 TASKS 자체 재검증이면 `[x]`와 `[ ]`를 모두 유효한 진행 상태로 보고,
   체크박스 상태만으로 거절하지 않는다.

## 검증 절차
1. 모든 `SPEC §5.N`과 `ANALYSIS §X.Y`를 Task의 목적, 접근, 검증 조건, 참조에 매핑한다.
2. 각 Task의 외부 관찰 결과와 책임 경계를 대조해 한 번의 구현 검증으로 닫히는지 확인한다.
3. 분석의 채택 설계와 Task 접근을 대조하고 새로운 경계, 상태, contract 또는 흐름 결정이 들어왔는지 확인한다.
4. 코드·테스트·빌드·lint 설정을 조사해 각 `확인`의 명령과 수동 검증이 실제 수행 가능한지 판단한다.
5. Task ID, 항목 순서, 참조와 모든 체크박스 상태를 확인하고 체크박스 기준을 식별된 검증 맥락에 맞게 적용한다.
6. 각 Criterion을 `충족`, `불충족`, `근거 부족`으로 판정한다.
   하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved` 후보로 판단한다.

검증 중에는 `implement.md`, feature 상태판, 코드, 테스트, 설정과 다른 산출물을 수정하지 않는다.

## 출력 구조
1. Status: `approved` 또는 `rejected` 후보
2. Target: 검증한 feature와 `implement.md`
3. Validation: 판단 기준과 같은 순서로 다음 내용을 적는다.
   - Criterion: 판정할 기준
   - Source: 관련 `SPEC §5.N`, `ANALYSIS §X.Y` 또는 Task 작성 contract
   - Evidence: 직접 확인한 문서, 코드, 테스트, 설정 또는 명령 결과
   - Result: `충족` | `불충족` | `근거 부족`
4. Issues: 거절 사유와 다시 필요한 `tasks-init` 또는 설계가 부족할 때 `analyze-init`; 없으면 `없음`
5. Residual Risk: 실행하지 못했거나 남은 근거 한계; 없으면 `없음`

## 상태 전환
- main은 후보 판단과 근거를 검토한 뒤에만 최종 상태를 전환한다.
- 최초 승인, `tasks-init` 재작성 뒤 승인 또는 상위 문서 무효화 뒤 재승인이 최종 `approved`이면
  feature `README.md`의 `TASKS`를 `[x]`로 바꾸고 `IMPLEMENT`는 `[ ]`로 유지한 뒤
  `- <yyyy-MM-dd>: TASKS 승인` 이력을 추가한다.
- 승인된 상위 문서와 TASKS 내용이 바뀌지 않은 승인된 TASKS 자체 재검증이 최종 `approved`이면
  `TASKS`는 `[x]`로 유지하고 현재 Task 체크박스와 `IMPLEMENT` 상태를 그대로 보존한다.
- 최종 `rejected`이면 `TASKS`를 `[ ]`로 유지하며 `implement`로 진행하지 않는다.
- 이미 승인된 TASKS의 재검증이 `rejected`이면 승인된 `SPEC`, `ANALYSIS`는 유지하고 `TASKS`, `IMPLEMENT`를
  모두 `[ ]`로 되돌리되 Task 체크박스는 후속 `tasks-init`의 비교 근거로 보존한다.
  이력에는 `- <yyyy-MM-dd>: TASKS 재검증 거절로 구현 승인 상태 초기화`를 추가한다.
- 검증 상세는 대화에 보고하며 별도 결과 Markdown 문서로 저장하지 않는다.
