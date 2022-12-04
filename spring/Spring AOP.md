## Spring AOP

----

```java
// 어노테이션 사용위치 정의: 메서드 및 타입 선언 시 적용
@Target({ ElementType.METHOD, ElementType.TYPE }) 
// Annotation이 실제로 적용되고 유지되는 범위 지정 (RUNTIME(리플렉션 로깅 등에 사용), CLASS, SOURCE)
@Retention(RetentionPolicy.RUNTIME) 
// Annotation이 자손 클래스에도 상속되도록 함
@Inherited
public @interface AnnotationName {
}
```

```java
@Getter
@RequiredArgsConstructor
public enum AdminAuthorityType {

    USER("일반 유저", 0b00000001),
    MANAGER("Manager", 0b00000011),
    SYSTEM_ADMIN("시스템 어드민", 0b00000111);

    private final String description;
    private final int authority;

  	// 논리 연산자를 통한 권한의 포함관계 확인
    public boolean isPermitted(AdminAuthorityType type){
        int i = this.authority & type.authority;
        return i == type.authority;
    }
}
```

```java
@Aspect // 해당 Class가 횡단 관심사(부가기능) Class임을 명시
@Component
@Slf4j
public class AuthorizeAspect {

  	// Advide(@Around): 지정된 패턴에 해당하는 메소드의 실행되기 전, 실행된 후 모두에서 동작한다.
  	// Point cut: 실제 advice가 적용될 메서드를 선정
    @Around("@annotation(com.koba.admin.api.annotation.Authorize)") 
    public Object checkAuthorize(ProceedingJoinPoint joinPoint) throws Throwable {
      
        // getSignature(): 클라이언트가 호출한 메소드의 시그니처 (리턴타입, 이름, 매개변수) 정보가 저장된 Signature 객체를 리턴
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();
        Authorize annotation = method.getAnnotation(Authorize.class);

        // method의 annotation을 통해 요청자의 AdminAuthorityType을 받아와서 권한 체크!
        AdminAuthorityType minAuthority = annotation.value();
        if (!SecurityHolder.getAuthority().isPermitted(minAuthority)) {
            throw new ApiException(AdminApiStatus.NOT_PERMITTED_LOW_LEVEL);
        }
        // 원래 비지니스 메서드를 실행
        return joinPoint.proceed();
    }
}
/** 
		결론: 스프링 AOP 개념을 적용하여 횡단 관심사를 모아 공통 로직을 처리 (위에서는 권한 체크)
    Reflection API: 컴파일 타임에 어떤 클래스의 인스턴스가 실행될지 알 수 없는 경우에 런타임에서 클래스 정보를 분석하고 실행할 수 있도록 할 때 사용
**/
```

- Advice 종류

  - **@Around** : 메서드 호출 전후에 수행. 가장 강력한 어드바이저. 조인 포인트 실행여부, 반환 값 변환, 예외 변환 등 가능
    - JointPoint를 개발자가 직접 실행 해주어야 함.

  - @Before : 조인 포인트 실행 이전에 실행. 조인 포인트는 자동 실행. 예외 발생 시, 다음 코드는 호출 X

  - @AfterReturning : 조인 포인트 정상 실행 완료 직후에 실행. 조인 포인트 값은 자동으로 리턴됨. 반환되는 값을 받을 수 있다.

  - @AfterThrowing : 조인 포인트가 예외를 던진 직후 실행. 예외는 자동으로 던짐. 반환되는 예외를 받을 수 있다.

  - @After : 조인 포인트가 정상, 예외 실행 완료되면 실행.