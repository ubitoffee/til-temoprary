# 생성자 대신 static 팩토리 메소드

자바에서는 생성자를 이용하여 클래스의 인스턴스를 얻는 방법을 주로 사용한다.

```java
Boolean b = new Boolean(true); // 생성자의 인자로 boolean을 받아 인스턴스를 생성
```

하지만 생성자 대신 stastic 팩토리 메소드를 고민해볼 수도 있다.

```java
public static Boolean valueOf(boolean b) {
  return b ? Boolean.TRUE :Boolean.FALSE; // 똑같이 Boolean 클래스의 인스턴스를 리턴한다
}
```

위 둘은 동일하게 Boolean 클래스의 인스턴스를 리턴하지만, 각각의 장단점이있다.

## static 팩토리 메소드를 사용할 때의 장점

### 1. 이름을 가질 수 있다.

생성자가 어떤 매개변수에 따라 어떤 인스턴스를 리턴하는지 정확히 알지 못한다면, 이름이 명확한 static 팩토리 메소드가 협업에 훨씬 수월하다.

```java
BigInteger prime = new BigInteger(int, int, Random); // 값이 소수를 리턴하는 생성자이다

BigInteger.probablePrime(int, Random); // 소수를 리턴하는 static 팩토리 메소드
```

> 이 부분은 문서화로 어느정도 대체가 가능하나 위의 예를 보면 static 팩토리 메소드가 조금 더 알아보기 쉽다.

또, 생성자는 같은 매개변수를 받아 다른 인스턴스를 리턴할 수 없다.(이걸 시그니처라고 하는구나)

> 클래스의 생성자는 메서드와는 다르게 이름을 다르게 지정할 수 없어 매개변수의 타입을 각각 다르게 해야하는 시그니처의 제약이 있다.

### 2. 항상 새로운 인스턴스를 만들 필요는 없다.

`Boolean.valueOf(boolean)`와 같은 메서드는 인스턴스를 새로 생성하지 않고 미리 만들어둔 인스턴스 또는 캐시된 인스턴스들을 재활용할 수 있다.

Boolean의 경우는 작으니까 체감이 잘 안가지만 같은 객체를 자주 요청하는 (특히 생성하는데 비용이 큰) 상황에는 성능을 업그레이드 시킬 수 있다.

> Flyweight pattern 참조

### 3. 리턴 타입의 하위 타입 인스턴스를 반환할 수 있다.

생성자는 해당 클래스의 타입만을 리턴할 수 있지만, static 팩토리 메소드를 이용하면 아래와 같은 방식으로 하위 타입의 인스턴스도 반환이 가능하다.

```java
class Dog extends Animal {}
class Cat extends Animal {}

// 다양한 하위타입의 리턴이 가능
public static Animal newPet(String type) {
  switch(type) {
    case 'dog':
      return new Dog();
    case 'cat':
      return new Cat();
  }
}

Animal animal = new Animal('dog'); // 생성자로는 Animal 타입밖에..
```

### 4. 매개변수에 따라 각기 다른 클래스의 객체를 반환할 수 있다.

1번, 3번 내용과 비슷한 내용으로 static 팩토리 메소드를 사용하면 메소드의 이름을 다양하게 변경하며 시그니처를 찍어낼 수 있고,
그 매개변수들에 따라 각기 다른 객체타입의 인스턴스를 리턴할 수 있다.

### 5. static 팩토리 메소드를 작성할 때는 리턴하는 객체의 클래스가 반드시 존재하지 않아도 된다.

위의 특징은 `Service Provider 프레임워크`의 기반이 된다. // TODO 프레임워크 공부할 것

`Service Prodier`의 구성요소
- 서비스의 구현체를 대표하는 `Service Interface`
- `Provider`가 구현체를 등록할 때 사용하는 `Provider Registration API`
- API 사용자(클라이언트)가 해당 서비스의 인스턴스를 얻을 때 사용하는 `Service Access API`

`JDBC` 에서 바라본 각 예
- Service Interface : `Connection`
- Provider Registration API : `DriverManager.registerDriver`
- Service Access API : `DriverManager.getConnection`
- Service Provider Interface : `Driver`

----

## Static 팩토리 메소드의 단점
