## 스프링 AOP

----

##### 관점 지향 프로그래밍 ( Aspect Oriented Programming )

- **어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어 보고 그 관점을 기준으로 각각 모듈화함**
- **흩어진 관심사** ( Crosscutting Concerns ) - 소스 코드 상에서 다른 부분에 계속 반복해서 쓰는 코드들
- **Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다!**

----

##### 다양한 AOP 구현 방법

1. 컴파일 A.java ---( AOP )---> A.class

2. 바이트 코드 조작 A.java -> A.class ---( AOP )---> 메모리

3. **프록시 패턴 ( Spring AOP )** 
   
   - 기존 코드 건드리지 않고 새 기능 추가
   
   > https://refactoring.guru/design-patterns/proxy
