##### StopWatch ( 작업 시간 확인 )

```java
// location: org.springframework.util.StopWatch

StopWatch stopWatch = new StopWatch();
stopWatch.start();

// 수행 코드

StopWatch.stop();
StopWatch.prettyPrint(); // 출력	
```

> https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/StopWatch.html

----

**StringUtils**

```java
// location: org.springframework.util.StringUtils

// hasLength() - null + 길이가 0이 아닌지
public static boolean hasLength(@Nullable String str) {
		return (str != null && !str.isEmpty());
}

// hasText() - null + 길이가 0이 아닌지 + 공백이 아닌 문자열 검증
public static boolean hasText(@Nullable String str) {
		return (str != null && !str.isEmpty() && containsText(str));
}
```

>https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/StringUtils.html

