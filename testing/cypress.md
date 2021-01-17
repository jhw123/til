# Cypress

## Cypress 커맨드는 비동기로 동작한다
Cypress는 테스트 대상 어플리케이션이 실행되는 브라우저와 자바스크립트 스레드를 공유한다.
그렇기에 만약 테스트 커맨드가 동기적으로 불린다면, 단일 스레드에서 실행되는 자바스크립트 언어 특성 상 어플리케이션의 
비동기처리가 블록킹 되어 제대로 된 테스트를 진행할 수 없다.
예를 들어 클릭했을 때 서버 api 호출하는지 버튼이 있고, 아래와 같이 서버 api가 호출될 때까지 클릭하는 테스트를 한다고 하자.
`click()` 커맨드는 버튼의 `onclick`을 트리거하는데, 이 커맨드가 동기적으로 실행된다면 
`onclick`을 트리거 시키고 바로 다음으로 넘어가기 때문에 계속해서 `while`문을 실행해 비동기인 서버 api 처리가 실행되지 않을 것이다.

```typescript
while ( 서버 api가 호출되지 않음 ) {
    cy.findByRole('button').click()
}
```

자바스크립트의 이런 특성에 대응하기 위해 Cypress는 테스트 커맨드를 비동기적으로 실행시켜 테스트하는 어플리케이션의 동작을 막지 않도록 설계되었다.
Cypress는 테스트 커맨드를 정의된 순서대로 큐에 넣고 브라우저가 idle 상태일 때 큐에서 커맨드를 꺼내 테스트를 진행한다.
따라서 `promise`를 사용한 비동기 처리는 서로 순서가 보장되지 않는 반면 Cypress는 순서를 보장한다.

### 참고
- [Understanding Cypress’s command execution order and Chainables](https://medium.com/@arnonaxelrod/understanding-cypresss-command-execution-order-and-chainables-75079d82710a)

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
