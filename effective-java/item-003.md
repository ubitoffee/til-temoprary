# private 생성자나 열거타입으로 싱글턴임을 보증하라

싱글턴은 인스턴스를 오직 하나만 생성할 수 있는 클래스이다.
단 하나의 인스턴스만 생성되기 때문에 테스트에 불편함이 있다.

## 싱글턴을 만드는 방법

### 1. 생성자를 private으로 만들고 인스턴스에 접근할 수 있는 public static 멤버를 열어 놓는 방법
```java
  public class Ubi {
    public static final Ubi INSTANCE = new Ubi(); // 열어놓은 싱글턴 멤버
    private Ubi() { ... }
  }
```

### 2. static 팩토리 메소드를 이용하여 인스턴스에 접근하게 하는 방법
```java
  public class Ubi {
    private static final Ubi INSTANCE = new Ubi();
    public static Ubi getInstance() { return INSTANCE; }
  }
```

### 3. enum을 이용한 방법
```java
  public enum Ubi {
    INSTANCE;
    
    public void receive() { ... }
  }
```
