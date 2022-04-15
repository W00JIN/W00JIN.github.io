---
emoji: 🦖
title: Spring 컴포넌트 스캔
date: '2022-04-16 00:10:00'
author: 우진
tags: blog
categories: Spring
---

<br/>

## 1) 컴포넌트 스캔과 의존관계 자동 주입

<br/>

### 🥎 컴포넌트 스캔 : 설정정보가 없어도 자동으로 스프링 빈을 등록하는 기능

- 스프링 빈 누락 방지
- 설정에 소요하는 시간, 설정정보 크기 감소

<br/>

### 🥎 @ComponentScan 어노테이션 사용 : 자동으로 스프링 빈을 등록하는 설정파일 명시

- @Bean으로 등록된 설정 정보가 존재하지 않음
- @Component 어노테이션이 붙은 모든 클래스를 스프링 빈으로 등록
- `빈 이름 기본전략` : 클래스명의 맨 앞글자를 소문자로 변경
- `빈 이름 직접 지정` : @Component(”지정할 빈 이름”) 형태로 지정

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan

public class AutoAppConfig {
}
```

<br/>

### 🥎 @Component 어노테이션 사용 : Scan될 컴포넌트를 명시

- @ComponentScan 어노테이션에 의해 클래스가 스캔됨

<br/>

### 🥎 @Autowired 어노테이션 사용 : 의존관계 자동 주입

- 의존관계를 직접 명시할 설정파일을 대신하여 자동으로 의존관계 주입
- 생성자에서 여러 의존관계도 한번에 주입 가능
- 생성자에 @Autowired 를 지정하면, 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입
- 기본 조회 전략 : 타입이 같은 빈을 찾아서 주입 - getBean(MemberRepository.class) 와 동일

<br/>

### 🥎 테스트

- AnnotationConfigApplicationContext 설정 정보로 AutoAppConfig.class를 넣어줌
- 로그를 통해 스캔된 컴포넌트 및 의존성 자동 주입 확인 가능

```java
import hello.core.AutoAppConfig;
import hello.core.member.MemberService;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContex;
import static org.assertj.core.api.Assertions.*;

public class AutoAppConfigTest {
	@Test
    void basicScan() {
		ApplicationContext ac = new
		AnnotationConfigApplicationContext(AutoAppConfig.class);
        
		MemberService memberService = ac.getBean(MemberService.class);
	    assertThat(memberService).isInstanceOf(MemberService.class);
    }
}
```

<br/><br/>

---

<br/>

## 2) 탐색위치와 기본 스캔 대상

<br/>

### 🥎 @ComponentScan의 탐색 위치 지정 가능

- 지정하지 않으면 default는 @ComponentScan 설정정보 클래스의 패키지가 시작 위치, 하위 패키지를 모두 탐색
- 프로젝트 최상단에 설정정보 클래스를 두는 방법을 권장 (프로젝트 대표 정보이기 때문에 + 하위 패키지 컴포넌트를 위치 지정 없이 스캔 가능)
- 스프링부트의 시작 정보인 @SpringBootApplication에 이미 @ComponentScan이 포함되어있기 때문에 시작 정보 파일을 프로젝트 시작 루트에 두는 것이 관례

<br/>

### 🥎 @ComponentScan 포함 어노테이션

- @Component : 컴포넌트 스캔에서 사용
- @Controlller : 스프링 MVC 컨트롤러로 인식
- @Service : 스프링 비즈니스 로직에서 사용, 특별한 처리는 X 비즈니스 계층 인식 기능
- @Repository : 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환
- @Configuration : 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가

> 참고: 애노테이션에는 상속관계 X 이렇게 애노테이션이 특정 애노테이션을 들고 있는 것을 인식할 수 있는 것은 자바 언어가 지원하는 기능은 아니고, 스프링이 지원하는 기능

<br/>

> 참고: useDefaultFilters 옵션은 기본으로 켜져있는데, 이 옵션을 끄면 기본 스캔 대상들이 제외됨

<br/><br/>

---

<br/>

## 3) 필터

`includeFilters` : 컴포넌트 스캔 대상을 추가로 지정, @Component 면 충분하기 때문에 잘 사용 X

`excludeFilters` : 컴포넌트 스캔에서 제외할 대상을 지정

<br/>

### 🥎 컴포넌트 스캔 대상에 추가할 애노테이션

```java
package hello.core.scan.filter;
import java.lang.annotation.*;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyIncludeComponent {
}
```

<br/>

### 🥎 컴포넌트 스캔 대상에서 제외할 애노테이션

```java
package hello.core.scan.filter;
import java.lang.annotation.*;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface MyExcludeComponent {
}

```

<br/>

### 🥎 설정파일에 등록

```java
@Configuration
@ComponentScan(
	includeFilters = @Filter(type = FilterType.ANNOTATION, classes =
MyIncludeComponent.class),
	excludeFilters = @Filter(type = FilterType.ANNOTATION, classes =
MyExcludeComponent.class)
)
public class ComponentFilterAppConfig {
}
```

<br/><br/>

---

<br/>

## 4) 중복 등록과 충돌

컴포넌트 스캔에서 같은 빈 이름을 등록한 경우 

### 🥎 자동 빈 등록 vs 자동 빈 등록

ConflictingBeanDefinitionException 예외 발생

### 🥎 자동 빈 등록 vs 수동 빈 등록

수동 빈이 자동 빈을 오버라이딩 하여 수동 빈 등록이 우선권을 가짐

<br/>

로그를 통해 replacing 된 정보 확인 가능

> 참고 : 최근 스프링 부트는 @SpringBootApplication 내의 @ComponentScan에서 수동 빈 등록과 자동 빈 등록이 충돌나면 오류 발생하도록 기본값 변경 (설정 정보가 꼬인 버그는 정말 잡기 어렵기 때문에)

<br/>

---
## References

[스프링 핵심 원리 - 기본편 (김영한)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)

<br/>

---
