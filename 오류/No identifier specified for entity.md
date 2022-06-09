### 오류내용
```
org.hibernate.AnnotationException: No identifier specified for entity 
```

+ 엔티티 선언시 식별자를 선언해주지 않아서 발생하는 에러이다.
+ `엔티티 내 특정 요소 중 하나에 @Id 어노테이션 추가해준다.` (@Id @GeneratedValue private Long id;)
