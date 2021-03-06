---
emoji: π¦
title: μ€νμ΄νΈ ν¨ν΄ (State Pattern)
date: '2022-04-20 23:50:00'
author: μ°μ§
tags: λμμΈν¨ν΄
categories: DESIGN_PATTERN
---

<br/>

### π₯ μν λ¨Έμ  λ€μ΄μ΄κ·Έλ¨ (State Machine Diagram)

- UMLμμ μνμ `μνλ³νλ₯Ό λͺ¨λΈλ§νλ λκ΅¬`
- μμ μν : κ²μ λκ·ΈλΌλ―Έ / μν : λͺ¨μλ¦¬κ° λ₯κ·Ό μ¬κ°ν / μνμ μ΄ : νμ΄ν

<br/>

**μν**

- κ°μ²΄κ° μμ€νμ μ‘΄μ¬νλ λΌμ΄ννμ λμ κ°μ²΄κ° κ°μ§ μ μλ μ‘°κ±΄μ΄λ μν©

<br/>

**μμ μν : κ°μ²΄κ° μμνλ μ²μ μν**

- μμ μνμμμ μ§μμ κ°μ²΄ μμ± λλ λͺμ X

<br/>

**μν μ§μ**

- κ°μ²΄μ ν μνμμ λ€λ₯Έ μνλ‘ μ΄λνλ κ²
- νΉμ  μ΄λ²€νΈ λ°μ ν λͺμΈ μ‘°κ±΄μ λ§μ‘±ν κ²½μ°μ μ΄λ£¨μ΄μ§
- βμ΄λ²€νΈ(μΈμ λ¦¬μ€νΈ)[μ‘°κ±΄]/μ‘μβμΌλ‘ λͺμΈ, β/β λ€μμ μ§μ ν μνλμ΄μΌ νλ μ‘μ κΈ°μ 


<br/>

---

<br/>

### π₯Β **νκ΄λ± λ§λ€κΈ°**

<br/>

**μν λ¨Έμ  λ€μ΄μ΄κ·Έλ¨**

μν : ON, OFF

![μ¬μ§](./state1.png)

μν : ON, OFF, SLEEPING

![μ¬μ§](./state2.png)

<br/>

**if-else μ‘°κ±΄λ¬ΈμΌλ‘ κ΅¬ννλ κ²½μ°μ λ¬Έμ μ **

- λ³΅μ‘ν μ‘°κ±΄λ¬ΈμΌλ‘ μν λ³νμ μ΄ν΄ μ΄λ €μ
- μλ‘μ΄ μν μΆκ° μ λͺ¨λ  λ©μλ μμ  νμ

<br/>

### π₯Β **μ½λ**

<br/>

**Light.java**

```java
public class Light {
	private static int ON = 0;
	private static int OFF = 1;
	private static int SLEEPING = 2;
	private int state;
	
	public Light() {
		state = OFF;
	}
	
	public void on_button_pushed() {
		if(state == ON) {
			System.out.println("Sleeping");
			state = SLEEPING;
		}
		else if (state == SLEEPING) {
			System.out.println("Light on");
			state = ON;
		}
		else {
			System.out.println("Light on");
			state = ON;
		}
	}

	public void off_button_pushed() {
		if(state == OFF) {
			System.out.println("Nothing happened");
		}
		else if (state == SLEEPING) {
			System.out.println("Light off");
			state = OFF;
		}
		else {
			System.out.println("Light off");
			state = OFF;
		}
	}
}

```

<br/>

**Client.java**

```java
public class Client() {
	public static void main(String[] args) {
		Light light = new Light();
		light.off();
		light.on();
		light.off();
	}
}
```

<br/>

---

<br/>

### π₯ State Pattern

: μνμ λ°λΌ λμΌν μμμ΄ λ€λ₯Έ λ°©μμΌλ‘ μ€νλ  λ ν΄λΉ μνκ° μμμ μννλλ‘ μμνλ λμμΈ ν¨ν΄

<br/>

**λͺ©ν** : νμ¬ μμ€νμ `μν,μν λ³νμ λλ¦½μ μΈ μ½λ κ΅¬ν`

<br/>

### π₯ κ΅¬ν
- Context μμκ° μ΄λ€ νμλ₯Ό μνν  λ `μν ν΄λμ€κ° νμλ₯Ό μννλλ‘ μμ` β `μμ€νμ μνμ λ¬΄κ΄` 
   μ΄λ₯Ό μν΄ μμ€νμ κ° μνλ₯Ό ν΄λμ€λ‘ λΆλ¦¬ν΄ νν

- μν ν΄λμ€λ `μν λ³νλ§λ€ μλ‘μ΄ κ°μ²΄λ₯Ό μμ±ν  νμκ° μμ` **β μ±κΈν΄ ν¨ν΄ μ¬μ©**

- μν ν΄λμ€λ₯Ό μΊ‘μν νκΈ° μν΄ μΈν°νμ΄μ€λ₯Ό λ§λ€μ΄ κ° μνλ₯Ό λνλ΄λ ν΄λμ€λ‘ μ€μ²΄ν

- μν λ³κ²½μ `μν μ€μ€λ‘ μμμ λ€μ μνλ₯Ό κ²°μ `

- κ²½μ°μ λ°λΌμλ μν λ³κ²½μ κ΄λ¦¬νλ ν΄λμ€λ₯Ό μΆκ° κ°λ₯

<br/>

![μ¬μ§](./state3.png)


<br/>

**State.java**

```java
interface State {
	public void on_button_pushed(Light light);
	public void off_button_pushed(Light light);
}
```

<br/>

**ON.java**

```java
public class ON implements State {
	private static ON on = new ON();
	private ON() {}

	public static ON getInstance() {
		return on;
	}

	public void on_button_pushed(Light light) {
		System.out.println("Nothing happened");
	}

	public void off_button_pushed(Light light) {
		light.setState(OFF.getInstance());
		System.out.println("Light off");
	}
}
```

<br/>

**OFF.java**

```java
public class OFF implements State {
	private static OFF off = new OFF();
	private OFF() {}

	public static OFF getInstance() {
		return off;
	}

	public void on_button_pushed(Light light) {
		light.setState(ON.getInstance());
		System.out.println("Light on");
	}

	public void off_button_pushed(Light light) {
		System.out.println("Nothing happened");
	}
}
```

<br/>

**Light.java**

```java
public class Light {
	private State state;

	public Light() {
		state = OFF.getInstance();
	}

	public void setState(State state) {
		this.state = state;
	}

	public void on_button_pushed() {
		state.on_button_pushed(this)
	}

	public void off_button_pushed() {
		state.off_button_pushed(this)
	}
}
```

---
## References

[μλ° κ°μ²΄μ§ν₯ λμμΈ ν¨ν΄ (μ μΈμ, μ±ν₯μ)](http://www.yes24.com/Product/Goods/12501269)

<br/>

---