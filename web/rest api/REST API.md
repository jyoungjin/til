## REST API

----

##### REST API 구성

1. 자원 ( resources ) - URI
2. 행위 ( verb ) - HTTP METHOD
3. 표현 ( representations )



##### REST의 특징

1. 유니폼 인터페이스 ( Uniform )
   - URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수정하는 아키텍처 스타일
2. 무상태성 ( Stateless )
   - 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다.
     세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다.
     때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.
3. 캐시 가능 ( Cacheable )
   - HTTP라는 기존 웹 표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능하다.
     따라서 HTTP가 가진 캐싱 기능이 적용 가능하다.
     HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
4. 자체 표현 구조 ( Self-descriptiveness )
   - REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어 있다.
5. Client - Server 구조
   - API 제공, 클라이언트는 사용자 인증이나 컨텍스트 ( 세션, 로그인 정보 ) 등을 직접 관리하는 구조로 
     각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버 간 의존성이 줄어든다.
6. 계층형 구조
   - REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 
     Proxy, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.

----

##### REST API 디자인 가이드

1. **URI는 정보의 자원을 표현해야 한다.**

   1. 리소스명은 동사보다는 명사를 사용

   2. ````http
      GET /members/delete/1   // REST를 제대로 적용하지 않은 URI
      DELETE /members/1       // REST를 적용한 URI
      ````

2. **자원의 대한 행위는 HTTP Method ( GET, POST, PUT, DELETE )로 표현한다.**

   | Method | 역할                                          |
   | ------ | --------------------------------------------- |
   | POST   | POST를 통해 해당 URI를 요청하면 리소스를 생성 |
   | GET    | GET을 통해 해당 리소스를 조회                 |
   | PUT    | PUT을 통해 해당 리소스를 수정                 |
   | DELETE | DELETE를 통해 리소스를 삭제                   |

   

##### URI 설계 시 주의할 점

1. 슬래시 구분자 ( / )는 계층 관계를 나타내는 데 사용

2. URI 마지막 문자로 슬래시 ( / )를 포함하지 않는다.

3. 하이픈 ( - )은 URI 가독성을 높이는데 사용

4. 밑줄 ( _ )은 URI에 사용하지 않는다.

5. URI 경로에는 소문자가 적합하다.

6. 파일 확장자는 URI에 포함시키지 않는다.

   - REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
     **Accept header를 사용하여 해결**

   - ```http
     GET /members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg
     ```



##### 리소스 간의 관계를 표현하는 방법

```http
GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
GET : /users/{userid}/likes/devices (관계명이 애매하거나 구체적 표현이 필요할 때)
```



##### 자원을 표현하는 Collection과 Element

```http
http://restapi.example.com/sports/soccer/players/13 (Collection은 복수로 Element는 단수로 사용)
```