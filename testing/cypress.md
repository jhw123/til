# Cypress

## TypeScript 사용시 custom command 추가하기
`Cypress.Commands.add()`를 통해 `cy`에서 바로 사용하거나 `Cypress.Chainable` 뒤에 체인닝해 사용할 수 있는 커스텀 커맨드를 사용할 수 있다.
plain javascript를 사용할 경우 `add` 이외에 별도로 해줄 것이 없지만, typescript를 사용할 경우 커스텀 커맨드가 추가되는 건 런타임에서이므로,
커맨드의 타입 정보를 따로 명시해주어야 한다.
Cypress의 타입 정보는 `Cypress`라는 namespace 아래에 모두 정의되어 있는데 타입 정보를 추가하기 위해선 이 namespace를 확장해야 한다.
namespace는 라이브러리 파일을 건드리지 않고 분리된 파일을 새롭게 만들어 쉽게 확장이 가능하다.
`global.d.ts`라는 파일을 하나 새로 만들고 여기에 커스텀 커맨드의 타입을 추가한다.
커맨드 정의와 타입 정의를 같은 파일에서 관리할 수도 있겠으나, 이는 추천하지 않는 방식이다.
그 이유는 타입스크립트의 동작 상 namespace를 확장하기 위해서는 해당 파일에 `import`/`export`를 사용하면 안 된다
(`import`/`export`를 사용할 경우 모듈 파일로 간주하기 때문).
따라서 커맨드 타입을 확장하는 파일에서는 최대한 간단하게 타입만 명시하고,
실제 커맨드를 구현하는 파일에서는 `import`를 자유롭게 사용해 type-safe하게 코드를 작성하는 것이 좋다.

```typescript
/// <reference type="Cypress">
// 이 파일에서는 import/export를 사용해서는 안 된다.
namespace Cypress {
    interface Chainable<Subject> {
        customCommand(): Chainable<Subject>
    }
}
```

### 참고
- [Github Issue - Adding custom commands with typescript definitions leads to error](https://github.com/cypress-io/cypress/issues/1065)
