## Spring 트랜잭션

----

- 트랜잭션

  - 데이터베이스의 상태를 변경하는 작업 또는 한번에 수행되어야 하는 연산들을 의미한다.
  - beginl, commit을 자동으로 수행해준다.
  - 예외 발생 시, rollback 처리를 자동으로 수행해준다.
  - 트랜잭션은 4가지의 성질(ACID)을 가지고 있다.
    1. 원자성(Atomicity): 한 트랜잭션 내에서 실행한 작업들은 하나의 단위로 처리한다. 즉, 모두 성공 또는 모두 실패
    2. 일관성(Consistency): 트랜잭션은 일관성 있는 데이터베이스 상태를 유지한다.
    3. 격리성(Isolation): 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야한다.
    4. 영속성(Durability): 트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.

- 처리 방법

  - @Transactional을 메소드, 클래스, 인터페이스 위에 추가하여 사용하는 방식이 일반적이다.
    이 방식을 선언적 트랜잭션이라 부르며, 적용된 범위에서는 트랜잭션 기능이 포함된 프록시 객체가 자동으로 commit 혹은 rollback을 진행해준다.

  - ```java
    @Transactional
    public void addUser(UserDTO dto) throws Exception {
    	// 로직 구현
    }
    ```

- @Transactional 옵션

  1. isolation: 트랜잭션에 일관성없는 데이터 허용 수준을 설정한다.
  2. propagation: 트랜잭션 동작 도중 다른 트랜잭션을 호출할 때, 어떻게 할 것인지 지정하는 옵션이다.
  3. noRollbackFor: 특정 예외 발생 시, rollback하지 않는다.
  4. rollbackFor: 특정 예외 발생 시, rollback한다.
  5. timeout: 지정한 시간 내에 메소드 수행이 완료되지 않으면 rollback한다. (-1일 경우 timeout을 사용하지 않는다.)
  6. readOnly: 트랜잭션을 읽기 전용으로 사용한다.



##### 1. isolation (격리 레벨)

```java
@Transactional(isolation=Isolation.DEFAULT)
public void addUser(UserDTO dto) throws Exception {
	// 로직 구현
}
```

- **DEFAULT**: 기본 격리 수준
  - 기본이며, DB의 isolation level을 따른다.
- **READ_UNCOMMITED** (level 0): 커밋되지 않는 데이터에 대한 읽기를 허용
  - 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 B라는 아직 완료되지 않은(Uncommitted 혹은 Dirty)데이터 B를 읽을 수 있다.
    **Dirty Read 발생**
- **READ_COMMITED** (level 1): 커밋된 데이터에 대해 읽기 허용
  - 어떠한 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 해당 데이터에 접근할 수 없다.
    **Dirty Read 방지**
- 

