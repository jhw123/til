# Deploy

## Rolling, Blue/green, Canary 배포 방식

서버 배포 방식에는 3가지의 방식이 있는데, 새로운 버전으로 넘어가는 속도에서의 차이와 사용되는 최대 인스턴스의 갯수에 서로 차이가 있다.
먼저 롤링(Rolling) 배포는 인스턴스를 하나씩 꺼가며 새로운 버전으로 배포하는 방식이다. 
점진적으로 배포되는 방식으로 직관적이고 관리가 쉽지만, 배포 과정에서 두 버전의 서버가 서로 인스턴스를 나눠가지므로 서버 처리 용량이 부족해질 수 있다.
다음은 블루/그린(Blue/green) 배포로 이전 서버를 그대로 놔둔채 새로운 서버를 미리 모두 띄워놓고, 로드 밸런서를 통해 한번에 새로운 서버로 갈아타는 방식이다.
점진적 배포가 아니기 때문에 빠르게 다음 버전으로 스위칭할 수 있고, 이전 버전으로 롤백하기도 쉽다.
하지만 두 버전의 서버 인스턴스를 처리 용량을 줄이지 않은 채로 모두 띄워두기 때문에 실제 사용되는 인스턴스 보다 많이 띄울 수 있어야 가능한 방식이다.
마지막으로 카나리(Canary) 배포는 롤링과 비슷하게 점진적으로 배포를 하지만 로드 밸런서를 통해 두 버전의 서버에 전달되는 트래픽을 좀 더 세심하게 조절한다.
예를 들어, 10%의 트래픽을 새 버전의 서버에 전달해 서버가 정상적으로 동작하는지 production 환경에서 테스트할 수도 있으며, 
특정 유저층을 걸러내어 A/B 테스트를 진행할 수도 있다.
상황에 따른 자유도와 컨트롤이 더 주어진만큼 로드 밸런서를 추가로 설정해야 한다는 단점이 있다.

### 참고
- [배포 전략의 종류(롤링/카나리 배포/블루 그린)](https://reference-m1.tistory.com/211)
