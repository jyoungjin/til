## Swagger

----

##### Swagger

-  개발한 REST API를 편리하게 문서화 해주고, 이를 통해서 편리하게 API를 호출해보고 테스트할 수 있는 프로젝트



##### Swagger 구성

- **SpringFox Swagger 2**는 적용된 프로젝트의 응답, 요청, 예시 등의 정보를 JSON 쌍으로 만들어준다.
- **SpringFox UI**는 Swagger 2를 통해 만든 JSON 쌍을 상호작용하기 좋은 웹페이지로 만들어준다.
- **SpringFox Bean Validator**를 적용할 경우, Swagger가 문서화를 진행하면서 Bean Validation Annotation이 적용된 내용에 대한 정보를 추가로 저장한다.
- **SpringFox Data REST**는 @Entity와 @Repository를 찾아 엔티티에 대한 사양 정보를 추가로 문서화한다.



##### Swagger 세팅

```java
// https://mvnrepository.com/artifact/io.springfox/springfox-boot-starter
implementation group: 'io.springfox', name: 'springfox-boot-starter', version: '3.0.0'
```

- 만약 Spring 실행 시,  Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException 에러가 발생한다면?

- ```java
  // application.properties 파일에 추가
  spring.mvc.pathmatch.matching-strategy=ant_path_matcher
  ```



##### Swagger 적용

- 프로그램을 실행한 뒤, uri 뒤에 **"/swagger-ui/"**를 붙여 실행하면 해당 문서를 확인할 수 있다.

- 주소 끝에 "/"를 붙이지 않으면 404 Error가 발생할 수 있다.

- SwaggerConfig

  - 메인 페이지의 이름과 설명을 추가

  - ````java
    package com.onj.weldbeing.config;
    
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import springfox.documentation.builders.ApiInfoBuilder;
    import springfox.documentation.builders.PathSelectors;
    import springfox.documentation.builders.RequestHandlerSelectors;
    import springfox.documentation.service.ApiInfo;
    import springfox.documentation.service.Contact;
    import springfox.documentation.spi.DocumentationType;
    import springfox.documentation.spring.web.plugins.Docket;
    import springfox.documentation.swagger2.annotations.EnableSwagger2;
    
    @Configuration
    @EnableSwagger2
    public class SwaggerConfig {
    
        private static final String API_NAME = "WELDBEING Project API";
        private static final String API_VERSION = "0.0.1";
        private static final String API_DESCRIPTION = "WELDBEING 프로젝트 명세서";
    
        @Bean
        public Docket api() {
            return new Docket(DocumentationType.SWAGGER_2)
                    .apiInfo(apiInfo())
                    .select()
                    .apis(RequestHandlerSelectors.any())
                    .paths(PathSelectors.any())
                    .build();
        }
    
        public ApiInfo apiInfo() {
            return new ApiInfoBuilder()
                    .title(API_NAME)
                    .version(API_VERSION)
                    .description(API_DESCRIPTION)
                    .contact(new Contact("ONJ", "https://weldbeing.ai/", "youngjin.jang@weldbeing.ai\n"))
                    .build();
        }
    
    }
    ````



##### Swagger 주요 어노테이션 목록

| Annotation                           | 설명                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| @Api                                 | 클래스를 스웨거의 리소스로 표시                              |
| @ApiOperation                        | 특정 경로의 오퍼레이션 HTTP 메소드 설명                      |
| @ApiParam                            | 오퍼레이션 파라미터에 메타 데이터 설명                       |
| @ApiResponse                         | 오퍼레이션의 응답 지정                                       |
| @ApiModelProperty                    | 모델의 속성 데이터를 설명                                    |
| @ApiImplicitParam/@ApiImplicitParams | 메소드 단위의 오퍼레이션 파라미터를 설명. <br />@ApiImplicitParams 내에 @ApiImplicitParam를 포함함 |
| @ApiIgnore                           | 메소드나 컨트롤러를 스웨거 문서화에서 제외                   |

