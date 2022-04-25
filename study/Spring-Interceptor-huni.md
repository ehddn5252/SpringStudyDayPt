# Spring Interceptor

### [Interceptor 참고 블로그](https://popo015.tistory.com/115)

### [Filter Interceptor AOP 참고 블로그1](https://dejavuhyo.github.io/posts/spring-filter-interceptor-aop-differences/)

### [Filter Interceptor AOP 참고 블로그2](https://goddaehee.tistory.com/154)

# Spring Interceptor

Controllor의 Handler를 호출하기 전과 후에 request와 response를 참고하거나 가공할 수 있는 일종의 필터

사용자의 request에 의해 서버에 들어온 request 객체를 Controllor의 Handler로 도달하기전에 낚아채서 개발자가 원하는 추가적인 작업을 한 후 Handler로 보낼 수 있또록 해주는것

# Spring Interceptor 사용하는 이유

개발자는 특정 Controller의 핸들러가 실행되기 전이나 후에 추가적인 작업을 원할때 사용

- 추가적인 작업 : 로그인 체크, 권한 체크 등

Handler가 많아지면 메모리 낭비, 서버의 부하가 늘어남

# Spring Method

1. preHandle
    - 컨트롤러가 호출되기 전에 실행됨
    - 컨트롤러가 실행 이전에 처리해야 할 작업이 있는 경우
    혹은 요청 정보를 가공하거나 추가하는 경우 가용
    - 실행되어야 할 Handler에 대한 정보를 인자값으로 받기 때문에
    ‘Servlet Filter’에 비해 세밀하게 로직 구성 가능
    - retrun type이 boolean
        - true : preHandle 실행 후 핸들러에 접근
        - false : 작업을 중단하기 때문에 컨트롤러와 남은 인터셉터가 실행되지 않음
2. postHandle
    - 핸들러가 실행은 완료되었지만 View가 생성되기 이전에 호출
    - ModelAndView 타입의 정보가 인자값으로 받음
    - Controller에서 View정보를 전달하기 위해 작업한 Model 객체의 정보를 참조하거나 조작 가능
    - 적용중인 인터셉터가 여러개인 경우, preHandler는 역순으로 후출
    - 비동기적 요청처리 시에는 처리되지 않음
3. afterCompletion
    - 모든 View에서 최종 겨로가를 생성하는 일을 포함한 모든 작업이 완료된 후에 실행
    - 요청 처리중에 사용한 리소스를 반환해주기 적당한 메스드
    - 적용중인 인터셉터가 여러개인 경우, preHandler는 역순으로 후출
    - 비동기적 요청처리 시에는 처리되지 않음

# Interceptor 동작 위치 및 순서

1. 사용자는 서버에 자신이 원하는 작업을 요청하기 위해 url을 통해 Request 객체를 보냄
2. DispatcherServlet은 해당 Request 객체를 받아서 분석한뒤 ‘HandlerMapping’에게 사용자의 요청을 처리할 핸들러를 찾도록 요청
3. 그 결과로 HandlerExectuonChanin이 동작하게 되는데 이 핸들러 실행체인은 하나 이상의 핸들러 인터셉터를 거쳐서 컨트롤러가 실행될 수 있도록 구성

![image](https://user-images.githubusercontent.com/58337647/165127392-c1ac2523-eb2f-4604-a460-405b0227eac5.png)