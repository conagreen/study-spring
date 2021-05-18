# 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술

[해당 강의 바로가기](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)

해당 강의는 예제를 만들어가면서 스프링 웹 애플리케이션 개발 전반을 빠르게 학습할 수 있는 강의로 스프링 입문자에게 적합한 수준의 강의입니다. **입문 강의인 만큼 강의자료를 토대로 프로젝트 환경설정부터 강의의 모든 내용을 기록합니다. 스프링 기본강의부터는 학습한 내용을 요약하여 정리 후 회고와 함께 업로드할 예정입니다.**

모든 코드는 저의 [Github 저장소](https://github.com/conagreen/study-spring/tree/main/hello-spring)에서 확인하실 수 있습니다.

## [ 목차 ]

<br>

## 1. 프로젝트 환경설정

### 1-1. 프로젝트 생성
**사전 준비물**
>- Java 11 설치
>- IDE: IntelliJ 또는 Eclipse 설치 (저는 IntelliJ IDEA Ultimate 버전으로 진행하였습니다.)

**주의!** 가급적 JDK 11버전을 설치해주세요. 다른 버전을 설치하면 정상 작동하지 않을 가능성이 높습니다. 

- 프로젝트 선택
>- Project: Gradle Project
>- Spring Boot: 2.3.x
>- Language: Java
>- Packaging: Jar
>- Java: 11

- Project Metadata
>- groupId: hello
>- artifactId: hello-spring

- Dependencies: Spring Web, Thymeleaf

**Gradle 전체 설정**

`build.gradle`
```
plugins {
    id 'org.springframework.boot' version '2.3.1.RELEASE'
    id 'io.spring.dependency-management' version '1.0.9.RELEASE'
    id 'java'
}
  group = 'hello'
  version = '0.0.1-SNAPSHOT'
  sourceCompatibility = '11'
  
  repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation('org.springframework.boot:spring-boot-starter-test') {
        exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }
}
  test {
    useJUnitPlatform()
}
```
- 동작 확인
>- 기본 메인 클래스 실행
>- 스프링 부트 메인 실행 후 에러페이지로 간단하게 동작 확인 (http://localhost:8080)

**IntelliJ Gradle 대신 자바 직접 실행**

최근 IntelltJ 버전은 Gradle을 통해서 실행 하는 것이 기본 설정이다. 이렇게 하면 실행속도가 느리다. 다음과 같이 변경하면 자바로 바로 실행해서 실행속도가 더 빠르다.

- Preferences -> Build, Execution, Deployment -> Build Tools -> Gradle
>- Build and run using: Gradle -> IntelliJIDEA
>- Run tests using: Gradle -> IntelliJ IDEA

**IntelliJ JDK 설치 확인**

IntelliJ에서 자바 실행이 잘 안되면 다음 부분을 확인해주세요.(일반적으로 자동으로 설정이 되어 있지만, 가끔 문제가 되는 경우에 참고하시면 됩니다.)

- 프로젝트 JDK 설정 -> 11로 지정
- Gradle JDK 설정 -> 11로 지정

### 1-2. 라이브러리 살펴보기
`Gradle은 의존관계가 있는 라이브러리를 함께 다운로드 한다.`

**스프링 부트 라이브러리**
- spring-boot-starter-web
  - spring-boot-starter-tomcat: 톰캣 (웹서버)
  - spring-webmvc: 스프링 웹 MVC
- spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
- spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback, slf4j

**테스트 라이브러리**
- spring-boot-starter-test
  - junit: 테스트 프레임워크
  - mockito: 목 라이브러리
  - assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  - spring-test: 스프링 통합 테스트 지원

### 1-3. View 환경설정
**Welcome Page 만들기**
`resources/static/index.html`
```Html
<!DOCTYPE HTML>
<html>
   <head>
      <title>Hello</title>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
  Hello
  <a href="/hello">hello</a>
  </body>
</html>
```

- 스프링 부트가 제공하는 Welcome Page 기능
>- `static/index.html`을 올려두면 Welcome page 기능을 제공한다.
>- 참고: https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-welcome-page

**thymeleaf 템플릿 엔진**
- thymeleaf 공식 사이트: https://www.thymeleaf.org/
- 스프링 공식 튜토리얼: https://spring.io/guides/gs/serving-web-content/
- 스프링부트 메뉴얼: https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-template-engines

```Java
@Controller
  public class HelloController {
      @GetMapping("hello")
      public String hello(Model model) {
          model.addAttribute("data", "hello!!");
          return "hello";
      }
}
```

`resources/templates/hello.html`
```Html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
      <title>Hello</title>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```

**thymeleaf 템플릿엔진 동작 확인**
- 실행: http://localhost:8080/hello

**동작 환경 그림**

![](src/main/resources/static/image/system-requirements.png)

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(`viewResolver`)가 화면을 찾아서 처리한다.
  - 스프링 부트 템플릿엔진 기본 viewName 매핑
  - `resources: templates/` + {ViewName} + `.html`

### 1-4. 빌드하고 실행하기
**콘솔로 이동**
1. `./gradlew build`
2. `cd build/libs`
3. `java -jar hello-spring-0.0.1-SNAPSHOT.jar`
4. 실행 확인

**윈도우 사용자를 위한 팁**
1. 콘솔로 이동 -> 명령 프롬프트(cmd)로 이동
2. `./gradlew` -> `gradlew.bat`를 실행하면 됩니다.
3. 명령 프롬프트에서 `gradlew.bat`를 실행하려면 `gradlew`하고 엔터를 치면 됩니다.
4. `gradlew build`
5. 폴더 목록 확인 `ls` -> `dir`
- 윈도우에서 Git bash 터미널 사용하기
   - 링크: https://www.inflearn.com/questions/53961

<br>

## 2. 스프링 웹 개발 기초
### 2-1. 정적 컨텐츠

### 2-2. MVC와 템플릿 엔진

### 2-3. API


<br>

## 3. 회원 관리 예제 - 백엔드 개발
### 3-1. 비즈니스 요구사항 정리

### 3-2. 회원 도메인과 리포지토리 만들기

### 3-3. 회원 리포지토리 테스트 케이스 작성

### 3-4. 회원 서비스 개발

### 3-5. 회원 서비스 테스트


<br>

## 4. 스프링 빈과 의존관계
### 4-1. 컴포넌트 스캔과 자동 의존관계 설정 

### 4-2. 자바 코드로 직접 스프링 빈 등록하기


<br>

## 5. 회원 관리 예제 - 웹 MVC 개발
### 5-1. 회원 웹 기능 - 홈 화면 추가

### 5-2. 회원 웹 기능 - 등록

### 5-3. 회원 웹 기능 - 조회


<br>

## 6. 스프링 DB 접근 기술
### 6-1. H2 데이터베이스 설치

### 6-2. 순수 JDBC

### 6-3. 스프링 통합 테스트

### 6-4. 스프링 JdbcTemplate

### 6-5. JPA

### 6-6. 스프링 데이터 JPA


<br>

## 7. AOP
### 7-1. AOP가 필요한 상황

### 7-2. AOP 적용


<br>

## 8. 다음으로
### 8-1. 다음으로