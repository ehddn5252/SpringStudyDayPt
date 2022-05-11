작성일자: 2022.04.26



# Gradle vs Maven

### 목차

1. 빌드 관리 도구
2. Gradle
3. Maven



### Build Tool(빌드 관리 도구)

- 빌드 자동화와 외부 라이브러리 관리를 위해 빌드 관리 도구가 사용된다.
- build를 자동으로 해주는 도구이다.
- 빌드하는 과정은 매우 복잡하고 수동으로 하면 시간과 노력이 많이 들기 때문에 이를 관리하는 도구를 만들었다. 
- 그것들 중 하나가 Gradle과 Maven이다. (Java 빌드 관리 도구에Ant 라는 도구도 있지만 이는 2000년대 초반까지만 쓰이다 이제 안 쓰인다.)







## Maven

<img src="https://user-images.githubusercontent.com/51036842/165306534-52a73da4-c2b2-46cb-be31-ce4eefe8f2a8.png" alt="image" style="zoom: 33%;" />

### 👓특징

- 프로젝트에 필요한 모든 종속성(Dependency)을 리스트 형태로 Maven에게 알려주어서 종속성을 관리한다.
- XML, Repository를 가져올 수 있다.
  - POM.xml 이라는 Maven 파일에, 필요한 Jar, Class Path를 선언만 하면 직접 다운로드 할 필요가 없이 Repository에서 자동으로 필요한 라이브러리 파일을 불러와준다.

### 🧧단점

- 라이브러리가 서로 종속할 경우 XML이 복잡해진다.
- 계층적인 데이터를 표현하기에는 좋지만, 플로우나 조건부 상황을 표현하기 어렵다.
- 편리하나 맞춤화된 로직 실행이 어렵다.



### 예시

![image](https://user-images.githubusercontent.com/51036842/165312730-c64509e7-402d-4b47-8189-7bbde5e8b739.png)



폴더 구조

![image](https://user-images.githubusercontent.com/51036842/165312566-5f7c9942-4012-4e78-b76d-a25ea49337f2.png)


pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.7</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>maven_test</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>maven_test</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```


## Gradle


![image](https://user-images.githubusercontent.com/51036842/165306791-6f7725d5-2696-4ef3-83b4-cff3692d7a48.png)

### 👓특징
- Maven의 단점을 보완
- 오픈소스 기반의 Build 자동화 도구
- 설정을 위해 groovy 언어를 사용한다. (이는 JVM 언어이다. DSL(Domain specific language), 즉 도메인 특화 언어이다.) 
- 외부 라이브러리를 관리할 수 있다.
- 캐싱이 잘된다. 그래서 **성능이 뛰어나다**.
- Maven에 비해 짧고 읽기 쉽다.


####  성능은 Gradle이 Maven 보다 빌드에 소요되는 시간, 유연성, 종속성 관리 등 다양한 측면에서 뛰어나다



#### 라이브러리 의존성 관리?

- Gradle이 Maven보다 더 효율적이고 강력한 기능을 제공하고 있다.
- Gradle은 버전 충돌 또한 관리해준다.

### 예시
![image](https://user-images.githubusercontent.com/51036842/165310975-14f1ca47-8862-4060-8325-c697271a80dc.png)

![image](https://user-images.githubusercontent.com/51036842/165311143-dde43e4d-6498-4027-bfa2-efde5235d15e.png)

폴더 구성

![image](https://user-images.githubusercontent.com/51036842/165320718-712f3d2d-3214-4104-9014-83b34fc810ae.png)




build.gradle
```groovy
plugins {
	id 'org.springframework.boot' version '2.6.7'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}
```



- build.gradle 파일은 의존성이나 플러그인 설정 등을 위한 스크립트 파일이다.
- settings.gradle 파일은 프로젝트의 구성 정보를 기록하는 파일이다. 
  - 어떤 하위프로젝트들이 어떤 관계로 구성되어 있는지를 기술한다. Gradle은 이 파일에 기술된대로 프로젝트를 구성한다.



### 코드로 비교

![image](https://user-images.githubusercontent.com/51036842/165315081-2b40975b-1589-410d-a7e7-c39ef5e46f42.png)

---

 

### 🎁참고

### Maven 구조

#### gradle.bat 파일

- 원도우용 실행 배치 스크립트.
- 원도우에서 실행 가능하다는 점만 제외하면 gradlew와 동일.

#### gradle/wrapper/gradle-wrapper.jar 파일

- Wrapper 파일.
- gradlew나 gradlew.bat 파일이 프로젝트 내에 설치하는 이 파일을 사용하여 gradle task를 실행하기 때문에 로컬 환경의 영향을 받지 않음.**(실제로는 Wrapper 버전에 맞는 구성들을 로컬 캐시에 다운로드 받음)**

#### gradle/wrapper/gradle-wrapper.properties 파일

- Gradle Wrapper 설정 파일.
- 이 파일의 wrapper 버전 등을 변경하면 task 실행시, 자동으로 새로운 Wrapper 파일을 로컬 캐시에 다운로드 받음.

#### build.grdle 파일

- 의존성이나 플러그인 설정 등을 위한 스크립트 파일.

#### settings.gradle 파일

- 프로젝트의 구성 정보를 기록하는 파일.
- 어떤 하위프로젝트들이 어떤 관계로 구성되어 있는지를 기술.
- Gradle은 이 파일에 기술된대로 프로젝트를 구성함.

#### 의존성 관리

- repositories를 사용해서 의존성을 가져올 주소를 설정.
- dependencies를 사용해서 설정된 Repository에서 가져올 아티팩트를 설정.






참고 자료

- 스티치의 빌드와 배포([[10분 테코톡\] 🐳스티치의 빌드와 배포 - YouTube](https://www.youtube.com/watch?v=6SvUZqbU37E))

- 빌드 캐시 gradle 홈페이지  (https://docs.gradle.org/current/userguide/build_cache.html)

- Gradle 빌드 시스템 기초 (https://effectivesquid.tistory.com/entry/Gradle-%EB%B9%8C%EB%93%9C%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B8%B0%EC%B4%88)
