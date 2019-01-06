# 생성자에 매개변수가 많다면 빌더를 고려하자

[아이템 1]('../item-001.md')에서 학습한 static 팩토리 메소드나 생성자나 매개변수가 많아질 경우 프로그래머들이 사용하기 어렵고 혼동이 쉬워진다.

이를위한 해결책으로는

## 1. 점층적 생성자 패턴 (Telescoping Constructor Pattern)

```java
public class Character {
  private final int hp;
  private final int mp;
  private final int str;
  private final int dex;
  pirvate final int int;
  private final int lck;
  
  public Character(int hp, int mp) {
    this(hp, mp, 0);
  }
  
  public Character(int hp, int mp, int str) {
    this(hp, mp, str, 0);
  }
  
  public Character(int hp, int mp, int str, int dex) {
    this(hp, mp, str, dex, 0);
  }
  
  ...
}
```

위와 같이 매개변수를 선택적으로 받는 경우, 점진적으로 매개변수를 늘려가는 방식이다.

허나, 이는 같은 형의 매개변수를 넘길 경우 혼동이 발생할 수 있고

```java
Character warrior = new Warrior(200, 100, 20, 10, 0, 5);
```

위와 같은 경우는 필요하지않은 int 스탯에도 기본값 0을 넘겨야하는 불편함이 있다.

## 2. 자바빈즈 (Java Beans) 패턴

자바 빈즈 패턴은 매개변수를 받지 않는 빈 생성자를 생성 후 setter를 통해 필드값을 생성하는 방식이다.

```java
Character warrior = new Character();
character.setHp(250);
character.setMp(200);
character.setStr(20);
```

위의 패턴의 문제점을 어느정도 보완했지만, **객체 하나를 만들기 위해 필드당 하나꼴로 메소드가 호출**되야 하는 문제점으로 인해 객체가 완전히 생성될 때 까지는 일관성이 무너진다.

이러한 문제로 인해 이 패턴에서는 불변 클래스([[아이템 17]('item-017.md'))로 만들 수 없고 스레드 안전성을 위한 추가 작업(lock)이 필요하다.

<!-- After: Freeze -->

## 3. Builder 패턴
