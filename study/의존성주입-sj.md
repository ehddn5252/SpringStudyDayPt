## 의존성 주입 (Dependecy Injection, DI)

#### 의존성 주입 Intro

- `DI`란 외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴

- 인터페이스를 둠으로써 클래스 레벨에서는 의존관계가 고정되지 않도록 하고 런타임 시에 관계를 다이나믹하게 주입

- 유연성 확보, 결합도 낮춤.

  

- 아래의 코드에서 Store 객체가 Pencil  객체에 의존성이 있다고 표현

  ```java
  public class Store {
      private Pencil pencil;
  }
  ```

  

- ##### DI 컨테이너

  - 애플리케이션 실행 시점에 필요한 객체(빈)을 생성
  - 의존성이 있는 두 객체를 연결하기 위해 한 객체(Pencil)를 다른 객체(Store)로 주입
  - 제어의 역전(IoC): 어떠한 객체를 사용할지에 대한 책임이 BeanFactory와 같은 클래스에게 넘어갔고, 자신은 수동적으로 주입받는 객체를 사용 (실제 Spring에서는 BeanFactory => Application Context) 

  ```java
  public class BeanFactory {
      
      public void store() {
          // Bean의 생성
          Product pencil = new Pencil();
          
          // 의존성 주입
          Store store = new Store(pencil);
      }
      
  }
  ```



#### 의존성 주입 장점 정리

- 두 객체 간의 관계라는 관심사의 분리
- 두 객체 간의 결합도를 낮춤
- 객체의 유연성을 높임
- 테스트 작성 용이



---

#### 의존성 주입 방법

1. ##### 생성자 주입 (Constructor Injection)

   - 생성자의 호출 시점에 1회 호출되는 것이 보장 -> **불변, 필수 의존관계에 사용**
   - 생성자가 1개만 있을 경우 `@Autowired` 생략 가능

   ```java
   @Service 
   public class UserServiceImpl implements UserService {
       private UserRepository userRepository; 
       private MemberService memberService; 
       
       @Autowired // 생략 가능
       public UserServiceImpl(UserRepository userRepository, MemberService memberService) { 
           this.userRepository = userRepository; 
           this.memberService = memberService; 
       } 
   }
   ```

   

2. ##### 수정자 주입 (Setter Injection)

   - 생성자 주입과 다르게 **주입받는 객체가 변경될 가능성이 있는 경우**에 사용 (극히 드뭄)

   ```java
   @Service 
   public class UserServiceImpl implements UserService { 
       private UserRepository userRepository; 
       private MemberService memberService; 
       
       @Autowired 
       public void setUserRepository(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
       
       @Autowired 
       public void setMemberService(MemberService memberService) { 
           this.memberService = memberService; 
       } 
   }
   ```

   

3. ##### 필드 주입

   - 필드에 바로 의존 관계 주입
   - 인텔리제이에서는 경고 문구 발생
   - 간결해서 과거에 많이 이용됨.
   - 외부에서 접근이 불가능하다는 단점 존재
   - 필드 주입은 반드시 DI 프레임워크가 존재해야 함.

   ```java
   @Service 
   public class UserServiceImpl implements UserService { 
       @Autowired 
       private UserRepository userRepository; 
       @Autowired private MemberService memberService; 
   }
   ```

   

### 생성자 주입을 사용해야 하는 이유

#### 장점

1. ##### 객체의 불변성 확보

   - 실무에서 의존 관계의 변경이 필요한 상황은 거의 없음.
   - 수정의 가능성을 배제해서 불변성 보장 굿 

2. ##### 테스트 코드의 작성

   - 다른 주입 방법으로 작성된 코드는 순수한 자바 코드로 단위 테스트 작성하는 것이 어려움
   - 컴파일 시점에 객체를 주입받아 테스트 코드 작성 가능/주입하는 객체가 누락된 경우 컴파일 시점에 오류 발견

3. ##### final 키워드 작성 및 Lombok 과의 결합

   - final 키워드를 사용하여, 컴파일 시점에 누락된 의존성 확인 가능 (다른 주입 방법들은 생성 이후에 호출되므로 final 키워드 사용 불가능)
   - final 키워드를 붙임으로써 Lombok과 결합되어 코드를 간결하게 작성 가능

4. ##### 순환 참조 에러 방지

   - 애플리케이션 구동 시점 (객체 생성 시점)에 순환 참조 에러 예방

   - ##### 필드 주입의 순환 참조 예시

     - StackOverflow 발생 -> 서버 죽음

![image-20220426224604905](0426_의존성주입.assets/image-20220426224604905.png)



#### 생성자 주입 장점 정리

- 객체의 불변성 확보
- 테스트 코드의 작성 용이
- final 키워드 사용가능/ Lombok과의 결합을 통해 코드를 간격하게 작성 가능
- 순환 참조 에러를 애플리케이션 구동 시점에 파악하여 방지 가능

---

#### 롬복과 스프링의 결합

1. Spring 프레임워크를 통한 생성자 주입/ fianl 키워드와의 결합(싱글톤이므로 불변성을 보장하기 위해 사용)

   ```java
   @Service 
   public class UserService { 
       private final UserRepository userRepository; 
       private final PasswordEncoder passwordEncoder; 
       
       @Autowired 
       public UserService userService(UserRepository userRepository, PasswordEncoder passwordEncoder) {
           this.userRepository = userRepository; 
           this.passwordEncoder = passwordEncoder;
       }
   }
   ```

2.  Lombok(롬복)과의 결합

   - 생성자가 1개라면 `@Autowired`생략 가능하여 Lombok  라이브러리로 생성자 주입을 하면 코드가 매우 간결해짐

   ```java
   @Service 
   @RequiredArgsConstructor 
   public class UserService { 
       private final UserRepository userRepository; 
       private final PasswordEncoder passwordEncoder; 
   }
   
   ```

   

