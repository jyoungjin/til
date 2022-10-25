## [ Swagger ] Spring Doc

----

##### Swagger SpringDoc 세팅

```java
implementation 'org.springdoc:springdoc-openapi-ui:1.6.12'
```



##### Swagger SpringDoc 적용

- 프로그램을 실행한 뒤, uri 뒤에 **"/swagger-ui.html"**를 붙여 실행하면 해당 문서를 확인할 수 있다.

- SwaggerConfig

  - 메인 페이지의 이름과 설명을 추가

  - ````java
    
    import io.swagger.v3.oas.annotations.OpenAPIDefinition;
    import io.swagger.v3.oas.annotations.info.Info;
    import lombok.RequiredArgsConstructor;
    import org.springdoc.core.GroupedOpenApi;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    @OpenAPIDefinition(
            info = @Info(title = "API 명세서",
                    description = "API 명세서",
                    version = "v1"))
    @RequiredArgsConstructor
    @Configuration
    public class SwaggerConfig {
    
        @Bean
        public GroupedOpenApi adminApi() {
            String[] paths = {"/admin/**"};
    
            return GroupedOpenApi.builder()
                    .group("ADMIN USER (BEFORE LOGIN) API v1")
                    .pathsToMatch(paths)
                    .build();
        }
    
        @Bean
        public GroupedOpenApi corporationApi() {
            String[] paths = {"/api/**"};
    
            return GroupedOpenApi.builder()
                    .group("CORPORATION USER API v1")
                    .pathsToMatch(paths)
                    .build();
        }
    }
    ````



##### Swagger SpringDoc 주요 어노테이션 목록

| Annotation                                                   | 설명                                                     |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| **@Tag**                                                     | 클래스를 Swagger 리소스로 표시                           |
| **@Parameter(hidden = true) or @Operation(hidden = true) or @Hidden** | API 작업에서 단일 매개 변수를 나타냄                     |
| **@Parameter**                                               | API 작업에서 단일 매개 변수를 나타냄                     |
| **@Parameters**                                              | API 작업에서 복수 매개 변수를 나타냄                     |
| **@Schema**                                                  | Swagger 모델에 대한 추가 정보를 제공                     |
| **@Schema(accessMode = READ_ONLY)**                          | 모델 속성의 데이터를 추가하고 조작                       |
| **@Schema**                                                  | Swagger 모델에 대한 추가 정보를 제공                     |
| **@Operation(summary = "foo", description = "bar")**         | 특정 경로에 대한 작업 또는 일반적으로 HTTP 메서드를 설명 |
| **@Parameter**                                               | 작업 매개 변수에 대한 추가 메타 데이터를 추가            |



##### Swagger 설정

```yaml
springdoc:
  swagger-ui:
    # 메서드 순 정렬
    operations-sorter: method
    # Operationa 목록 펼치기 (default = true)
    doc-expansion: false
```

