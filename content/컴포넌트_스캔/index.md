---
emoji: ๐ฆ
title: Spring ์ปดํฌ๋ํธ ์ค์บ
date: '2022-04-16 00:10:00'
author: ์ฐ์ง
tags: ์คํ๋ง
categories: Spring
---

<br/>

## 1) ์ปดํฌ๋ํธ ์ค์บ๊ณผ ์์กด๊ด๊ณ ์๋ ์ฃผ์

<br/>

### ๐ฅย ์ปดํฌ๋ํธ ์ค์บ : ์ค์ ์ ๋ณด๊ฐ ์์ด๋ ์๋์ผ๋ก ์คํ๋ง ๋น์ ๋ฑ๋กํ๋ ๊ธฐ๋ฅ

- ์คํ๋ง ๋น ๋๋ฝ ๋ฐฉ์ง
- ์ค์ ์ ์์ํ๋ ์๊ฐ, ์ค์ ์ ๋ณด ํฌ๊ธฐ ๊ฐ์

<br/>

### ๐ฅย @ComponentScan ์ด๋ธํ์ด์ ์ฌ์ฉ : ์๋์ผ๋ก ์คํ๋ง ๋น์ ๋ฑ๋กํ๋ ์ค์ ํ์ผ ๋ช์

- @Bean์ผ๋ก ๋ฑ๋ก๋ ์ค์  ์ ๋ณด๊ฐ ์กด์ฌํ์ง ์์
- @Component ์ด๋ธํ์ด์์ด ๋ถ์ ๋ชจ๋  ํด๋์ค๋ฅผ ์คํ๋ง ๋น์ผ๋ก ๋ฑ๋ก
- `๋น ์ด๋ฆ ๊ธฐ๋ณธ์ ๋ต` : ํด๋์ค๋ช์ ๋งจ ์๊ธ์๋ฅผ ์๋ฌธ์๋ก ๋ณ๊ฒฝ
- `๋น ์ด๋ฆ ์ง์  ์ง์ ` : @Component(โ์ง์ ํ  ๋น ์ด๋ฆโ) ํํ๋ก ์ง์ 

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan

public class AutoAppConfig {
}
```

<br/>

### ๐ฅย @Component ์ด๋ธํ์ด์ ์ฌ์ฉ : Scan๋  ์ปดํฌ๋ํธ๋ฅผ ๋ช์

- @ComponentScan ์ด๋ธํ์ด์์ ์ํด ํด๋์ค๊ฐ ์ค์บ๋จ

<br/>

### ๐ฅย @Autowired ์ด๋ธํ์ด์ ์ฌ์ฉ : ์์กด๊ด๊ณ ์๋ ์ฃผ์

- ์์กด๊ด๊ณ๋ฅผ ์ง์  ๋ช์ํ  ์ค์ ํ์ผ์ ๋์ ํ์ฌ ์๋์ผ๋ก ์์กด๊ด๊ณ ์ฃผ์
- ์์ฑ์์์ ์ฌ๋ฌ ์์กด๊ด๊ณ๋ ํ๋ฒ์ ์ฃผ์ ๊ฐ๋ฅ
- ์์ฑ์์ @Autowired ๋ฅผ ์ง์ ํ๋ฉด, ์คํ๋ง ์ปจํ์ด๋๊ฐ ์๋์ผ๋ก ํด๋น ์คํ๋ง ๋น์ ์ฐพ์์ ์ฃผ์
- ๊ธฐ๋ณธ ์กฐํ ์ ๋ต : ํ์์ด ๊ฐ์ ๋น์ ์ฐพ์์ ์ฃผ์ - getBean(MemberRepository.class) ์ ๋์ผ

<br/>

### ๐ฅย ํ์คํธ

- AnnotationConfigApplicationContext ์ค์  ์ ๋ณด๋ก AutoAppConfig.class๋ฅผ ๋ฃ์ด์ค
- ๋ก๊ทธ๋ฅผ ํตํด ์ค์บ๋ ์ปดํฌ๋ํธ ๋ฐ ์์กด์ฑ ์๋ ์ฃผ์ ํ์ธ ๊ฐ๋ฅ

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

## 2) ํ์์์น์ ๊ธฐ๋ณธ ์ค์บ ๋์

<br/>

### ๐ฅย @ComponentScan์ ํ์ ์์น ์ง์  ๊ฐ๋ฅ

- ์ง์ ํ์ง ์์ผ๋ฉด default๋ @ComponentScan ์ค์ ์ ๋ณด ํด๋์ค์ ํจํค์ง๊ฐ ์์ ์์น, ํ์ ํจํค์ง๋ฅผ ๋ชจ๋ ํ์
- ํ๋ก์ ํธ ์ต์๋จ์ ์ค์ ์ ๋ณด ํด๋์ค๋ฅผ ๋๋ ๋ฐฉ๋ฒ์ ๊ถ์ฅ (ํ๋ก์ ํธ ๋ํ ์ ๋ณด์ด๊ธฐ ๋๋ฌธ์ + ํ์ ํจํค์ง ์ปดํฌ๋ํธ๋ฅผ ์์น ์ง์  ์์ด ์ค์บ ๊ฐ๋ฅ)
- ์คํ๋ง๋ถํธ์ ์์ ์ ๋ณด์ธ @SpringBootApplication์ ์ด๋ฏธ @ComponentScan์ด ํฌํจ๋์ด์๊ธฐ ๋๋ฌธ์ ์์ ์ ๋ณด ํ์ผ์ ํ๋ก์ ํธ ์์ ๋ฃจํธ์ ๋๋ ๊ฒ์ด ๊ด๋ก

<br/>

### ๐ฅย @ComponentScan ํฌํจ ์ด๋ธํ์ด์

- @Component : ์ปดํฌ๋ํธ ์ค์บ์์ ์ฌ์ฉ
- @Controlller : ์คํ๋ง MVC ์ปจํธ๋กค๋ฌ๋ก ์ธ์
- @Service : ์คํ๋ง ๋น์ฆ๋์ค ๋ก์ง์์ ์ฌ์ฉ, ํน๋ณํ ์ฒ๋ฆฌ๋ X ๋น์ฆ๋์ค ๊ณ์ธต ์ธ์ ๊ธฐ๋ฅ
- @Repository : ์คํ๋ง ๋ฐ์ดํฐ ์ ๊ทผ ๊ณ์ธต์ผ๋ก ์ธ์ํ๊ณ , ๋ฐ์ดํฐ ๊ณ์ธต์ ์์ธ๋ฅผ ์คํ๋ง ์์ธ๋ก ๋ณํ
- @Configuration : ์คํ๋ง ์ค์  ์ ๋ณด๋ก ์ธ์ํ๊ณ , ์คํ๋ง ๋น์ด ์ฑ๊ธํค์ ์ ์งํ๋๋ก ์ถ๊ฐ

>ย ์ฐธ๊ณ : ์ ๋ธํ์ด์์๋ ์์๊ด๊ณ X ์ด๋ ๊ฒ ์ ๋ธํ์ด์์ด ํน์  ์ ๋ธํ์ด์์ ๋ค๊ณ  ์๋ ๊ฒ์ ์ธ์ํ  ์ ์๋ ๊ฒ์ ์๋ฐ ์ธ์ด๊ฐ ์ง์ํ๋ ๊ธฐ๋ฅ์ ์๋๊ณ , ์คํ๋ง์ด ์ง์ํ๋ ๊ธฐ๋ฅ

<br/>

>ย ์ฐธ๊ณ : useDefaultFilters ์ต์์ ๊ธฐ๋ณธ์ผ๋ก ์ผ์ ธ์๋๋ฐ, ์ด ์ต์์ ๋๋ฉด ๊ธฐ๋ณธ ์ค์บ ๋์๋ค์ด ์ ์ธ๋จ

<br/><br/>

---

<br/>

## 3) ํํฐ

`includeFilters` : ์ปดํฌ๋ํธ ์ค์บ ๋์์ ์ถ๊ฐ๋ก ์ง์ , @Component ๋ฉด ์ถฉ๋ถํ๊ธฐ ๋๋ฌธ์ ์ ์ฌ์ฉ X

`excludeFilters` : ์ปดํฌ๋ํธ ์ค์บ์์ ์ ์ธํ  ๋์์ ์ง์ 

<br/>

### ๐ฅย ์ปดํฌ๋ํธ ์ค์บ ๋์์ ์ถ๊ฐํ  ์ ๋ธํ์ด์

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

### ๐ฅย ์ปดํฌ๋ํธ ์ค์บ ๋์์์ ์ ์ธํ  ์ ๋ธํ์ด์

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

### ๐ฅย ์ค์ ํ์ผ์ ๋ฑ๋ก

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

## 4) ์ค๋ณต ๋ฑ๋ก๊ณผ ์ถฉ๋

์ปดํฌ๋ํธ ์ค์บ์์ ๊ฐ์ ๋น ์ด๋ฆ์ ๋ฑ๋กํ ๊ฒฝ์ฐ 

### ๐ฅย ์๋ ๋น ๋ฑ๋ก vs ์๋ ๋น ๋ฑ๋ก

ConflictingBeanDefinitionException ์์ธ ๋ฐ์

### ๐ฅย ์๋ ๋น ๋ฑ๋ก vs ์๋ ๋น ๋ฑ๋ก

์๋ ๋น์ด ์๋ ๋น์ ์ค๋ฒ๋ผ์ด๋ฉ ํ์ฌ ์๋ ๋น ๋ฑ๋ก์ด ์ฐ์ ๊ถ์ ๊ฐ์ง

<br/>

๋ก๊ทธ๋ฅผ ํตํด replacing ๋ ์ ๋ณด ํ์ธ ๊ฐ๋ฅ

> ์ฐธ๊ณ  : ์ต๊ทผ ์คํ๋ง ๋ถํธ๋ @SpringBootApplication ๋ด์ @ComponentScan์์ ์๋ ๋น ๋ฑ๋ก๊ณผ ์๋ ๋น ๋ฑ๋ก์ด ์ถฉ๋๋๋ฉด ์ค๋ฅ ๋ฐ์ํ๋๋ก ๊ธฐ๋ณธ๊ฐ ๋ณ๊ฒฝ (์ค์  ์ ๋ณด๊ฐ ๊ผฌ์ธ ๋ฒ๊ทธ๋ ์ ๋ง ์ก๊ธฐ ์ด๋ ต๊ธฐ ๋๋ฌธ์)

<br/>

---
## References

[์คํ๋ง ํต์ฌ ์๋ฆฌ - ๊ธฐ๋ณธํธ (๊น์ํ)](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)

<br/>

---
