# React

## Portal
리액트의 컴포넌트 트리는 `<div id="root">` 내에 만들어 지는데 상황에 따라 외부에 컴포넌트를 만들고 싶을 때가 있다.
예를 들어 버튼을 누르면 자체적인 팝업 모달이 뜨는 아래와 같은 컴포넌트가 있다고 하자.
이 컴포넌트를 컴포넌트 트리의 아무 곳에 위치시키려면 문제가 하나 있는데, 
부모 컴포넌트가 어떻게 스타일링되어 있는지 모르므로 (e.g. position, z-index) Custom popup이 제대로 동작하는 걸 보장하기 힘들다.

```typescript jsx
const ModalBtn: FC = () => {
    const [visible, setVisible] = useState(false)
    ...
    return (
        <>
            <button onClick={() => setVisible(true)}>Show popup</button>
            <div style={{visibility: visible}}>Custom popup</div>
        </>
    )
}
```

이럴 때 Custom popup을 외부의 고정된 위치, 즉 DOM 트리의 최상단에 위치시키면 해당 컴포넌트가 어디 위치해있는지에 상관없이 일관된 동작을 보장할 수 있다.
`react-dom`의 `createPortal()` API는 DOM의 어느 곳이든 컴포넌트가 이동할 수 있게 해주면서도 이동시킨 컴포넌트가 react 트리에는 종속하게 만들어
이벤트 핸들링이나 상태변화 감지를 정상적으로 동작하게 해준다.

아래는 위의 예시에 포탈 API를 적용시킨 예이다.
범용적인 `Portal` 컴포넌트를 만들어 포탈 API를 이용해 고정된 DOM 위치로 이동시키는 로직을 좀 더 쉽게 재사용할 수 있게 하였다.

```typescript jsx
import ReactDom from 'react-dom'

const Portal: FC = props => {
    return ReactDom.createPortal(
        props.children,
        document.getElementById('root')
    )
}

const ModalBtn: FC = () => {
    ...
    return (
        <>
            <button onClick={() => setVisible(true)}>Show popup</button>
            <Portal>
                <div style={{visibility: visible}}>Custom popup</div>
            </Portal>
        </>
    )
}
```

### 참고
- [Portals - React](https://reactjs.org/docs/portals.html#gatsby-focus-wrapper)
