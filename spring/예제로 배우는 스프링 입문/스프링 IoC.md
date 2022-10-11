## 스프링 IoC

----

##### Inversion of Control

- 일반적인 제어권: "내가 사용할 의존성은 내가 만든다."

  - ```java
    class OwnerController {
    		private OwnerRepository repository = new OwnerRepository();
    }
    ```

- IoC: "내가 사용할 의존성을 누군가 알아서 주겠지"

  - ```java
    // OwnerController는 OwnerRepository를 사용하지만 만들지는 않는다. - 의존성 관리는 OwnerController 밖에서 함
    class OwnerController {
        private OwnerRepository repo;
        public OwnerController(OwnerRepository repo) { 
        		this.repo = repo;
        }
        
    		// repo를 사용합니다. 
    }
    ```

----

##### IoC 컨테이너

- **ApplicationContext** ( Bean Factory를 상속받고 있으며 이외에도 다양한 일들을 수행한다. )
  - Bean을 만들고 엮어주며 제공해준다.
    - 기본적으로 IoC 컨테이너에 등록되어 있는 Bean들 끼리만 서로 의존성 주입이 가능하다.
  - Bean 설정 - 이름 or ID, 타입, 스코프
  - ApplicationContext가 의존성을 알아서 관리해주기 때문에 직접 호출할 일은 거의 없다.
- **싱글톤**
  - **하나의 객체를 매번 새로 만들지 않고, Application 전체에서 재사용하는 것**
  

----

##### Bean

- **스프링 IoC 컨테이너가 관리하는 객체**

  - ```java
    // Bean 객체가 아님.
    OwnerController notBean = new OwnerController();
    
    // ApplicationContext가 관리하는 Bean 객체.
    OwnerController bean = applicationContext.getBean(OwnerController.class)
    ```

- 등록 방법
  1. Component Scanning

     - @Component
       - @Repository
       - @Service
       - @Controller
       - @Configuration

     ```Java
     @SpringBootApplication 내에 @ComponentScan - 하위 패키지들의 Component를 찾아 Bean으로 등록
     ```

  2. 직접 XML이나 자바 설정 파일에 등록

     ```java
     // 자바 설정 파일에 Bean 등록 예시
     @Configuration
     public class Sample Config {
     		
     		@Bean
     		public SampleController sampleController() {
     				return new SampleController;
     		}
     }
     ```

- 호출 방법
  - @Autowired or @Inject
  - ApplicationContext - getBean()

- 특징
  - 오로지 Bean들만 의존성 주입을 해준다.

----

##### 의존성 주입 ( Dependency Injection )

- @Autowired / @Inject 사용 위치
  
  - **Ref에서 권장 방법은 생성자**
  - **생성자**
  
    - **OwnerRepository없이 OwnerController를 생성할 수 없으므로 가장 안전한 방법**
    - 순환참조가 발생할 경우에는 필드나 Setter 방법으로 처리
  
    ```java
    private OwnerRepository ownerRepository;
    
    @Autowired // Spring ver 5부터 어노테이션 생략 가능
    public OwnerController(OwnerRepository ownerRepository) {
    		this.ownerRepository = ownerRepository;
    }
    ```
  - 필드
  
    ```java
    @Autowired
    private OwnerRepository ownerRepository;
    ```
  - Setter
  
    ```java
    private OwnerRepository ownerRepository;
    
    @Autowired
    public void setOwners(OwnerRepository ownerRepository) {
    		this.ownerRepository = ownerRepository;
    }
    ```
  
    
