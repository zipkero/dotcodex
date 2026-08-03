---
name: verify-analysis
description: >-
  Independently verify a feature analysis.md against its approved spec.md, project sources,
  implementation code, and configuration. Use before implement-init or when revalidating an
  approved ANALYSIS.
---

# Verify Analysis

## 목적
- 현재 `analysis.md`가 `spec.md` 전체를 실제 구현 구조에 맞는 설계 기준으로 충분히 구체화했는지 독립 검증한다.
- 원본 코드·설정을 직접 조사하고 하나라도 불충분하거나 근거가 부족하면 거절한다.
- 분석 작성과 승인을 분리해 `approved` 또는 `rejected` 후보와 근거만 main에 반환한다.
- 구현 Task를 작성하거나 별도 검증 결과 Markdown 문서를 만들지 않는다.

## 전제 조건과 입력
- 대상 기능에 `README.md`와 현재 `spec.md`가 있어야 하며 상태판의 `SPEC`이 `[x]`여야 한다.
- `README.md` 또는 `spec.md`가 없거나 `SPEC`이 `[ ]`이면 `spec-init`이 필요하다고 보고하고 중단한다.
- `README.md`와 승인된 `spec.md`가 있지만 `analysis.md`가 없으면 `analyze-init`이 필요하다고 보고하고 중단한다.
- `spec.md`, 현재 `analysis.md`, 관련 프로젝트 기준 문서와 실제 구현 코드·테스트·설정을 직접 확인한다.
- `spec.md`의 입력 맥락과 `analysis.md`의 근거는 조사 출발점으로만 사용하고 작성자의 요약을 원본 근거로 대신하지 않는다.
- 재검증이면 현재 승인 상태와 승인 뒤 바뀐 문서, 코드, 설정의 범위를 함께 확인한다.

## verifier agent 사용
- `agents/verifier.toml`의 읽기 전용 verifier에게 이 skill의 전체 경로, 대상 기능, `spec.md`와 현재 `analysis.md`,
  프로젝트 기준 문서·코드·테스트·설정의 조사 출발점, 재검증 상태와 변경 범위를 전달한다.
- verifier는 이 skill의 기준을 재정의하지 않고 원본을 독립적으로 조사해 후보 판단만 반환한다.
- main은 반환된 기준별 근거를 검토해 최종 승인·거절과 상태 전환을 결정한다.

## 판단 기준
다음 기준을 각각 독립적으로 판정한다.

1. 모든 `SPEC §5.N`이 관련 설계 본문에 추적되며 요구사항이 추가·누락·약화되지 않았다.
2. 구조의 책임 경계, 데이터 소유권, 호출 방향과 실패 처리 위치가 실제 코드·설정에 맞게 설명됐다.
3. 진입점부터 산출물까지의 정상 흐름, 상태 전이, 외부 연동과 실패 경로가 구현 Task를 만들 수 있을 만큼 확정됐다.
4. 경계를 가로지르는 인터페이스와 유지해야 할 규약이 빠짐없이 정의됐다.
5. 실제 변경 대상과 원본 조사로 확인한 의존 관계가 영향 범위에 포함됐다.
6. 주요 Decision Point마다 채택안, 근거, 배제한 주요 대안과 장단점이 다음 단계에서 재결정할 필요 없게 남아 있다.
7. 확인 사실과 추정이 구분되고 미채택 결정이나 사용자 확인 사항이 다음 단계로 유실되지 않았다.
8. 새 세션에서 `spec.md`와 `analysis.md`만 읽고 설계를 새로 결정하지 않은 채 Task 체크리스트를 작성할 수 있다.

## 검증 절차
1. 모든 `SPEC §5.N`과 분석의 구조, 데이터 흐름, 인터페이스, 영향 범위, Decision Points를 기준 목록으로 만든다.
2. 관련 프로젝트 기준 문서와 실제 코드·테스트·설정을 조사해 현재 책임 경계, 흐름, 규약과 의존 관계를 확인한다.
   분석 본문의 주장만으로 사실을 확정하지 않는다.
3. 기준마다 출처와 독립적으로 판정 가능한 Criterion을 만들고 직접 확인한 원본을 Evidence로 연결한다.
4. 기준마다 누락, 모순, 근거 없는 설계 확정과 다음 단계에서 새로 결정해야 하는 사항이 있는지 확인한다.
5. 각 Criterion을 `충족`, `불충족`, `근거 부족`으로 판정한다.
   하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved` 후보로 판단한다.

검증 중에는 `analysis.md`, 기능 상태판, 코드, 테스트, 설정과 다른 산출물을 수정하지 않는다.

## 출력 구조
1. Status: `approved` 또는 `rejected` 후보
2. Target: 검증한 기능과 `analysis.md`
3. Validation: 판단 기준과 같은 순서로 다음 내용을 적는다.
   - Criterion: 판정할 기준
   - Source: 관련 `SPEC §5.N`, 분석 섹션 또는 프로젝트 규약
   - Evidence: 직접 확인한 문서, 코드, 테스트, 설정 또는 명령 결과
   - Result: `충족` | `불충족` | `근거 부족`
4. Issues: 거절 사유와 가장 이른 수정 소유 단계(`analyze-init`, 승인된 요구사항 변경은 `spec-init`); 없으면 `없음`
5. Residual Risk: 실행하지 못했거나 남은 근거 한계; 없으면 `없음`

## 상태 전환
- main은 후보 판단과 근거를 검토한 뒤에만 최종 상태를 전환한다.
- 최초 검증이 최종 `approved`이면 기능 `README.md`의 `ANALYSIS`만 `[x]`로 바꾸고
  `- <yyyy-MM-dd>: ANALYSIS 승인` 이력을 추가한다.
- 승인된 ANALYSIS를 내용 변경 없이 재검증해 `approved`이면 현재 Task 체크박스와 `IMPLEMENT` 상태를 보존한다.
- 승인 상태를 초기화하지 않은 채 `analysis.md` 내용이 바뀐 재검증이 `approved`이면 `ANALYSIS`는 `[x]`로 유지하고
  `IMPLEMENT`와 기존 Task 체크박스를 `[ ]`로 되돌린 뒤 `- <yyyy-MM-dd>: ANALYSIS 변경 재승인으로 구현 상태 초기화` 이력을 추가한다.
- 최종 `rejected`이면 `ANALYSIS`를 `[ ]`로 유지하며 `implement-init`으로 진행하지 않는다.
- 이미 승인된 ANALYSIS의 재검증이 `rejected`이면 `SPEC`은 유지하고 `ANALYSIS`, `IMPLEMENT`를 `[ ]`로 되돌린 뒤
  `- <yyyy-MM-dd>: ANALYSIS 재검증 거절로 구현 승인 상태 초기화` 이력을 추가한다.
- 이 재검증 거절 시 기존 `implement.md`의 파일과 각 Task의 내용·ID·순서는 보존하고 체크박스만 모두 `[ ]`로 바꾼다.
- 검증 상세는 대화에 보고하며 별도 결과 Markdown 문서로 저장하지 않는다.
