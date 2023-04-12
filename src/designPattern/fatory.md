# CS 면접대비

책을 보면서 배우는 CS내용에 대해서 정리해보자

> 💡 팩토리 패턴 ? <br>
> 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자,<br>
> 상속 관계에 있는 두 클래스의 상위 클래스가 중요한 뼈대를 결정하고,<br>
> 하위 클래스에 객체 생성에 관한 구체적인 내용을 결정하는 패턴이다
> <br> > <br>
> 상위 클래스는 하위 클래와 분리되기 때문에 느슨한 결합을 가지며<br>
> 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요 없기 때문에 더 많은 유연성을 갖음<br>
> 추가로 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩터링해도 한 곳만 고 필 수 있게 되어 유지 보수성이 증가됨

## 1. 자바스크립트 팩토리 패턴 예제

```javascript
class CoffeeFactory {
  static createCoffee(type) {
    const factory = factoryList[type];
    return factory.createCoffee();
  }
}
class Latte {
  constructor() {
    this.name = "latte";
  }
}
class Espresso {
  constructor() {
    this.name = "Espresso";
  }
}

class LatteFactory extends CoffeeFactory {
  static createCoffee() {
    return new Latte();
  }
}
class EspressoFactory extends CoffeeFactory {
  static createCoffee() {
    return new Espresso();
  }
}
const factoryList = { LatteFactory, EspressoFactory };

const main = () => {
  // 라떼 커피를 주문한다.
  const coffee = CoffeeFactory.createCoffee("LatteFactory");
  // 커피 이름을 부른다.
  console.log(coffee.name); // latte
};
main();
```

1. `CoffetFactory` 상위 클래스가 중요 뼈대를 결정함

2. `LatteFactory`, `EspressoFactory`가 구체적인 내용을 결정

> 참고로 해당 코드는 의존성 주입(DI)라고 볼 수 있다 CoffeFactory에서 LatteFactory<br>
> 인스턴스를 생성하는 것이 아닌 LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문이다

또한, CoffeeFactory를 보면 static으로 정적인 메서드를 정의한 것을 알 수 있다<br>
정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능하며<br>
메모리 절약 할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있는 장점이 있다
