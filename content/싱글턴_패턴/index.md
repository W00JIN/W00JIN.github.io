---
emoji: 🦖
title: 싱글턴 패턴 (Singleton Pattern)
date: '2022-05-16 01:12:00'
author: 우진
tags: 디자인패턴
categories: DESIGN_PATTERN
---

<br/>

### 🥎 싱글턴 패턴 설계가 필요한 경우
: 스레드 풀, 캐시, 대화상자, 사용자 설정, 로그 기록 객체, 디바이스 드라이버

<br/>

### 🥎 싱글턴 패턴

: 클래스 `인스턴스를 하나만 생성`, 그 인스턴스로의 전역 접근을 제공하는 패턴

- 클래스 내에서 하나뿐인 인스턴스를 관리하도록 설계
- 다른 클래스에서의 인스턴스 생성을 막음
- 인스턴스가 필요하면 반드시 구현 클래스를 거쳐야 함
- 어디서든 접근 가능하게 전역 접근 지점 제공
- lazy instantiation 방식으로 인스턴스를 생성하므로 자원을 많이 사용하는 경우 유용


<br/>

`싱글턴 패턴 구현에 전역변수를 사용하면 안되는 이유`

: 프로그램 실행 시 메모리에 올라가므로 사용하지 않는 순간에도 자원을 차지

  → 싱글턴 패턴을 사용하면 필요할 때만 객체 생성 가능

<br/>

<br/>



### 🥎 고전적인 싱글턴 패턴 구현 방법

- 클래스 내부에서만 인스턴스를 생성 가능하게 `생성자를 private로 선언`
- `정적 메소드`에서 인스턴스 변수에 인스턴스를 생성해 리턴

```java
public class Singleton {

	private static Singleton uniqueInstance;
	//싱글턴 패턴의 하나뿐인 인스턴스 저장하는 정적 변수

	private Singleton(){}
	//생성자가 private이므로 Singleton 클래스 내에서만 인스턴스 생성 가능

	public static Singleton getInstance() {

		if(uniqueInstance == null){
			uniqueInstance = new Singleton();
		}
		//호출 시 인스턴스를 생성함 (lazy instantiation - 게으른 인스턴스 생성)

		return uniqueInstance;
	}

}
```

<br/>

<br/>

### 🥎 멀티쓰레딩 문제

멀티 쓰레드를 사용하면 uniqueInstance에 유일한 인스턴스가 생성되기 직전에 두 쓰레드에서 if(uniqueInstance == null) 에 순차적으로 들어가서 `인스턴스가 두개 생성되는 경우`가 발생할 수 있다.

⇒ getInstance() 메소드를 `동기화`하면 멀티쓰레딩 문제 해결 가능

```java
public class Singleton {

	private static Singleton uniqueInstance;
	//싱글턴 패턴의 하나뿐인 인스턴스 저장하는 정적 변수

	private Singleton(){}
	//생성자가 private이므로 Singleton 클래스 내에서만 인스턴스 생성 가능

	public static synchronized Singleton getInstance() {

		if(uniqueInstance == null){
			uniqueInstance = new Singleton();
		}
		//호출 시 인스턴스를 생성함 (lazy instantiation - 게으른 인스턴스 생성)

		return uniqueInstance;
	}

}
```

<br/>

<br/>

❗️ `동기화 필요 시점을 고민하자`
    
실제로 동기화가 필요한건 getInstance() 메소드 수행될 때마다가 아니라, 최초 수행시점!
    
메소드를 synchronized 로 선언하는건 불필요한 오버헤드 증가를 야기 
    
(속도가 중요하지 않다면 그냥 사용 가능)
    

<br/>

<br/>

### 🥎 동기화 사용하지 않는 멀티 쓰레딩 해결 

<br/>

### 방법 1 ⇒ lazy instantiation 포기

- Singleton 인스턴스를 생성하고 계속 사용하는 경우 (lazy instantiation 로 생성할 필요 X)
    
- 클래스 로딩 시 JVM에서 Singleton의 하나뿐인 인스턴스 생성

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

### 방법2 ⇒ DCL을 사용하여 동기화 부분 줄이기  

- DCL(Double-Checked Locking)을 사용하면 인스턴스 생성 시점에만 동기화 사용 가능

```java
public class Singleton {

	private volatile static Singleton uniqueInstance;
	//volatile 변수는 cpu cache 사용 없이 main memory에서 관리

	private Singleton(){}

	public static Singleton getInstance() {

		if(uniqueInstance == null){
			synchronized (Singleton.class){
			//인스턴스 없을 때만 동기화 블록 들어감
				if(uniqueInstance == null){
				//블록 내부에서도 다시 한번 인스턴스 생성 여부 체크
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

### **❗️싱글턴 패턴의 작동 원리를 파악했다면 ⇒ enum을 사용해 싱글턴을 구현하자❗️**

Enum Singleton 은 Thread-safety, Serialization 보장 및 Reflection을 통한 공격에도 안전

⭐️ 싱글턴 패턴을 구현하는 최적의 방법

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

### 🥎 싱글턴 패턴 구현의 문제점
- 리플랙션, 직렬화, 역직렬화도 문제가 될 수 있음
- Loose Coupling 위배 (싱글턴 문제점으로 종종 제기)
- SRP(단일책임원칙) 위배
- 서브클래스 생성 불가능

<br/>

---
## References

[헤드 퍼스트 디자인 패턴](http://www.yes24.com/Product/Goods/108192370)

<br/>

---