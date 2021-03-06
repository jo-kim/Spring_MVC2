# Spring_MVC2
> Jar -> 내장 서버 사용  ||  War -> 주로 외부서버 배포 목적
> 스프링 부트에 Jar 사용하면 /resources/static/ 위치에 index.html 파일을 두면 Welcome 페이지로 처리해준다

+ Logging
  > 운영 시스템에서는 System.out.println() 같은 시스템 콘솔을 사용해서 필요한 정보를 출력하지 않고, 별도의 로깅 라이브러리를 
  > 사용해서 로그를 출력
+ 로그 선언
  + private Logger log = LoggerFactory.getLogger(getClass());
  + private static final Logger log = LoggerFactory.getLogger(Xxx.class)  
  +  @Slf4j : 롬복 사용 가능
+ 로그 사용시 장점
  + 쓰레드 정보, 클래스 이름 같은 부가 정보를 함께 볼 수 있고, 출력 모양을 조정할 수 있다.
  + 로그 레벨에 따라 개발 서버에서는 모든 로그를 출력하고, 운영서버에서는 출력하지 않는 등 로그를 상황에 맞게 조절할 수 있다.
  + 시스템 아웃 콘솔에만 출력하는 것이 아니라, 파일이나 네트워크 등, 로그를 별도의 위치에 남길 수 있다. 
  + 특히 파일로 남길 때는 일별, 특정 용량에 따라 로그를 분할하는 것도 가능하다.
  + 성능도 일반 System.out보다 좋다. (내부 버퍼링, 멀티 쓰레드 등등) 그래서 실무에서는 꼭 로그를 사용해야 한다.

+ 요청 매핑
>+ @RestController
>+ @Controller 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 된다.
>+ @RestController 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다.
+ @PathVariable
>+ @RequestMapping 은 URL 경로를 템플릿화 할 수 있는데, @PathVariable 을 사용하면 매칭 되는 부분을 편리하게 조회할 수 있다.
>+ consume = 요청헤더의 content Type 
>+ produce = 요청헤더의 accept 
+ MultiValueMap
>+ MAP과 유사한데, 하나의 키에 여러 값을 받을 수 있다.
>+ HTTP header, HTTP 쿼리 파라미터와 같이 하나의 키에 여러 값을 받을 때 사용한다. 
>+ keyA=value1&keyA=value2
+ POST, HTML Form 전송
  >+ 요청파라미터 조회 (request parameter) 
  >+ Get 쿼리 파라미터 전송방식이든, POST HTML Form 전송 방식이든 둘다 형식이 같으므로 구분없이 조회 가능
+ requestParamV1
  >+ HttpServletRequest 가 제공하는 방식으로 요청 파라미터 조회
+ requestParamV2
  >+ @RequestParam
  >+ 파라미터 이름으로 바인딩
  >+ @ResponseBody 추가 ( View조회를 무시하고, HTTP message body에 직접 해당 내용입력)
  >+ @RequestParam("username") String memberName
  >+ request.getParameter("username"
+ requestParamV3
  >+ HTTP 파라미터 이름이 변수 이름과 같으면 @RequestParam(name="xx") 생략 가능
+ requestParamV4
  >+ String , int , Integer 등의 단순 타입이면 @RequestParam 도 생략 가능
+ requestParamRequired
  >+ 파라미터 필수 여부
  >+ 기본값이 @RequestParam(required = true) 
+ requestParamMap
  >+ 파라미터를 Map으로 조회
  >+ @RequestParam Map , Map(key=value)
+ @ModelAttribute V1
  ```
   @ResponseBody
   @RequestMapping("/model-attribute-v1")
   public String modelAttributeV1(@ModelAttribute HelloData helloData) {
   log.info("username={}, age={}", helloData.getUsername(),
   helloData.getAge());
   return "ok";
   }
   ```
 >+ HelloData가 객체를 생성
 >+ 요청 파라미터의 이름으로 HelloData객체의 프로퍼티를 찾는다.
 >+ 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력(바인딩) 한다
    >+ 예) 파라미터 이름이 username 이면 setUsername() 메서드를 찾아서 호출하면서 값을 입력.
    >+ 프로퍼티: 객체에 getUsername(), setUsername() 메서드 있으면 이객체는 username이라는 프로퍼티를 가지고 있음.
+ @ModelAttribute V2
  >+ @ModelAttribute - 생략가능
+ @RequestBody
  >+ HTTP 메시지 바디 정보를 편리하게 조회할 수 있다.
  >+ 헤더 정보가 필요하면 HttpEntity 나 @RequestHeader를 사용
#### 요청 파라미터를 조회하는 기능: @RequestParam , @ModelAttribute
#### HTTP 메시지 바디를 직접 조회하는 기능: @RequestBody
+ @ResponseBody
  >+ 응답결과를 Http에 직접 담아서 전달, view를 사용하지 않는다.
+ RequestBodyJsonV1
  >+ HttpServletRequest를 사용해서 직접 HTTP 메시지 바디에서 데이터를 읽어와서, 문자로 변환.
  >+ 문자로 된 JSON 데이터를 Jackson 라이브러리인 objectMapper 를 사용해서 자바 객체로 변환
+ RequestBodyJsonV2
  >+ @RequestBody 를 사용해서 HTTP 메시지에서 데이터를 꺼내고 messageBody에 저장.
  >+ 문자로 된 JSON 데이터인 messageBody 를 objectMapper 를 통해서 자바 객체로 변환
+ RequestBodyJsonV3
  >+ @RequestBody HelloData data
  >+ @RequestBody 에 직접 만든 객체를 지정할 수 있다
  >+ HttpEntity , @RequestBody 를 사용하면 HTTP 메시지 컨버터가 HTTP 메시지 바디의 내용을 우리가 원하는 문자나 객체 등으로 변환
  >+ @RequestBody는 생략 불가능 (HelloData에 @RequestBody 를 생략하면 @ModelAttribute 가 적용되어버린다.
HelloData data @ModelAttribute HelloData data
따라서 생략하면 HTTP 메시지 바디가 아니라 요청 파라미터를 처리하게 된다)
+ RequestBodyJsonV4
  >+ HttpEntity사용
+ RequestBodyJsonV5
  >+ @RequestBody 요청
    >+ JSON요청 -> HTTP 메시지 컨버터 - > 객체
  >+ @ResponseBody 응답
    >+ 객체 - > HTTP 메시지 컨버터 -> JSON 응답
-----------
+ responseBodyV1
  >+ 서블릿을 직접 다룰 때 처럼 HttpServletResponse 객체를 통해서 HTTP 메시지 바디에 직접 ok 응답 메시지를 전달.
+ responseBodyV2
  >+ ResponseEntity는 HttpEntity 를 상속 받았는데, HttpEntity는 HTTP 메시지의 헤더, 바디 정보를 가지고 있음.
  >+ ResponseEntity는 HttpStatus.CREATED 와 같이 HTTP 응답코드를 설정 할 수 있다.
+ responseBodyV3
  >+ @ResponseBody 를 사용하면 view를 사용하지 않고, HTTP 메시지 컨버터를 통해서 HTTP 메시지를 직접 입력할 수 있다.
+ responseBodyJsonV1
  >+ ResponseEntity 를 반환한다. HTTP 메시지 컨버터를 통해서 JSON 형식으로 변환되어서 반환
+ responseBodyJsonV2
  >+ @ResponseStatus(HttpStatus.OK) 애노테이션을 사용하여 응답코드 설정 가능.
+ @RestController
  >+ @Controller 대신에 @RestController 애노테이션을 사용하면, 해당 컨트롤러에 모두@ResponseBody 가 적용되는 효과가 있다. 
  >+ 따라서 뷰 템플릿을 사용하는 것이 아니라, HTTP 메시지 바디에 직접 데이터를 입력한다. 
  >+ 이름 그대로 Rest API(HTTP API)를 만들 때 사용하는 컨트롤러
#### HTTP 메시지 컨버터
>+ @ResponseBody 를 사용 -> HTTP의 BODY에 문자 내용을 직접 반환 , viewResolver 대신에 HttpMessageConverter 가 동작
+ HTTP 메시지 컨버터 적용 케이스
  + HTTP 요청 : @RequestBody, HttpEntity(RequestEntity)
  + HTTP 응답 : @ResponseBody, HttpEntity(ResponseEntity)
  >+ canRead(), canWrite() 로 해당클래스, 미디어타입을 지원하는지 체크
  >+ read(), write() 로 메시지를 읽고 씀
  >+ Byte~ , String~, MappingJackson~ 컨버터를 제공하는데 대상 클래스타입과 미디어 타입을 체크 후 사용여부 결정
+ HTTP 요청 데이터 읽기
  >+ 요청이오면, 컨트롤러에서 @RequestBody, HttpEntity 파라미터 사용
  >+ 컨버터가 메시지 읽을 수 있는지 여부 확인 위해 canRead() 호출
  >+ 클래스 타입과 미디어타입 확인 후 조건 만족 시 read()를 호출해서 객체를 생성하고 반환한다.
+ HTTP 응답 데이터 생성
  >+ 컨트롤러에서 @ResponseBody, HttpEntity 로 값이 반환
  >+ 컨버터가 메시지를 쓸 수 있는지 확인 위해 canWrite() 호출
  >+ 클래스 타입과 미디어타입 확인 후 조건 만족 시 write() 호출해서 HTTP 응답 메시지 바디에 데이터를 생성.
### RequestMappingHandler
![image](https://user-images.githubusercontent.com/92084680/175439600-c093bdd5-44b3-4c18-aa69-9b68119eeda7.png)
> ArgumentHandler [Request]
  + 애노테이션 기반 컨트롤러를 처리하는 RequestMappingHandlerAdapter 는 
  + ArgumentResolver 를 호출해서 컨트롤러(핸들러)가 필요로 하는 다양한 파라미터의 값(객체)을 생성한다.
  + 파라미터의 값이 모두 준비되면 컨트롤러를 호출하며너 값을 넘겨준다.
> 동작방식
  + ArgumentResolver 의 supportsParameter() 를 호출해서 해당 파라미터를 지원하는지 체크
  + 지원하면 resolveArgument() 를 호출해서 실제 객체를 생성.
  + 이렇게 생성된 객체가 컨트롤러 호출시 넘어간다.
 > ReturnValueHandler [Response]
  + 응답값을 변환하고 처리하는 역할.
    >+ 컨트롤러에서 String으로 뷰 이름을 반환해도, 동작하는 이유가 바로 ReturnValueHandler 덕분
    

 ![image](https://user-images.githubusercontent.com/92084680/175440097-7e727237-1979-467e-a1e7-bb67c0d1b716.png)
   + 요청
      + @RequestBody를 처리하는 ArgumentResolver가 있고, HttpEntity를 처리하는 ArgumentResolver가 있다.
      + 이 ArgumentResolver들이 HTTP메시지 컨버터를 사용해서 필요한 객체를 생성.
   + 응답
      + @ResponseBody와 HttpEntity를 처리하는 ReturnValueHandler가 있다. 
      + 이 ReturnValueHandler가 HTTP 메시지 컨버터를 호출해서 응답 결과를 만든다

