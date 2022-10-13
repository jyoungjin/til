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
   
   ```java
   // 사용 시점엔 Payment를 사용하지만 CreditCard가 조건 별로 Cash를 사용할지 Card를 사용할지 분기시켜준다.
   // 기존 코드 건드리지 않고 프록시 패턴으로 새 기능 추가하기
   public class CreditCard implements Payment{
   
   	Payment cash = new Cash();
   
   	@Override
   	public void pay(int amount) {
   		if (amount > 100) {
   			System.out.printf(amount + " 신용 카드");
   		} else {
   			cash.pay(amount);
   		}
   	}
   }	
   ```

----

##### AOP 적용 예제

```java
// @LogExecutionTime 으로 메소드 처리 시간 로깅하기
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExcutionTime {
}
```



```java
// 실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)
@Component
@Aspect
public class LogAspect {
    Logger logger = LoggerFactory.getLogger(LogAspect.class);
    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
          StopWatch stopWatch = new StopWatch(); 
          stopWatch.start();
          Object proceed = joinPoint.proceed(); stopWatch.stop();
          logger.info(stopWatch.prettyPrint()); return proceed;
    } 
}
```

