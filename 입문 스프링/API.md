정적 컨텐츠를 제외하면 '2가지'를 기억하면된다.
+ `MVC 방식` : view를 찾아서 템플릿 엔진을 통해 화면을 rendering해서 웹브라우저에 html을 넘겨준다.
+ `API 방식` : `1` 그대로 데이터 넘겨준다. / `2` 객체로 넘기기


## ResponseBody로 문자반환
```java
@Controller
public class HelloController {
 @GetMapping("hello-string")
 @ResponseBody
 public String helloString(@RequestParam("name") String name) {
 return "hello " + name;
 }
}
```

+ localhost:8080/hello-spring?name=spring!을 요청하면, 'hello spring!'이 출력된다.
+ `요청 파라미터 그대로 데이터를 넣는다.`
+ @ResponseBody 를 사용하면 뷰 리졸버( viewResolver )를 사용하지 않음
+ `@ResponseBody :대신에 HTTP의 BODY에 문자 내용을 직접 반환(HTML BODY TAG를 말하는 것이 아님)`


---
## @ResponseBody 객체반환
```java
 @GetMapping("hello-api")
 @ResponseBody
 public Hello helloApi(@RequestParam("name") String name) {
 Hello hello = new Hello();
 hello.setName(name);
 return hello;
 }
 
 static class Hello {
 private String name;
 
 public String getName() {
 return name;
 }
 public void setName(String name) {
 this.name = name;
 }
 }
 ```
 
+ hello객체를 만들어 넘긴다.
+ `@ResponseBody 를 사용하고, 객체를 반환하면 객체가 JSON으로 변환됨`
+ localhost:8080/hello-api?name=spring!을 요청하면, {"name" : "spring!"}이 출력된다.

> 번외, <br> 옛날에는 xml방식(HTML방식)으로 썼다. HTML방식은 '<></>' 열고 닫는 방식으로 무거웠는데 최근에는 JSON 방식으로 통일되어서 가볍다. <br> 그래서 JSON으로 반환되었음



---
## @ResponseBody 사용원리
![입문2](https://user-images.githubusercontent.com/57389368/171414235-d969f86b-3843-419a-bf6a-50632022c96f.JPG)
+ `1` 웹브라우저에서 'localhost:8080/hello-api?name=spring'을 친다.
+ `2` 톰켓서버가 그걸 확인해서, 스프링에게 보낸다.
+ `3` 스프링은 helloController(컨트롤러)가 @ResponseBody를 확인한다.

> 이전에 'MVC-템플릿엔진'에서는 viewResolver에게 넘겼었다. 

<br> 

> @ResponseBody을 확인해서 문자를 넘기려고 했지만, <br> 객체를 확인했다. Hello hello = new Hello(); <br> 그래서 JSON방식으로 데이터를 만들어 HTTP응답에 반환해야 했다.

+ `4` 객체반환이라, viewResolver 대신에 HttpMessageConvert가 동작한다.

> 기본 문자이면, 'StringHttpMessageConverter'가 동작한다. <br> 기본 객체이면, 'MessageJackson2HttpMessageConverter'가 동작한다.

+ `5` JSON타입으로 응답을 반환한다.
