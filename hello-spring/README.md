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

**주의!** 가급적 JDK 11버전을 설치를 권장한다. 다른 버전을 설치하면 정상 작동하지 않을 가능성이 높음. 

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

IntelliJ에서 자바 실행이 잘 안되면 다음 부분을 확인하자.(일반적으로 자동으로 설정이 되어 있지만, 가끔 문제가 되는 경우에 참고)

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
2. `./gradlew` -> `gradlew.bat`를 실행하면 된다.
3. 명령 프롬프트에서 `gradlew.bat`를 실행하려면 `gradlew`하고 엔터를 치면 된다.
4. `gradlew build`
5. 폴더 목록 확인 `ls` -> `dir`
- 윈도우에서 Git bash 터미널 사용하기
   - 링크: https://www.inflearn.com/questions/53961

<br>

## 2. 스프링 웹 개발 기초
### 2-1. 정적 컨텐츠
- 스프링 부트 정적 컨텐츠 기능
- https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content

`resources/static/hello-static.html`
```Html
<!DOCTYPE HTML>
<html>
<head>
  <title>static content</title>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<body>
  정적 컨텐츠 입니다.
</body>
</html>
```
**실행**
- http://localhost:8080/hello-static.html

### 2-2. MVC와 템플릿 엔진
- MVC: Model, View, Controller

**Controller**
```Java
@Controller
public class HelloController {
  
  @GetMapping("hello-mvc")
  public String helloMvc(@RequestParam("name") String name, Model model) {
    model.addAttribute("name", name);
    return "hello-template";
  }
}
```
**View**

`resources/template/hello-template.html`
```Html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```
**실행**
- http://localhost:8080/hello-mvc?name=spring

**MVC, 템플릿 엔진 이미지**

![](src/main/resources/static/image/mvc-template-engine.png)

### 2-3. API
**@ResponseBody 문자 반환**
```Java
@Controller
public class HelloController {
  @GetMapping("hello-string")
  @ResponseBody
  public String helloString(@RequestParam("name") String name) {
      return "hello " + name;
  }
}
```
- `@ResponseBody`를 사용하면 뷰 리졸버(`viewResolver`)를 사용하지 않음
- 대신에 HTTP의 BODY에 문자 내용을 직접 반환(HTML BODY TAG를 말하는 것이 아님)

**실행**
- http://localhost:8080/hello-string?name=spring

**ResponseBody 객체 반환**
```Java
@Controller
public class HelloController {

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
}
```
**실행**
- http://localhost:8080/hello-api?name=spring

**@ResponseBody 사용 원리**

![](src/main/resources/static/image/ResponseBody-principle.png)

- `@ResponseBody`를 사용
  - HTTP의 BODY에 문자 내용을 직접 반환
  - `viewResolver` 대신에 `HttpMessageConverter`가 동작
  - 기본 문자처리: `StringHttpMessageConverter`
  - 기본 객체처리: `MappingJackson2HttpMessageConverter`
  - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

**참고: 클라이언트 HTTP Accept 헤더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해서 `HttpMessageConverter`가 선택된다.**

<br>

## 3. 회원 관리 예제 - 백엔드 개발
### 3-1. 비즈니스 요구사항 정리
- 데이터: 회원ID, 이름
- 기능: 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)

**일반적인 웹 애플리케이션 계층 구조**

![](src/main/resources/static/image/layer-of-web-app.png)

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스: 핵심 비즈니스 로직 구현
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인: 비즈니스 도메인 객체, 예) 회원 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

**클래스 의존관계**

![](src/main/resources/static/image/dependency-relationship.png)

- 아직 데이터 저장소가 선정되지 않아서, 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계
- 데이터 저장소는 RDB, NoSQL 등등 다양한 저장소를 고민중인 상황으로 가정
- 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용

### 3-2. 회원 도메인과 리포지토리 만들기
**회원 객체**
```Java
package hello.hellospring.domain;

public class Member {
  
  private Long id;
  private String name;
      
  public Long getId() {
    return id;
  }

  public void setId(Long id) {
    this.id = id;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
      this.name = name;
  } 
}
```

**회원 리포지토리 인터페이스**
```Java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {

  Member save(Member member);
  Optional<Member> findById(Long id);
  Optional<Member> findByName(String name);
  List<Member> findAll();

}
```

**회원 리포지토리 메모리 구현체**
```Java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.*;

 /**
* 동시성 문제가 고려되어 있지 않음, 실무에서는 ConcurrentHashMap, AtomicLong 사용 고려
*/
public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    public void clearStore() {
        store.clear();
    } 
}
```

### 3-3. 회원 리포지토리 테스트 케이스 작성
개발한 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다. 이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는 단점이 있다. 자바는 JUnit이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.

**회원 리포지토리 메모리 구현체 테스트**

`src/test/java`하위 폴더에 생성한다.
```Java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.Optional;

import static org.assertj.core.api.Assertions.*;

class MemoryMemberRepositoryTest {
  
  MemoryMemberRepository repository = new MemoryMemberRepository();

  @AfterEach
  public void afterEach() {
      repository.clearStore();
  }

  @Test
  public void save() {
      //given
      Member member = new Member();
      member.setName("spring");
      
      //when
      repository.save(member);

      //then
      Member result = repository.findById(member.getId()).get();
      assertThat(result).isEqualTo(member);
  }

  @Test
  public void findByName() {
      //given
      Member member1 = new Member();
      member1.setName("spring1");
      repository.save(member1);

      Member member2 = new Member();
      member2.setName("spring2");
      repository.save(member2);

      //when
      Member result = repository.findByName("spring1").get();

      //then
      assertThat(result).isEqualTo(member1);
  }

  @Test
  public void findAll() {
      //given
      Member member1 = new Member();
      member1.setName("spring1");
      repository.save(member1);

      Member member2 = new Member();
      member2.setName("spring2");
      repository.save(member2);

      //when
      List<Member> result = repository.findAll();
     
     //then
      assertThat(result.size()).isEqualTo(2);    
  }
}
```
- `@AfterEach`: 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있다. 이렇게 되면 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있다. `@AfterEach`를 사용하면 각 테스트가 종료될 때 마다 이 기능을 실행한다. 여기서는 메모리 DB에 저장된 데이터를 삭제한다.
- 테스틑 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.

### 3-4. 회원 서비스 개발
```Java
package hello.hellospring.service;
  
import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
  
import java.util.List;
import java.util.Optional;

public class MemberService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /**
    * 회원가입
    */
    public Long join(Member member) {
      
      validateDuplicateMember(member); //중복 회원 검증 
      memberRepository.save(member);
      return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    /**
    *전체 회원 조회
    */
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    /**
    회원 조회
    */
    public Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    } 
}
```

### 3-5. 회원 서비스 테스트

**기존에는 회원 서비스가 메모리 회원 리포지토리를 직접 생성하게 했다.**
```Java
public class MemberService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();

}
```
**회원 리포지토리의 코드가 회원 서비스 코드를 DI 가능하게 변경한다.**
```Java
public class MemberService {

    private final MemberRepository memberRepository;

        public MemberService(MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
        }
        ... 
}
```

**회원 서비스 테스트**
```Java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {
  
  MemberService memberService;
  MemoryMemberRepository memberRepository;
      
  @BeforeEach
  public void beforeEach() {
      memberRepository = new MemoryMemberRepository();
      memberService = new MemberService(memberRepository);
  }

  @AfterEach
  public void afterEach() {
      memberRepository.clearStore();
  }

  @Test
  public void 회원가입() throws Exception {
      //Given
      Member member = new Member();
      member.setName("hello");

      //When
      Long saveId = memberService.join(member);

      //Then
      Member findMember = memberRepository.findById(saveId).get();
      assertEquals(member.getName(), findMember.getName());
  }

  @Test
  public void 중복_회원_예외() throws Exception {
      //Given
      Member member1 = new Member();
      member1.setName("spring");
          
      Member member2 = new Member();
      member2.setName("spring");

      //When
      memberService.join(member1);
      IllegalStateException e = assertThrows(IllegalStateException.class,
              () -> memberService.join(member2)); // 예외가 발생해야 한다. 
              
      assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
  } 
}
```

<br>

## 4. 스프링 빈과 의존관계
### 4-1. 컴포넌트 스캔과 자동 의존관계 설정 
회원 컨트롤러가 회원서비스와 회원 리포지토리를 사용할 수 있게 의존관계를 준비하자.

**회원 컨트롤러에 의존관계 추가**

```Java
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {
    
    private final MemberService memberService;
      
    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```
- 생성자에 `@Autowired`가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존관계를 외부에서 넣어주는 것을 DI(Dependency Injection), 의존성 주입이라 한다.
- 이전 테스트에서는 개발자가 직접 주입했고, 여기서는 @Autowired에 의해 스프링이 주입해준다.

**오류 발생**
```
Consider defining a bean of type 'hello.hellospring.service.MemberService' in your configuration.
```
**memberService가 스프링 빈으로 등록되어 있지 않다.**

> 참고: helloController는 스프링이 제공하는 컨트롤러여서 스프링 빈으로 자동 등록된다 ->
`@Controller`가 있으면 자동 등록!

**스프링 빈을 등록하는 2가지 방법**
- 컴포넌트 스캔과 자동 의존관계 설정
- 자바 코드로 직접 스프링 빈 동륵하기

**컴포넌트 스캔 원리**
- `@Component` 어노테이션이 있으면 스프링 빈으로 자동 등록된다.
- `@Controller` 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.

**`@Component`를 포함하는 다음 어노테이션도 스프링 빈으로 자동 등록된다.**
- `@Controller`
- `@Service`
- `@Repository`

**회원 서비스 스프링 빈 등록**

```Java
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```
> 참고: 생성자에 `@Autowired`를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. (생성자가 1개일 경우 `@Autowired`는 생략 가능하다.)

**회원 리포지토리 스프링 빈 등록**

```Java
@Repository
public class MemoryMemberRepository implements MemberRepository {}
```
- `memberService`와 `memberRepository`가 스프링 컨테이너에 스프링 빈으로 등록되었다.

> 참고: 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다.(유일하기 하나만 등록해서 공유한다.) 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.

### 4-2. 자바 코드로 직접 스프링 빈 등록하기
- 회원 서비스와 회원 리포지토리의 @Service, @Repository, @Autowired 어노테이션을 제거하고 진행한다.

```java
package hello.hellospring;

import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }
      
    @Bean
    public MemberRepository memberRepository() {
      return new MemoryMemberRepository();
    }
}
```
**여기서는 향후 메모리 리포지토리를 다른 리포지토리로 변경할 예정이므로, 컴포넌트 스캔 방식 대신에 자바 코드로 스프링 빈을 설정하겠다.**

> 참고: XML로 설정하는 방식도 있지만 최근에는 잘 사용하지 않으므로 생략한다.

> 참고: DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.

> 참고: 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다. 그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.

> 주의: `@Autowired`를 통한 DI는 `helloController`, `memberService` 등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.

<br>

## 5. 회원 관리 예제 - 웹 MVC 개발

### 5-1. 회원 웹 기능 - 홈 화면 추가

**홈 컨트롤러 추가**
```Java
package hello.hellospring.controller;
    
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {     
          return "home";
    }
}
```

**회원 관리용 홈**
```Html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>

<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p> 
    </div>
</div> <!-- /container -->

</body>
</html>
```
> 참고: 컨트롤러가 정적 파일보다 우선순위가 높다.
> 
### 5-2. 회원 웹 기능 - 등록

[회원 등록 폼 개발]

**회원 등록 폼 컨트롤러**
```Java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @GetMapping(value = "/members/new")
    public String createForm() {
        return "members/createMemberForm";
    }

}
```

**회원 등록 폼 HTML**(`resources/templates/members/createMemberForm`)
```Html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <form action="/members/new" method="post">
        <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" name="name" placeholder="이름을 입력하세요"> 
        </div>
        <button type="submit">등록</button> 
    </form>
</div> <!-- /container -->
</body>
</html>
```

[회원 등록 컨트롤러]

**웹 등록 화면에서 데이터를 전달 받을 폼 객체**
```Java
package hello.hellospring.controller;

public class MemberForm {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    } 
}
```

**회원 컨트롤러에서 회원을 실제 등록하는 기능**
```Java
@PostMapping(value = "/members/new")
public String create(MemberForm form) {

    Member member = new Member();
    member.setName(form.getName());

    memberService.join(member);

    return "redirect:/";
}
```

### 5-3. 회원 웹 기능 - 조회
**회원 컨트롤러에서 조회 기능**
```Java
@GetMapping(value = "/members")
public String list(Model model) {
    List<Member> members = memberService.findMembers();
    model.addAttribute("members", members);
    return "members/memberList";
}
```

**회원 리스트 HTML**
```Html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>

<div class="container">
    <div>
        <table>
            <thead>
            <tr>
                <th>#</th>
                <th>이름</th> 
            </tr>
            </thead>
            <tbody>
            <tr th:each="member : ${members}">
                <td th:text="${member.id}"></td>
                <td th:text="${member.name}"></td>
            </tr>
            </tbody>
        </table>
    </div>

</div> <!-- /container -->
</body>
</html>
```

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