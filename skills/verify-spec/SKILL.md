---
name: verify-spec
description: >-
  Independently verify a feature spec.md against the originating request, confirmed decisions,
  project sources, and observable completion criteria. Use before analyze-init or when
  revalidating an approved SPEC.
---

# Verify Spec

## 목적
- 현재 `spec.md`가 사용자 요청과 확정 판단을 빠짐없이 요구사항 기준 문서로 고정했는지 독립 검증한다.
- 요구사항 승인과 작성 책임을 분리하고, `approved` 또는 `rejected` 후보와 근거를 main에 반환한다.
- 설계나 구현을 검증하지 않으며 별도 검증 결과 Markdown 문서를 만들지 않는다.

## 전제 조건과 입력
- 대상 feature의 현재 `spec.md`와 `README.md`가 있어야 한다.
- 원본 사용자 요청과 확정 판단, `spec.md`가 참조한 프로젝트 기준 문서, 필요한 현재 코드·설정을 직접 확인한다.
- 프로젝트 기준 문서는 존재하는 루트 `README.md`, `ROADMAP.md`, `docs/product.md`, `docs/design.md`에서
  feature와 관련된 범위만 조사한다.
- 재검증이면 현재 승인 상태와 승인 뒤 변경된 `spec.md` 또는 원본 근거의 범위도 확인한다.
- 원본 요청이나 판단에 필수인 근거가 없어 요구사항의 정합성을 판단할 수 없으면 승인으로 추정하지 않는다.

## verifier agent 사용
- `agents/verifier.toml`의 읽기 전용 verifier에게 이 skill의 전체 경로, 대상 feature와 `spec.md`, 원본 요청과 확정 판단,
  조사 출발점인 프로젝트 문서·코드·설정, 재검증 상태와 변경 범위를 전달한다.
- verifier는 이 skill의 기준을 재정의하지 않고 원본을 독립적으로 조사해 후보 판단만 반환한다.
- main은 반환된 기준별 근거를 검토해 최종 승인·거절과 상태 전환을 결정한다.

## 판단 기준
다음 기준을 각각 독립적으로 판정한다.

1. 사용자 요청과 확정 판단이 누락·추가·약화되지 않았다.
2. 프로젝트 기준 문서와 조사된 현재 동작에 근거 없이 충돌하지 않는다.
3. 범위, 목표, 제약, 제외 범위와 완료 조건의 경계가 서로 모순되지 않는다.
4. 각 `SPEC §5.N`이 외부에서 관찰 가능하고 하위 문서에서 영구 식별자로 참조할 수 있다.
5. 설계, 구현 순서나 Task 분해가 요구사항으로 잘못 확정되지 않았다.
6. 승인 전에 해소해야 할 feature 고유 판단이나 필수 근거 부족이 남아 있지 않다.

## 검증 절차
1. 판단 기준마다 출처와 독립적으로 판정 가능한 Criterion을 만든다.
2. 원본 요청과 확정 판단을 `spec.md`의 범위, 목표, 제약, 제외 범위, 완료 조건과 대조한다.
3. `spec.md`가 인용한 프로젝트 문서와 필요한 코드·설정을 직접 조사한다.
   작성자의 요약이나 대화 기억만으로 근거를 대체하지 않는다.
4. 각 Criterion에 확인한 문서, 코드, 설정 또는 명령 결과를 Evidence로 연결한다.
5. 각 Criterion을 `충족`, `불충족`, `근거 부족`으로 판정한다.
   하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved` 후보로 판단한다.

검증 중에는 `spec.md`, feature 상태판, 코드, 설정과 다른 산출물을 수정하지 않는다.

## 출력 구조
1. Status: `approved` 또는 `rejected` 후보
2. Target: 검증한 feature와 `spec.md`
3. Validation: 판단 기준과 같은 순서로 다음 내용을 적는다.
   - Criterion: 판정할 기준
   - Source: 기준의 출처
   - Evidence: 직접 확인한 파일, 코드, 설정 또는 명령 결과
   - Result: `충족` | `불충족` | `근거 부족`
4. Issues: 거절 사유와 가장 이른 수정 소유 단계인 `spec-init`; 없으면 `없음`
5. Residual Risk: 실행하지 못했거나 남은 근거 한계; 없으면 `없음`

## 상태 전환
- main은 후보 판단과 근거를 검토한 뒤에만 최종 상태를 전환한다.
- 최종 `approved`이면 feature `README.md`의 `SPEC`만 `[x]`로 바꾸고
  `- <yyyy-MM-dd>: SPEC 승인` 이력을 추가한다.
- 최종 `rejected`이면 `SPEC`을 `[ ]`로 유지하며 `analyze-init`으로 진행하지 않는다.
- 이미 승인된 SPEC의 재검증이 `rejected`이면 `SPEC`, `ANALYSIS`, `TASKS`, `IMPLEMENT`를 모두 `[ ]`로 되돌리고
  `- <yyyy-MM-dd>: SPEC 재검증 거절로 하위 승인 상태 초기화` 이력을 추가한다.
- 이 재검증 거절 시 기존 `implement.md`가 있으면 파일과 각 Task의 내용·ID·순서는 보존하고,
  Task 항목의 체크박스만 모두 `[ ]`로 바꾼다.
- 검증 상세는 대화에 보고하며 별도 결과 Markdown 문서로 저장하지 않는다.
