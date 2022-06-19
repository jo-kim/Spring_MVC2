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

> 요청 매핑
+ @RestController
  + @Controller 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 된다.
  + @RestController 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다.
+ @PathVariable
+ @RequestMapping 은 URL 경로를 템플릿화 할 수 있는데, @PathVariable 을 사용하면 매칭 되는 부분을 편리하게 조회할 수 있다.
+ consume = 요청헤더의 content Type 
+ produce = 요청헤더의 accept 
