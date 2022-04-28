작성일자: 2022.04.28

# 스프링 설정 파일 (Configuration과 Properties)
- 내부 설정 (configuration.java 파일)
- 외부 설정 (properties 파일)


### 관심사 분리
- 애플리케이션을 하나의 공연이라 생각해보자. 각각의 인터페이스를 배역(배우 역할)이라 생각하자. 그런데! 실제 배역 맞는 배우를 선택하는 것은 누가 하는가?
- 로미오와 줄리엣 공연을 하면 로미오 역할을 누가 할지 줄리엣 역할을 누가 할지는 배우들이 정하는 게 아니다. 

- 배우는 본인의 역할인 배역을 수행하는 것에만 집중해야 한다.
- 디카프리오는 어떤 여자 주인공이 선택되더라도 똑같이 공연을 할 수 있어야 한다.
- 공연을 구성하고, 담당 배우를 섭외하고, 역할에 맞는 배우를 지정하는 책임을 담당하는 별도의 공연 기획자가 필요하다.



### ⚒ 내부 설정 
- 스프링에서 변할수 있는 값 설정, 내부 di 나 기획 설정은 configuration 파일을 통해서 합니다.

```java
package com.ssafy.guestbook.config;


import java.util.Arrays;
import java.util.List;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.ssafy.guestbook.interceptor.ConfirmInterceptor;

@Configuration
@EnableAspectJAutoProxy
@MapperScan(basePackages = {"com.ssafy.**.mapper"})
public class WebMvcConfiguration implements WebMvcConfigurer {
	
	@Autowired
	private ConfirmInterceptor confirm;
	
	private final List<String> patterns = Arrays.asList("/guestbook/*", "/admin/*","/user/list");
	
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(confirm).addPathPatterns(patterns);
	}
}
```



### ⚽외부 설정 (properties)
- 코드 내부에 쓰고 싶지 않은 정보 저장
- 민감한 값의 설정은 application.properties 을 통해서 합니다.

```properties
#server setting
server.port=80

#JSP Setting
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

#DataBase Setting
# driver url id password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/ssafy_user?serverTimezone=UTC&amp;useUniCode=yes&amp;characterEncoding=UTF-8
spring.datasource.username=ssafy
spring.datasource.password=ssafy1234


#MyBatis Setting
mybatis.type-aliases-package=com.ssafy.guestbook.model
# mapper
mybatis.mapper-locations=mapper/**/*.xml

#File Upload size Setting
spring.servlet.multipart.max-file-size = 5MB
spring.servlet.multipart.max-request-size = 5MB


#log level Setting
logging.level.root=info
logging.level.com.ssafy.guestbook.controller=debug


#Failed to start bean 'documentationPluginsBootstrapper'; error
spring.mvc.pathmatch.matching-strategy = ANT_PATH_MATCHER

 ```



properties 값 가져오는 방법
 ```java

@Controller
public class HomeController {
	
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Model model) throws Exception {
	
		// 컨테이너 생성
		GenericXmlApplicationContext ctx 
				= new GenericXmlApplicationContext();
		
		// 환경변수 관리 객체 생성
		ConfigurableEnvironment env = ctx.getEnvironment();
		
		// 프로퍼티 관리 객체 생성
		MutablePropertySources prop = env.getPropertySources();
		
		// 프로퍼티 관리 객체에 프로퍼티 파일 추가
		prop.addLast
			(new ResourcePropertySource("classpath:test.properties"));
		
		// 프로퍼티 정보 얻기
		String ip = env.getProperty("ip");
		String hostName = env.getProperty("hostName");
		
		System.out.println(ip + " : " + hostName);
		
		return "home";
	}
}
```

## .properties

위키백과 정의:

.properties는 응용 프로그램의 구성 가능한 파라미터들을 저장하기 위해 자바 관련 기술을 주로 사용하는 파일들을 위한 파일 확장자이다. 국제화와 지역화를 위한 문자열들을 저장하는데 사용할 수도 있는데 이것을 Property Resource Bundles로 부른다.

### 포맷

- .properties의 각 줄은 일반적으로 하나의 프로퍼티를 저장한다. 키=값, 키 = 값, 키:값, 키 값과 같이 여러 형태가 올 수 있다.

- .properties 파일에서 주석은 줄 맨 앞에 #이나 !로 시작하며, 이 때 해당 줄의 나머지 문자열들은 모두 무시된다. properties 파일의 예는 아래와 같다.

### 참고 자료
- 위키피디아 properties 뜻 (https://ko.wikipedia.org/wiki/.properties)
- properties 파일에서 값 가져오는 방법 (https://codevang.tistory.com/243)