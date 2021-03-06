---
emoji: π¦
title: λ°μ»€λ μ΄ν° ν¨ν΄ (Decorator Pattern)
date: '2022-04-17 15:20:00'
author: μ°μ§
tags: λμμΈν¨ν΄
categories: DESIGN_PATTERN
---

<br/>

### π₯Β λλ‘ νμ λ°©λ² μ‘°ν©νκΈ°
λ΄λΉκ²μ΄μ SWμμ λλ‘ νμ κΈ°λ₯κ³Ό μΆκ° μ΅μμΈ μ°¨μ  νμ κΈ°λ₯ μ€κ³

μΆκ° μ΅μμΈ μ°¨μ νμ κΈ°λ₯μ RoadDisplayμ μμλ°μ draw() λ©μλλ₯Ό μ€λ²λΌμ΄λ

<br/>

`λλ‘ νμ ν΄λμ€` - RoadDisplay 

`μ°¨μ  νμ ν΄λμ€` - RoadDisplayWithLane extends RoadDisplay

![μ¬μ§](./decoratorPattern1.png)

```java
public class RoadDisplay{
	public void draw(){
		System.out.println("κΈ°λ³Έ λλ‘ νμ");
	}
}


public class RoadDisplayWithLane extends RoadDisplay{
	public void draw(){
		super.draw();
		drawLane();
	}

	private void drawLane(){
		System.out.println("μ°¨μ  λλ‘ νμ");
	}
}
```

<br/>

### π₯Β λ¬Έμ μ 
- λλ€λ₯Έ λλ‘νμ κΈ°λ₯μ μΆκ°λ‘ κ΅¬ννκ³ μΆλ€λ©΄?
- μ¬λ¬κ°μ§ μΆκ°κΈ°λ₯μ μ‘°ν©ν΄ μ¬μ©νκ³  μΆλ€λ©΄?

<br/>

### π₯Β λλ€λ₯Έ λλ‘νμ κΈ°λ₯ μΆκ° κ΅¬ν
![μ¬μ§](./decoratorPattern2.png)
```java
public class RoadDisplayWithTraffic extends RoadDisplay{
	public void draw(){
		super.draw();
		drawTraffic();
	}

	private void drawTraffic(){
		System.out.println("κ΅ν΅λ νμ");
	}
}
```

<br/>

### π₯Β μ¬λ¬ μΆκ° κΈ°λ₯ μ‘°ν©
λλ‘νμμ `μΆκ°μ μΈ κΈ°λ₯`μ RoadDisplay ν΄λμ€λ₯Ό μμλ°λ κ²μ μ μ ν μ€κ³μ΄λ,
μ¬λ¬ μ‘°ν©μ κ΅¬νν  λ μμμ ν΅ν κΈ°λ₯ νμ₯μ μ‘°ν© λ³λ‘ νμ ν΄λμ€λ₯Ό κ΅¬νν΄μΌ ν¨ 

β `λ¨μ ` : μ€λ³΅νμ¬ μμ±λλ λ©μλ μ‘΄μ¬, μΆκ°κΈ°λ₯μ λΆλΆμ§ν© κ°μλ§νΌ ν΄λμ€ μμ± νμ (2μ nμ κ³±)

 ![μ¬μ§](./decoratorPattern3.png)

```java
public class RoadDisplayWithLaneAndTraffic extends RoadDisplay{
	public void draw(){
		super.draw();
		drawLane();
		drawTraffic();
	}
	private void drawLane(){ //RoadDisplayWithLane λ΄ drawLane()λ©μλμ μ€λ³΅
		System.out.println("μ°¨μ  λλ‘ νμ");
	}
	private void drawTraffic(){ //RoadDisplayWithTraffic λ΄ drawTraffic()λ©μλμ μ€λ³΅
		System.out.println("κ΅ν΅λ νμ");
	}
}
```

<br/>

### π₯Β ν΄κ²°μ±
μΆκ° κΈ°λ₯λ³λ‘ κ°λ³μ μΈ ν΄λμ€λ₯Ό μ€κ³ β κΈ°λ₯μ μ‘°ν©ν  λ ν΄λμ€μ κ°μ²΄λ₯Ό μ΄μ©
μΆκ° κΈ°λ₯μ μκ° λ§μμλ‘ ν¨κ³Όκ° ν° μ€κ³ (2μ nμ κ³± β n+2)

 ![μ¬μ§](./decoratorPattern4.png)

κΈ°λ³Έμ μΌλ‘ μ κ³΅νλ λλ‘ νμ κΈ°λ₯μ RoadDisplay ν΄λμ€μ draw λ©μλλ₯Ό νΈμΆ
λ°λΌμ μ°¨μ  νκΈ°μ, κ΅ν΅λ νκΈ° κΈ°λ₯μ RoadDisplay κ°μ²΄ μ°Έμ‘°κ° νμ (draw λ©μλ μ€λ²λΌμ΄λ)
β μμ ν΄λμ€μΈ DisplayDecorator ν΄λμ€μμ Display ν΄λμ€λ‘μ μ»΄ν¬μ§μ κ΄κ³λ₯Ό ν΅ν΄ νν

```java
public abstract class Display{
	public abstract void draw();
}

public abstract class DisplayDecorator extends Display{
	private Display decoratedDisplay;

	public DisplayDecorator(Display decoratedDisplay){
		this.decoratedDisplay = decoratedDisplay;
	}
	public void draw(){
		decoratedDisplay.draw();
	}
}
```

<br/>

**μ°¨μ νκΈ°μ κ΅ν΅λ νκΈ° κΈ°λ₯ μ‘°ν© κ΅¬ν**
```java
public class Client {
	public static void main(String[] args){

		Display roadWithLandAndTraffic = 
			new TrafficDecorator(new LaneDecorator(new RoadDisplay()));

		roadWithLandAndTraffic.draw();

	}
}
```

<br/>

### π₯Β λ°μ»€λ μ΄ν° ν¨ν΄
λ°μ»€λ μ΄ν° ν¨ν΄μ `κΈ°λ³Έ κΈ°λ₯μ μΆκ°ν  μ μλ κΈ°λ₯ μ’λ₯κ° λ§μ κ²½μ°` κ° μΆκ° κΈ°λ₯μ Decorator ν΄λμ€λ‘ μ μ ν ν νμν Decorator κ°μ²΄λ₯Ό μ‘°ν©ν¨μΌλ‘μ¨ μΆκ° κΈ°λ₯μ μ‘°ν©μ μ€κ³νλ λ°©μ
Decorator κ°μ²΄ μ‘°ν©μ νλ‘κ·Έλλ° μ€ν μ€ λμ μΌλ‘ μμ±

```java
public class Client {
	public static void main(String[] args){

		Display road = new RoadDisplay();
		
		for(String decoratorName: args){
			if(decoratorName.equalsIgnoreCase("Lane"))
				road = new LaneDecorator(road);
			if(decoratorName.equalsIgnoreCase("Traffic"))
				road = new TrafficDecorator(road);
			if(decoratorName.equalsIgnoreCase("Crossing"))
				road = new CrossingDecorator(road);
		}
		road.draw();
	}
}
```

<br/>

### π₯Β **λ°μ»€λ μ΄ν° ν¨ν΄μ ν΄λμ€ λ€μ΄μ΄κ·Έλ¨**

 ![μ¬μ§](./decoratorPattern5.png)

- `Component` : κΈ°λ³Έ κΈ°λ₯μ ConcreteComponentμ μΆκ° κΈ°λ₯μ Decoratorμ κ³΅ν΅ κΈ°λ₯μ μ μ
                       μ¦, ν΄λΌμ΄μΈνΈλ Componentλ₯Ό ν΅ν΄ μ€μ  κ°μ²΄λ₯Ό μ¬μ©
- `ConcreteComponent` : κΈ°λ³Έ κΈ°λ₯μ κ΅¬ννλ ν΄λμ€
- `Decorator` : λ§μ μκ° μ‘΄μ¬νλ κ΅¬μ²΄μ μΈ Decoratorμ κ³΅ν΅ κΈ°λ₯μ μ κ³΅
- `ConcreteDecoratorA`, `ConcreteDecoratorB` : Decoratorμ νμ ν΄λμ€, κΈ°λ³Έ κΈ°λ₯μ μΆκ°λλ κ°λ³ κΈ°λ₯


<br/>

### π₯Β **λ°μ»€λ μ΄ν° ν¨ν΄μ μμ°¨ λ€μ΄μ΄κ·Έλ¨**

 ![μ¬μ§](./decoratorPattern6.png)



<br/>

---
## References

[μλ° κ°μ²΄μ§ν₯ λμμΈ ν¨ν΄ (μ μΈμ, μ±ν₯μ)](http://www.yes24.com/Product/Goods/12501269)

<br/>

---