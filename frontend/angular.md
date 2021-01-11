# Angular

## Incremental DOM vs. Virtual DOM
먼저 두 개념은 모두 상태 변화가 일어난 후 새로 렌더링을 해야할 때, 현재의 DOM에서 바뀐 부분을 찾아내고 DOM을 업데이트하는 접근 방식들이다.
Virtual DOM은 실제 DOM을 모방한 가상의 immutable한 DOM을 항상 가지고 있는데, 렌더링 때마다 새로운 가상 DOM tree를 만들고 기존에 가지고 있었던
가상 DOM tree와 비교하여 업데이트할 부분을 찾아내고 대응되는 실제 DOM을 업데이트하는 방식을 취한다.
Incremental DOM의 경우, 가상 DOM을 사용하지 않고 실제 DOM을 탐색하면서 바뀌어야 하는 부분을 찾고 mutable하게 업데이트한다.

두 방식 모두 각각 DOM 업데이트 속도와 메모리 사용량에서 trade-off가 있다.
가상 DOM을 사용할 경우 DOM에서 업데이트 해야할 최소한의 부분만을 찾아 업데이트할 수 있으므로 DOM을 수정하는데에 드는 operation 수와 시간을 줄일 수 있다
(실제 DOM을 사용하기 보다 메타 데이터 및 탐색에 효율적인 구조로 구성된 가상 DOM으로 비교하기 때문에 차이를 더 빠르게 찾을 수도 있을 것이다).
반대로, Incremental DOM에서와 같이 실제 DOM을 사용할 경우 업데이트할 DOM을 정확하고 빠르게 찾기는 좀 더 어려워지만 가상 DOM과 같은
추가적인 데이터구조를 사용하지 않기 때문에 메모리를 아낄 수 있다.
따라서, virtual DOM은 DOM의 업데이트가 자주 일어나 렌더링이 잦은 어플리케이션에 적합하고, incremental DOM은 메모리 용량이 작은 모바일 기기 환경에 좀 더 적합하다.

### 참고
- [What is Virtual Dom? And Why is it faster?](https://dev.to/karthikraja34/what-is-virtual-dom-and-why-is-it-faster-14p9)
- [Understanding Angular Ivy: Incremental DOM and Virtual DOM](https://blog.nrwl.io/understanding-angular-ivy-incremental-dom-and-virtual-dom-243be844bf36)
- [Incremental DOM 101: What is it and why I should care?](https://auth0.com/blog/incremental-dom/)
