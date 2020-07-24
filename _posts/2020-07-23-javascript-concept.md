# javascript basic concept
해당 글은 [자바스크립트 개발자라면 알아야 할 33가지 개념 포스팅](https://medium.com/@gaurav.pandvia)을 요약한것입니다. 원문 번역은 [서진규님의 포스팅](https://velog.io/@jakeseo_me/2019-03-15-2303-%EC%9E%91%EC%84%B1%EB%90%A8-rmjta5a3xh)을 참고하면 좋습니다.


---
## [Understanding Javascript Function Executions](https://velog.io/@jakeseo_me/2019-03-15-2303-%EC%9E%91%EC%84%B1%EB%90%A8-rmjta5a3xh)

### Call stack
![callstack-gif](https://media.vlpt.us/post-images/jakeseo_me/fc418e50-456c-11e9-83dd-8359947fc569/callstack.gif)
실행해야할 함수를 담는 Stack.모든 프로그램은 가장 먼저 main()함수가 push되고 이후 호출 순서에따라 그위에 push된다. pop은 최근 함수부터 진행된다.

### Heap
![힙 이미지](https://t1.daumcdn.net/cfile/tistory/257CE33C53AB84BE35)
동적으로 런타임에 할당되는 메모리 영역으로, 변수와 객체들의 메모리 할당이 일어남

### 