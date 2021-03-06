---
emoji: ๐ฆ
title: ์ฑ๊ธํด ํจํด (Singleton Pattern)
date: '2022-05-16 01:12:00'
author: ์ฐ์ง
tags: ๋์์ธํจํด
categories: DESIGN_PATTERN
---

<br/>

### ๐ฅ ์ฑ๊ธํด ํจํด ์ค๊ณ๊ฐ ํ์ํ ๊ฒฝ์ฐ
: ์ค๋ ๋ ํ, ์บ์, ๋ํ์์, ์ฌ์ฉ์ ์ค์ , ๋ก๊ทธ ๊ธฐ๋ก ๊ฐ์ฒด, ๋๋ฐ์ด์ค ๋๋ผ์ด๋ฒ

<br/>

### ๐ฅ ์ฑ๊ธํด ํจํด

: ํด๋์ค `์ธ์คํด์ค๋ฅผ ํ๋๋ง ์์ฑ`, ๊ทธ ์ธ์คํด์ค๋ก์ ์ ์ญ ์ ๊ทผ์ ์ ๊ณตํ๋ ํจํด

- ํด๋์ค ๋ด์์ ํ๋๋ฟ์ธ ์ธ์คํด์ค๋ฅผ ๊ด๋ฆฌํ๋๋ก ์ค๊ณ
- ๋ค๋ฅธ ํด๋์ค์์์ ์ธ์คํด์ค ์์ฑ์ ๋ง์
- ์ธ์คํด์ค๊ฐ ํ์ํ๋ฉด ๋ฐ๋์ ๊ตฌํ ํด๋์ค๋ฅผ ๊ฑฐ์ณ์ผ ํจ
- ์ด๋์๋  ์ ๊ทผ ๊ฐ๋ฅํ๊ฒ ์ ์ญ ์ ๊ทผ ์ง์  ์ ๊ณต
- lazy instantiation ๋ฐฉ์์ผ๋ก ์ธ์คํด์ค๋ฅผ ์์ฑํ๋ฏ๋ก ์์์ ๋ง์ด ์ฌ์ฉํ๋ ๊ฒฝ์ฐ ์ ์ฉ


<br/>

`์ฑ๊ธํด ํจํด ๊ตฌํ์ ์ ์ญ๋ณ์๋ฅผ ์ฌ์ฉํ๋ฉด ์๋๋ ์ด์ `

: ํ๋ก๊ทธ๋จ ์คํ ์ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ผ๊ฐ๋ฏ๋ก ์ฌ์ฉํ์ง ์๋ ์๊ฐ์๋ ์์์ ์ฐจ์ง

  โ ์ฑ๊ธํด ํจํด์ ์ฌ์ฉํ๋ฉด ํ์ํ  ๋๋ง ๊ฐ์ฒด ์์ฑ ๊ฐ๋ฅ

<br/>

<br/>



### ๐ฅ ๊ณ ์ ์ ์ธ ์ฑ๊ธํด ํจํด ๊ตฌํ ๋ฐฉ๋ฒ

- ํด๋์ค ๋ด๋ถ์์๋ง ์ธ์คํด์ค๋ฅผ ์์ฑ ๊ฐ๋ฅํ๊ฒ `์์ฑ์๋ฅผ private๋ก ์ ์ธ`
- `์ ์  ๋ฉ์๋`์์ ์ธ์คํด์ค ๋ณ์์ ์ธ์คํด์ค๋ฅผ ์์ฑํด ๋ฆฌํด

```java
public class Singleton {

	private static Singleton uniqueInstance;
	//์ฑ๊ธํด ํจํด์ ํ๋๋ฟ์ธ ์ธ์คํด์ค ์ ์ฅํ๋ ์ ์  ๋ณ์

	private Singleton(){}
	//์์ฑ์๊ฐ private์ด๋ฏ๋ก Singleton ํด๋์ค ๋ด์์๋ง ์ธ์คํด์ค ์์ฑ ๊ฐ๋ฅ

	public static Singleton getInstance() {

		if(uniqueInstance == null){
			uniqueInstance = new Singleton();
		}
		//ํธ์ถ ์ ์ธ์คํด์ค๋ฅผ ์์ฑํจ (lazy instantiation - ๊ฒ์ผ๋ฅธ ์ธ์คํด์ค ์์ฑ)

		return uniqueInstance;
	}

}
```

<br/>

<br/>

### ๐ฅ ๋ฉํฐ์ฐ๋ ๋ฉ ๋ฌธ์ 

๋ฉํฐ ์ฐ๋ ๋๋ฅผ ์ฌ์ฉํ๋ฉด uniqueInstance์ ์ ์ผํ ์ธ์คํด์ค๊ฐ ์์ฑ๋๊ธฐ ์ง์ ์ ๋ ์ฐ๋ ๋์์ if(uniqueInstance == null) ์ ์์ฐจ์ ์ผ๋ก ๋ค์ด๊ฐ์ `์ธ์คํด์ค๊ฐ ๋๊ฐ ์์ฑ๋๋ ๊ฒฝ์ฐ`๊ฐ ๋ฐ์ํ  ์ ์๋ค.

โ getInstance() ๋ฉ์๋๋ฅผ `๋๊ธฐํ`ํ๋ฉด ๋ฉํฐ์ฐ๋ ๋ฉ ๋ฌธ์  ํด๊ฒฐ ๊ฐ๋ฅ

```java
public class Singleton {

	private static Singleton uniqueInstance;
	//์ฑ๊ธํด ํจํด์ ํ๋๋ฟ์ธ ์ธ์คํด์ค ์ ์ฅํ๋ ์ ์  ๋ณ์

	private Singleton(){}
	//์์ฑ์๊ฐ private์ด๋ฏ๋ก Singleton ํด๋์ค ๋ด์์๋ง ์ธ์คํด์ค ์์ฑ ๊ฐ๋ฅ

	public static synchronized Singleton getInstance() {

		if(uniqueInstance == null){
			uniqueInstance = new Singleton();
		}
		//ํธ์ถ ์ ์ธ์คํด์ค๋ฅผ ์์ฑํจ (lazy instantiation - ๊ฒ์ผ๋ฅธ ์ธ์คํด์ค ์์ฑ)

		return uniqueInstance;
	}

}
```

<br/>

<br/>

โ๏ธ `๋๊ธฐํ ํ์ ์์ ์ ๊ณ ๋ฏผํ์`
    
์ค์ ๋ก ๋๊ธฐํ๊ฐ ํ์ํ๊ฑด getInstance() ๋ฉ์๋ ์ํ๋  ๋๋ง๋ค๊ฐ ์๋๋ผ, ์ต์ด ์ํ์์ !
    
๋ฉ์๋๋ฅผ synchronized ๋ก ์ ์ธํ๋๊ฑด ๋ถํ์ํ ์ค๋ฒํค๋ ์ฆ๊ฐ๋ฅผ ์ผ๊ธฐ 
    
(์๋๊ฐ ์ค์ํ์ง ์๋ค๋ฉด ๊ทธ๋ฅ ์ฌ์ฉ ๊ฐ๋ฅ)
    

<br/>

<br/>

### ๐ฅ ๋๊ธฐํ ์ฌ์ฉํ์ง ์๋ ๋ฉํฐ ์ฐ๋ ๋ฉ ํด๊ฒฐ 

<br/>

### ๋ฐฉ๋ฒ 1 โ lazy instantiation ํฌ๊ธฐ

- Singleton ์ธ์คํด์ค๋ฅผ ์์ฑํ๊ณ  ๊ณ์ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ (lazy instantiation ๋ก ์์ฑํ  ํ์ X)
    
- ํด๋์ค ๋ก๋ฉ ์ JVM์์ Singleton์ ํ๋๋ฟ์ธ ์ธ์คํด์ค ์์ฑ

```java
public class Singleton {
	private static Singleton uniqueInstance = new Singleton();
	
	private Singleton() {}

	public static Singleton getInstance() {
		return uniqueInstance;
	}
}
```

<br/>

### ๋ฐฉ๋ฒ2 โ DCL์ ์ฌ์ฉํ์ฌ ๋๊ธฐํ ๋ถ๋ถ ์ค์ด๊ธฐ  

- DCL(Double-Checked Locking)์ ์ฌ์ฉํ๋ฉด ์ธ์คํด์ค ์์ฑ ์์ ์๋ง ๋๊ธฐํ ์ฌ์ฉ ๊ฐ๋ฅ

```java
public class Singleton {

	private volatile static Singleton uniqueInstance;
	//volatile ๋ณ์๋ cpu cache ์ฌ์ฉ ์์ด main memory์์ ๊ด๋ฆฌ

	private Singleton(){}

	public static Singleton getInstance() {

		if(uniqueInstance == null){
			synchronized (Singleton.class){
			//์ธ์คํด์ค ์์ ๋๋ง ๋๊ธฐํ ๋ธ๋ก ๋ค์ด๊ฐ
				if(uniqueInstance == null){
				//๋ธ๋ก ๋ด๋ถ์์๋ ๋ค์ ํ๋ฒ ์ธ์คํด์ค ์์ฑ ์ฌ๋ถ ์ฒดํฌ
					uniqueInstance = new Singleton();
				}
			}
		}

		return uniqueInstance;
	}
}
```

<br/>

<br/>

### **โ๏ธ์ฑ๊ธํด ํจํด์ ์๋ ์๋ฆฌ๋ฅผ ํ์ํ๋ค๋ฉด โ enum์ ์ฌ์ฉํด ์ฑ๊ธํด์ ๊ตฌํํ์โ๏ธ**

Enum Singleton ์ Thread-safety, Serialization ๋ณด์ฅ ๋ฐ Reflection์ ํตํ ๊ณต๊ฒฉ์๋ ์์ 

โญ๏ธ ์ฑ๊ธํด ํจํด์ ๊ตฌํํ๋ ์ต์ ์ ๋ฐฉ๋ฒ

```java
public enum Singleton{
	UNIQUE_INSTANCE;
}

public class SingletonClient {
	public static void main(String[] args)
		Singleton singleton = Singleton.UNIQUE_INSTANCE;
	}
}
```

<br/>
<br/>

### ๐ฅ ์ฑ๊ธํด ํจํด ๊ตฌํ์ ๋ฌธ์ ์ 
- ๋ฆฌํ๋์, ์ง๋ ฌํ, ์ญ์ง๋ ฌํ๋ ๋ฌธ์ ๊ฐ ๋  ์ ์์
- Loose Coupling ์๋ฐฐ (์ฑ๊ธํด ๋ฌธ์ ์ ์ผ๋ก ์ข์ข ์ ๊ธฐ)
- SRP(๋จ์ผ์ฑ์์์น) ์๋ฐฐ
- ์๋ธํด๋์ค ์์ฑ ๋ถ๊ฐ๋ฅ

<br/>

---
## References

[ํค๋ ํผ์คํธ ๋์์ธ ํจํด](http://www.yes24.com/Product/Goods/108192370)

<br/>

---