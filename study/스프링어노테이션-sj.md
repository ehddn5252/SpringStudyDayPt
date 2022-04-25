# 어노테이션 종류

#### @ComponentScan

- `@ComponentScan`는 @Component와 @Service, @Repository, @Controller, @Configuration이 붙은 클래스 Bean들을 찾아서 Context에 bean 등록을 해줌.

- xml 버전

  ```xml
  <context:component-scan base-package="com.ssafy.emp"/>
  ```

- 어노테이션 버젼

  ```java
  @Configuration // Bean 설정 파일 (XML 파일 대체)
  @ComponentScan (basePackage = "com.ssafy.emp") // 어노테이션이 부여된 클래스들을 자동으로 IoC 컨테이너에 등록 (경로)
  // ComponentScan (basePackageClasses = "Application.class") (클래스 기존)
  public class ApplicationConfig {}
  ```

  

---

##### 	+ 왜 다 @Component로 하지 않고 다르게 빈 설정을 할까?

	1. @Repository는 DAO의 메소드에서 발생할 수 있는 unchecked exception들을 스프링의 DataAccessException으로 처리 가능
	1. 가독성이 좋음.

---

#### @Configuration

- 스프링의 환경 설정과 관련된 Java 클래스임을 알림.

- `@Configuration`을 클래스에 적용하고 @Bean을 해당 클래스의 메서드에 적용하면 @Autowired로 Bean을 부를 수 있음.

  ```java
  @Configuration
  public class ResourceConfig {
      
      @Bean // 이 클래스에서의 Bean 함수가 리턴하는 객체는 자체적으로 싱글톤으로 관리
      public Resource resource() {
          return new Resource();
      }
      
  }
  ```

  



#### @Component

- `@Component`는 개발자가 직접 작성한 class를 bean으로 등록해줌.

- Component에 대한 추가 정보가 없으면, 클래스명을 camelCase로 변경한 것이 Bean id로 사용됨.

- `value`로 이름 설정

  ```java
  @Component(value="mystudent")
  public class Student {
      public Student() {
          System.out.println("hi");
      }
  }
  ```

  

#### @Bean

- `@Bean`은 개발자가 직접 제어가 불가능한 외부 라이브러리 등을 Bean으로 만들려할 때 사용.

- `value`로 이름 설정 **(@Component와 다름)**

  ```java
  @Configuration
  public class ApplicationConfig {    
      @Bean
      public ArrayList<String> array(){
          return new ArrayList<String>();
      }   
  }
  // 라이브러리 객체를 반환하는 Method를 만들고 @Bean 사용
  ```

  

#### @Autowired

- `@Autowired`는 Type에 따라 자동으로 Bean을 주입해주며, 속성/setter/생성자에서 사용.

- 주로 Controller 클래스에서 DAO나 Service에 관한 객체들을 주입시킬 때 많이 사용.

- `@Qualifier`을 사용하면 같은 타입이 존재해도 name으로 지정 가능.

  ```java
  public class Profile {
     @Autowired
     @Qualifier(value="student1")
     private Student student;
  ```

---

##### (+Bean을 주입받는 방식)

 	1. `@Autowired`
 	2. setter
 	3. 생성자 (`@AllArgsConstructor` 사용) -> 권장방식

---

#### @Inject

- `@Autowired`와 비슷한 역할



#### @Controller

- spring MVC에서 Controller 클래스에 쓰임

#### 	@RestController 

- View로 응답하지 않는 Controller **(@Controller + @ResponseBody)**
- method의 반환 결과를 JSON 형태로 반환 (원래는 메소드에 `@ResponseBody` 붙여야 함.)
- 이 Controller의 메서드는 HttpResponse로 바로 응답 가능



#### @Service

- 비즈니스 로직을 수행하는 Service Class



#### @Repository

- DataBase에 접근하는 메서드를 가지고 있는 DAO Class



#### @EnableAutoConfiguration

- Spring Application Context를 만들 때 자동으로 설정하는 기능을 켬
- classpath의 내용에 기반해서 자동으로 생성



#### @Required

- setter method에 적용하면 Bean 생성 시 필수 프로퍼티 임을 알림.

- 영향을 받는 bean property 구성할 시에는 XML 설정 파일에 반드시 property를 채워야 함.

  ##### 사용 예시

  ```java
  public class RequiredClass {
  	private int number; 
  
  	@Required // 반드시 프로퍼티를 이용하여 값을 주입받도록 정의. 값이 안들어오면 에러나게 함.
  	public void setNumber(int number){
  		this.number = number;
  	}
  	public void display(){
  		System.out.println("RequiredClass.number = "+number);
  	}
  }
  ```

  ##### xml 설정

  ```xml
  <bean id="required" class="spring.anno.ch01.RequiredClass">
  	<property name="number" value="10"/><!--SetNumber를 호출하며 10을 넣어줌 -->
  </bean>
  ```

  

#### @RequestMapping

- `@RequestMapping`은 요청 URL을 어떤 method가 처리할지 mapping.

- Controller나 Controller의 메서드에 적용

- 요청 형식: GET,POST,PATCH,PUT,DELETE

- 디폴트는 GET

  ```java
  @Controller
  // 1) Class Level
  //모든 메서드에 적용되는 경우 “/home”로 들어오는 모든 요청에 대한 처리를 해당 클래스에서 한다는 것을 의미
  @RequestMapping("/home") 
  public class HomeController {
      /* an HTTP GET for /home */ 
      @RequestMapping(method = RequestMethod.GET)
      public String getAllEmployees(Model model) {
          ...
      }
      /*
      2) Handler Level
      요청 url에 대해 해당 메서드에서 처리해야 되는 경우
      “/home/employees” POST 요청에 대한 처리를 addEmployee()에서 한다는 것을 의미한다.
      value: 해당 url로 요청이 들어오면 이 메서드가 수행된다.
      method: 요청 method를 명시한다. 없으면 모든 http method 형식에 대해 수행된다.
      */
      /* an HTTP POST for /home/employees */ 
      @RequestMapping(value = "/employees", method = RequestMethod.POST) 
      public String addEmployee(Employee employee) {
          ...
      }
  }
  ```

- @RequestMapping에 대한 모든 매핑 정보는 스프링에서 제공하는 HandlerMapping 클래스가 가지고 있음.

<img src="0425_스프링어노테이션.assets/image-20220425222951470.png" alt="image-20220425222951470" style="zoom:50%;" />





#### @CookieValue

- 쿠키 값을 parameter로 전달 받을 수 있음.

- `required` 속성이 true이면(default), 해당 쿠키가 존재하지 않을 때 500 에러 발생

- `required` 속성이 false이면, null 반환하고 에러 발생 X

  ```java
  // 쿠키의 key가 auth에 해당하는 값을 가져옴
  public String view(@CookieValue(value="auth")String auth){...}; 
  ```



#### @RequestBody (`@ResponseBody`와 반대)

- 요청이 온 데이터(JSON이나 XML형식)를 바로 Class나 model로 매핑

- POST나 PUT, PATCH로 요청을 받을때에, **요청에서 넘어온 body 값들을 자바 타입으로 파싱**

- HTTP POST 요청에 대해 request body에 있는 request message에서 값을 얻어와 매핑한다.
  RequestData를 바로 Model이나 클래스로 매핑한다.

- 이를테면 **JSON 이나 XML같은 데이터를 적절한 messageConverter로 읽을 때 사용하거나 POJO 형태의 데이터 전체로 받는 경우에 사용**한다.

```java
@RequestMapping(value = "/book", method = RequestMethod.POST)
public ResponseEntity<?> someMethod(@RequestBody Book book) {
// we can use the variable named book which has Book model type.
try {
   service.insertBook(book);
} catch(Exception e) {
    e.printStackTrace();
}

// return some response here
}
```



#### @CrossOrigin

- SOP/CORS 라는 보안 정책 때문에, 동일한 출처의 Origin만 리소스를 공유할 수 있음.

- 이를 해결하기 위한 방법 중 하나가 `@CrossOrigin`을 메서드나 컨트롤러 상단부에 추가하는 것

- `@CrossOrigin`를 붙이면 **모든 도메인, 모든 요청방식**에 대해 허용

- 다음과 같이 도메인을 지정하면, **특정 도메인**만 허용

  ```java
  @CrossOrigin(origins = "http://domain1.com, http://domain2.com")
  @RequestController
  @RequestMapping("/account")
  public class AccountController{
  	
      @RequestMapping("/{id}")
      public Account retrieve(@PathVariable Long id){
      	// ... 
      }
      
      @RequestMapping(method = RequestMethod.DELETE, value = "/{id}")
      public void remove(@PathVariable Long id){
      	// ...
      }
  }
  ```

  

#### @ModelAttribute



#### @Transactional

- 메서드에 `@Transactional`을 적용하면 해당 메서드에 대한 로직 전체가 트랜잭션으로 설정됨.

- method 내부에서 일어나는 데이터 로직이 전부 성공되거나 하나라도 실패하면 다시 롤백 (트랜잭션의 원자성)

- 일반적으로 등록/수정/삭제 하는 메서드는 `@Transactional`이 필수

- 비지니스 로직과 트랜잭션 관리는 대부분 Service 클래스에서 관리

  ```java
  @Service
  public class GuestBookServiceImpl implements GuestBookService {
  	
  	@Autowired
  	private GuestBookMapper guestBookMapper;
      
  	@Override
  	@Transactional
  	public void registerArticle(GuestBookDto guestBookDto) throws Exception {
  		guestBookMapper.registerArticle(guestBookDto);
  		List<FileInfoDto> fileInfos = guestBookDto.getFileInfos();
  		if (fileInfos != null && !fileInfos.isEmpty()) {
  			guestBookMapper.registerFile(guestBookDto);
  		}
  	}
  }
  ```

  

---

### Lombok Annotation

#### @NoArgsConstructor

- 기본 생성자를 자동 추가

  ```java
  @NoArgsConstructor(access = AccessLevel.PROTECTED)
  
  // protected Posts() {} 와 유사
  ```

  

#### @AllArgsConstructor

- 모든 필드 값을 파라미터로 받는 생성자 추가



#### @RequiredArgsConstructor

- final이나 @NonNull인 필드 값만 파라미터로 받는 생성자를 추가
  *final: 값이 할당되면 더 이상 변경할 수 없다.*

  

#### @Getter

- Class 내 모든 필드의 Getter method를 자동 생성



#### @Setter

- Class 내 모든 필드의 Setter method를 자동 생성

- Controller에서 @RequestBody로 외부에서 데이터를 받는 경우엔 기본생성자 + set method를 통해서만 값이 할당된다.
  그래서 이때만 setter를 허용한다.
  *Entity Class에는 Setter를 설정하면 안된다.
  차라리 DTO 클래스를 생성해서 DTO 타입으로 받도록 하자*



#### @ToString

- Class 내 모든 필드의 toString method를 자동 생성

`@ToString(exclude = "password")`
특정 필드를 toString() 결과에서 제외한다.
클래스명(필드1이름=필드1값, 필드2이름=필드2값, …) 식으로 출력된다.





#### 참고자료

**[Spring] Annotation 정리** https://velog.io/@gillog/Spring-Annotation-%EC%A0%95%EB%A6%AC#requestbody

