### 오류
```
parameter 2 of constructor in exam.demodto.service.boardservice required a bean of type 'exam.file.filestorehandler' that could not be found
```

+ 컴포넌트 스캔 대상으로 찾지 못해 발생했다.
+ 해당 서비스가 컴포넌트 대상으로 포함되지 않아 의존관계를 주입할 때 에러가 발생한 것이다.
+ `패키지 위치를 확인해보거나, 해당 클래스에 컴포넌트 대상 에노테이션이 있는지 확인해보자`
