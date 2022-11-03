## content type 'application/octet-stream' not supported

----

##### 오류 발생

>@RequestPart과 MultipartFile을 사용하여 이미지 올리는 것을 구현하는 중 content type 'application/octet-stream' not supported ~. 오류가 발생하였다.

##### 오류 해결

```java
// MultipartJackson2HttpMessageConverter를 추가하면 끝!

import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.http.MediaType;
import org.springframework.http.converter.json.AbstractJackson2HttpMessageConverter;
import org.springframework.stereotype.Component;

import java.lang.reflect.Type;

@Component
public class MultipartJackson2HttpMessageConverter extends AbstractJackson2HttpMessageConverter {

    /**
     * Converter for support http request with header Content-Type: multipart/form-data
     */
    public MultipartJackson2HttpMessageConverter(ObjectMapper objectMapper) {
        super(objectMapper, MediaType.APPLICATION_OCTET_STREAM);
    }

    @Override
    public boolean canWrite(Class<?> clazz, MediaType mediaType) {
        return false;
    }

    @Override
    public boolean canWrite(Type type, Class<?> clazz, MediaType mediaType) {
        return false;
    }

    @Override
    protected boolean canWrite(MediaType mediaType) {
        return false;
    }
}
```

##### 참조

- https://stackoverflow.com/questions/16230291/requestpart-with-mixed-multipart-request-spring-mvc-3-2