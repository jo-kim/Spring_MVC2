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
