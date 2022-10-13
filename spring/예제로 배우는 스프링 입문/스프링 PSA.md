## 스프링 PSA

----

##### Portable Service Abstraction

환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하려는 추상화 구조
PSA가 적용된 코드라면 나의 코드가 바뀌지 않고, 다른 기술로 간편하게 변경 가능하며, 기술에 특화되어 있지 않는 코드를 의미

1. 스프링 웹 MVC
   - @Controller 와 @RequestMapping
   - Servlet | Reactive
   - 톰캣, 제티, 네티, 언더토우
2. 스프링 트랜잭션
   - @Transactional
   - JpaTransacionManager | DatasourceTransactionManager | HibernateTransactionManager
3. 스프링 캐시
   - @Cacheable | @CacheEvict | ...
   - JCacheManager | ConcurrentMapCacheManager | EhCacheCacheManager
