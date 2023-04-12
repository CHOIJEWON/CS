# CS 면접대비

책을 보면서 배우는 CS내용에 대해서 정리해보자

> 💡 디자인 패턴 ? <br>
> 디자인 패턴이이란 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을<br>
> 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것을 의미함

## 1. SingleTon pattern

---

> 💡 싱글톤 패턴(singleton pattern)은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴<br>
> 클래스당 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는데 사용, 보통 데이터베이스 연결 모듈에 많이 사용함

### 1-1) 자바스크립트의 싱글톤 패턴

```javascript
const obj = {
  a: 27,
};

const obj2 = {
  a: 27,
};

console.log(obj === obj2); // false
```

obj와 obj2는 다른 인스턴스를 갖듯. <br>
이 또한 new Onject라는 클래스에서 나온 단 하나의 인스턴스이다 <br>
따라 어느정도 싱글톤 패턴이라 볼 수 있지만, 실제 싱글톤 패턴은 보통 다음과 같은 코드로 구성됨

```javascript
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    return Singleton.instance;
  }
  getInstance() {
    return this;
  }
}
const a = new Singleton();
const b = new Singleton();
console.log(a === b); // true
```

### 1-2) 데이터베이스 연결 모듈

앞서 설명했듯이 싱글톤 패턴은 `데이터베이스 연결` 모듈에 많이 사용된다

```javascript
// DB 연결을 하는 것이기 때문에 비용이 더 높은 작업
const URL = "mongodb://localhost:27017/kundolapp";
const createConnection = (url) => ({ url: url });
class DB {
  constructor(url) {
    if (!DB.instance) {
      DB.instance = createConnection(url);
    }
    return DB.instance;
  }
  connect() {
    return this.instance;
  }
}
const a = new DB(URL);
const b = new DB(URL);
console.log(a === b); // true
```

### 1-2-1) MySQL connection 예제를 보자

```javascript
// 메인 모듈
const mysql = require("mysql");
const pool = mysql.createPool({
  connectionLimit: 10,
  host: "example.org",
  user: "kundol",
  password: "secret",
  database: "승철이디비",
});
pool.connect();
// 모듈 A
pool.query(query, function (error, results, fields) {
  if (error) throw error;
  console.log("The solution is: ", results[0].solution);
});
// 모듈 B
pool.query(query, function (error, results, fields) {
  if (error) throw error;
  console.log("The solution is: ", results[0].solution);
});
```

1. 예제 코드처럼 메인 모듈에선 데이터베이스 연결에 관한 인스턴스를 정의함
2. 다른 모듈인 A or B에서 해당 인스턴스를 기반으로 쿼리를 보내는 형식

### 1-3) 싱글톤 패턴의 장/단점

- 장점
  - 인스턴스를 생성할 때 비용을 낮출수 있음
- 단점
  - 의존성이 높아진다

### 1-3-1) 싱글톤 패턴의 단점

- 싱글톤 패턴은 `TDD(Test Driven Development)`를 할 때 걸림돌이 된다<br>

TDD를 할 때 단위 테스트를 주로 진행 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하기 때문에<br>
테스트를 어떤 순서로든 실행할 수 있어야 한다

### 1-3-2) 의존성 주입

- 싱글톤 패턴은 사용하기 쉽고 굉장히 실용적이지만 모듈 간의 결합을 강하게 만든다

이때 NestJS 개발자들은 익히 알고 있는 `의존성` 주입(DI, Dependency Injection)을 통해<br>
모듈간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다<br>

\*의존성 : 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A도 또한 변해야 된다는 것을 의미함

![의존성주입](./images/%EC%9D%98%EC%A1%B4%EC%84%B1.png)

위 그림처럼 메인 모듈이 '직접' 다른 하위 모둘에 대한 의존성을 주기보다는<br>
중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 '간접적'으로 의존성을 주입하는 방식<br>
이를 통해 메인 모듈은 하위 모둘에 대한 의존성이 떨어지게 됨 이 과정을 `디커플링이 된다`라고 함

- 디커플링 : 커플링이란 의존성, 한 쌍을 의미한다. 객체들 간 의존성이 강할 경우, <br>
  한 프로그램의 구성요소를 수정하면 그와 연관된 모든 구성요소를 새로 수정해야 한다. <br>
  여기서 디커플링이란 인터페이스를 이용해 모듈 사이의 의존성을 감소하고 유지보수 효과를 높이는 방법을 말한다.

### 1-3-3) 의존성 주입의 장/단점

- 장점
  - 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅 하기 쉬움
  - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주어 의존성 방향이 일관됨
  - 애플리케이션을 쉽게 추론 가능하고, 모듈간의 관계들이 조금 더 명확해짐
- 단점
  - 모듈들이 더욱 분리되어 클래수의 수가 늘어나 복잡성이 증가함
  - 약간의 런타임 패널티가 발생하는 경우도 있음

```
* 의존성 주입의 원칙 *

의존성 주입은 "상위 모듈은 하위 모듈에 어떠한 것도 가져오면 않아야 한다"
또한 둘 다 추상화에 의존하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다
```
