# C# 작업 기준

## 적용 범위
- 이 문서는 전역 기본값이다. 프로젝트의 `AGENTS.md`, `.editorconfig`, analyzer, formatter, test 설정이 있으면 그 기준을 우선한다.
- C# 파일을 수정하거나 검토할 때 적용한다.

## 코드 작성
- nullable reference type 설정을 확인하고, null 가능성은 타입과 검사 조건으로 명확히 표현한다.
- 공개 API 규약 변경은 호출부와 테스트 영향을 함께 확인한다.
- LINQ는 가독성이 유지되는 범위에서 사용하고, 중첩으로 의도가 흐려지면 명시적 흐름을 우선한다.
- DI, options, logging, configuration 패턴은 기존 프로젝트 관례를 따른다.

## 오류와 비동기
- 예외는 실패 상황 표현에 사용하고, catch 후 근거 없이 삼키지 않는다.
- 비동기 API를 추가하거나 수정할 때는 `CancellationToken` 전달 경로를 유지할 수 있는지 확인한다.
- `ConfigureAwait`, 동기화 컨텍스트, 백그라운드 작업 처리 방식은 기존 프로젝트 관례를 따른다.
- `IDisposable` 또는 `IAsyncDisposable` 소유권이 생기면 해제 책임을 명확히 한다.

## 테스트
- 기존 테스트 프레임워크와 assertion 스타일을 따른다.
- 프로젝트 관례가 없고 C# 코드 동작을 바꿨다면 `dotnet test`를 우선 검증 명령으로 고려한다.
