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
