### 오류 메시지
```
BeanCreationException: 
Error creating bean with name 'userRepository' defined in com.sgw.common.repository.UserRepository defined 
in @EnableJpaRepositories declared on DataSourceConfiguration:
Caused by: java.lang.IllegalArgumentException: Not a managed type: class com.sgw.common.domain.User
```

+ JPA에서 repository를 생성했을 때 만난 오류 메시지였다.
+ 해당 repository를 생성해놓고 구현하지 않아 생긴 것 같다.
+ `Repository를 생성 할 때 명시한 entity 객체가 내부에서 entity 객체로 인식되지 않은 문제다`
+ (일단 지우고 실행했더니 정상동작되버렸다.)

