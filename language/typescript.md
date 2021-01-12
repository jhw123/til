# Typescript

## Namespace
Namespace(구: internal module)와 module은 모두 코드를 관심사 별로 독립된 파일에 분리하여 작성하고 합칠 수 있게 돕는 기능이다.
Namespace는 `interface`, `class`, 또는 변수 정의 등을 하나의 이름 아래로 전역으로 묶어준다.
Namespace를 사용하는 쪽(consumer)에서는 별도의 `import` 문 없이 해당 네임스페이스 아래 export된 값들을 사용할 수 있다.
이렇게 여러 종류의 정의들을 하나로 묶을 수 있기 때문에 보다 넓은 스코프 단위로 코드를 정리하고 마치 클래스의 `static`을 사용하는 것처럼 가독성을 높일 수 있다.

```typescript
// electronics.ts
namespace Electronics {
    export interface Phone { ... }
    export class IPhone extends Phone { ... }
    export const myPhone = new IPhone()
}
```

또한 namespace는 module과 다르게 여러 파일에 작성된 정의들이 하나의 네임스페이스로 합쳐지도록 코드를 구성할 수 있다.
다만, 이 경우 컴파일러에게 파일 간 종속 관계를 알려주기 위해 `/// <reference path="파일명.ts">`를 가장 위에 넣어주고
컴파일러의 `--outFile` 플래그를 통해 하나의 js 파일로 컴파일 되도록 하거나,
namespace가 사용되는 쪽에서 각각의 파일을 `<script>` 태그를 통해 로드시켜주어야 한다.

```typescript
// samsung.ts
/// <reference path="electronics.ts" />
namespace Electronics {
    export class Samsung { ... }
}
```

공식 문서에서도 namespace보다 module를 사용하길 더 권장하지만 namespace가 필요한 경우가 있다.
모듈이 아닌 `<script>`로 로드되는 라이브러리의 경우인데, 이러한 라이브러리는 보통 전역 객체 하나를 사용하고 
해당 객체의 필드 및 타입 정보를 namespace로 정의하여 타입핑을 지원한다.

### 참고
- [Namespaces and Modules - TypeScript](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)
- [Namespaces - TypeScript](https://www.typescriptlang.org/docs/handbook/namespaces.html)
