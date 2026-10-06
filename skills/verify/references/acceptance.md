# 구현 승인 판정 계약

## 기준
- Per-Request는 사용자 요청, 변경 diff와 관련 실행 결과를 기준으로 한다.
- Phased는 `~/.codex/skills/verify/references/phased.md`의 기준을 함께 적용한다.
- 판정 대상이 만들거나 보존해야 하는 실제 동작과 이번 승인으로 완료되는 요구사항만 판정한다.

## 절차
1. 원본 기준에서 독립적으로 판정할 기준과 출처를 정한다.
2. 각 기준을 실제 변경과 현재 코드 상태에 대조한다.
3. 기준에 대응하는 실행 결과나 확인 가능한 산출물을 연결한다. 기존 근거는 명령·cwd·환경·대상 코드와 미커밋 diff가 현재 상태에 대응할 때만 재사용한다.
4. 각 기준을 `충족`, `불충족`, `근거 부족`으로 판정한다.

하나라도 `불충족` 또는 `근거 부족`이면 `rejected`, 모두 `충족`이면 `approved`다.

## 출력
출력은 아래 모양이다. Phased는 `references/phased.md`의 표시 줄 다음에 둔다.
`<…>`는 채울 자리, `|`는 그중 하나이며, 그 밖의 글자는 적힌 그대로 쓴다. 번호 항목은 들여쓰지 않는다.

```markdown
1. Status: `approved` | `rejected`
2. Target: task-<nnn>: <제목>
3. Completed requirements:
   - SPEC §5.<N>: `성립` | `불성립` | `미확인` — <근거>
4. Issues:
   - Category: `style/minor` | `correctness` | `design/scope`
   - Repair stage: `구현` | `Task` | `설계` | `spec`
   - Problem: <문제와 근거>
5. Validation:
   - Criterion: <기준>
     Source: <출처>
     Evidence: <근거>
     Result: `충족` | `불충족` | `근거 부족`
6. Explanation: <승인 근거와 남은 위험>
```

`Category`가 `evidence`면 4번은 아래 모양이다.

```markdown
4. Issues:
   - Category: `evidence`
   - Resolution: <필요한 입력·환경·재검증 조건>
   - Problem: <문제와 근거>
```

- 3번은 Phased에서 이번 승인으로 완료되는 적용 중인 `SPEC §5.N`마다 한 줄이다. 그 요구사항에 걸린 기준이 `불충족`이면 `불성립`, `불충족` 없이 `근거 부족`이면 `미확인`이다. 없으면 3번은 `3. Completed requirements: 없음` 한 줄이다.
- 4번은 `rejected`일 때, 6번은 `approved`일 때만 둔다.
- 문제가 여럿이어도 `Category`와 `Repair stage`는 하나씩이다. `Category`는 확인된 문제가 있으면 그 값이고 근거 부족은 `Problem`에 적으며, `Repair stage`는 고칠 자리 중 가장 앞선 단계다.
- 5번은 이번 판정 대상의 기준마다 한 묶음이다. Phased 판정 범위와 요구사항 전체 판정 시점은 `references/phased.md`를 따른다.
- `Category` 값의 뜻:
  - `style/minor`: 정확성을 깨지 않는 이름·주석·포맷 관례 위반
  - `correctness`: 완료 조건·Task 목적·검증 조건 불충족, 버그, 잘못된 출력
  - `design/scope`: 설계 결정 이탈, 범위 초과·미달
  - `evidence`: 불충족을 확인한 것이 아니라 성립 여부를 확인할 근거가 없음
- Target의 Task ID 형식은 Phased에만 쓰고, Per-Request는 요청한 변경을 적는다.

Per-Request 결과 검토는 같은 판정 기준으로 결과·핵심 근거·미실행 검증만 간결하게 보고할 수 있다.
판정 과정은 읽기 전용이다. 별도 검증 Markdown은 만들지 않는다.
