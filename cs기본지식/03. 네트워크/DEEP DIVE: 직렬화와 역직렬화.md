## DEEP DIVE: 직렬화와 역직렬화

---

##### 직렬화 & 역직렬화

- 직렬화
  - 직렬화란 외부의 시스템에서도 사용할 수 있도록 바이트(byte) 형태로 데이터를 변환하는 기술을 말합니다. 
- 역직렬화
  - 역직렬화는 내부 시스템에서 사용할 수 있게 객체 등의 형태로 변환하는 것을 말합니다.
- 직렬화란 JSON Object를 JSON 형태를 가진 문자열로 변환하는 것(JSON.stringify)이며 
  문자열을 JSON Object로 바꾸는 것(JSON.parse)을 역직렬화라고 합니다.
- ex) MongoDB에서의 aggregate와 mapReduce의 성능차이
  - MongoDB에서 집적된 계산물을 만들어내는 함수는 aggregate와 mapReduce가 있습니다.	
  - 2개의 차이를 비교할 때 역직렬화 직렬화 개념이 들어갑니다.
    aggregate가 mapReduce 보다 빠른데 이 이유는 다음과 같습니다.
    mapReduce는 BSON이 지원하는 32, 64비트 정수에 대해 자바스크립트 정수타입인 64bit IEEE754 방식으로 변환해서 JSON 직렬화 과정을 거쳐 로직을 수행하고 그다음 다시 BSON으로 변환하는 직렬화과정, 역직렬화과정이 들어가기 때문에 느립니다. 하지만 aggregate는 BSON에 이러한 변환작업을 가하지 않고 쿼리를 수행합니다. 그래서 3 ~ 5배 정도 더 빠릅니다.



