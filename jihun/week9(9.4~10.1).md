## 9.4 private, protected 프로퍼티와 메서드

객체 지향 프로그래밍에서 가장 중요한 원리 중 하나는 '내부 인터페이스와 외부 인터페이스를 구분 짓는 것’인데, 단순히 'hello word’를 출력하는 것이 아닌 복잡한 애플리케이션을 구현하려면, 내부 인터페이스와 외부 인터페이스를 구분하는 방법을 ‘반드시’ 알고 있어야 한다.

잠시 개발을 벗어나 현실 세계로 눈을 돌려, 내부 인터페이스와 외부 인터페이스 구분이 무엇을 의미하는지 알아보자.

일상생활에서 접하게 되는 기계들은 꽤 복잡한 구조로 되어 있지만 내부인터페이스와 외부 인터페이스가 구분되어있기 때문에 문제없이 기계를 사용할 수 있다.



#### 실생활 예제

커피 머신을 예로 들어보자. 외형은 심플하다. 버튼 하나, 화면 하나, 구멍 몇 개 등이 있다.

![img](https://ko.javascript.info/article/private-protected-properties-methods/coffee.jpg) 

하지만 내부는 이렇게 생겼다고 한다(수리용 매뉴얼에서 가져온 사진).

![img](https://ko.javascript.info/article/private-protected-properties-methods/coffee-inside.jpg) 

뭔가 디테일한 것들이 아주 많지만 이 모든 것을 알지 못해도 커피 머신을 사용하는 데 지장이 없다.

커피 머신은 꽤 믿음직한 기계이다. 초기불량이 아닌 이상 보통 수년 간 사용할 수 있고, 중간에 고장이 나도 수리를 받으면 된다. 외형은 단순하지만 커피 머신을 신뢰할 수 있는 이유는 모든 세부 요소들이 기계 내부에 잘 정리되어 *숨겨져* 있기 때문이다.

커피 머신에서 보호 커버를 제거하면 사용법이 훨씬 복잡해지고 위험한 상황이 생길 수 있다. 어디를 눌러야 할지 모르고 감전이 될 수도 있기 때문이다. 앞으로 학습하겠지만, 프로그래밍에서 객체는 커피 머신과 같다.

프로그래밍에서는 보호 커버를 사용하는 대신 특별한 문법과 컨벤션을 사용해 안쪽 세부 사항을 숨긴다는 점이 다르다.



#### 내부 인터페이스와 외부 인터페이스

객체 지향 프로그래밍에서 프로퍼티와 메서드는 두 그룹으로 분류된다.

- *내부 인터페이스(internal interface)* – 동일한 클래스 내의 다른 메서드에선 접근할 수 있지만, 클래스 밖에선 접근할 수 없는 프로퍼티와 메서드
- *외부 인터페이스(external interface)* – 클래스 밖에서도 접근 가능한 프로퍼티와 메서드

커피 머신으로 비유하자면 기계 안쪽에 숨어있는 뜨거운 물이 지나가는 관이나 발열 장치 등이 내부 인터페이스가 될 수 있다.

내부 인터페이스의 세부사항들은 서로의 정보를 이용하여 객체를 동작시킨다. 발열 장치에 부착된 관을 통해 뜨거운 물이 이동하는 것처럼 말이다.그런데 커피 머신은 보호 커버에 둘러싸여 있기 때문에 보호 커버를 벗기지 않고는 커피머신 외부에서 내부로 접근할 수 없다. 밖에선 세부 요소를 알 수 없고, 접근도 불가능합니다. 내부 인터페이스의 기능은 외부 인터페이스를 통해야만 사용할 수 있다. 

이런 특징 때문에 외부 인터페이스만 알아도 객체를 가지고 무언가를 할 수 있다. 객체 안이 어떻게 동작하는지 알지 못해도 괜찮다는 점은 큰 장점으로 작용한다.

지금까진 내부 인터페이스와 외부 인터페이스의 개관에 대해 설명했다.



자바스크립트에는 아래와 같은 두 가지 타입의 객체 필드(프로퍼티와 메서드)가 있다.

- public: 어디서든지 접근할 수 있으며 외부 인터페이스를 구성한다. 지금까지 다룬 프로퍼티와 메서드는 모두 public이다.
- private: 클래스 내부에서만 접근할 수 있으며 내부 인터페이스를 구성할 때 쓰인다.

자바스크립트 이외의 다수 언어에서 클래스 자신과 자손 클래스에서만 접근을 허용하는 ‘protected’ 필드를 지원한다. protected 필드는 private과 비슷하지만, 자손 클래스에서도 접근이 가능하다는 점이 다르다. protected 필드도 내부 인터페이스를 만들 때 유용하다. 자손 클래스의 필드에 접근해야 하는 경우가 많기 때문에, protected 필드는 private 필드보다 조금 더 광범위하게 사용된다.

자바스크립트는 protected 필드를 지원하지 않지만, protected를 사용하면 편리한 점이 많기 때문에 이를 모방해서 사용하는 경우가 많다.

이제 지금까지 배운 프로퍼티 타입을 사용해 커피머신을 만들어면, 실제 커피 머신은 아주 복잡하지만, 여기선 간결성을 위해 간이 모델을 만들어 보도록 하자(원한다면 구현은 가능하다).



#### 프로퍼티 보호하기

먼저, 간단한 커피 머신 클래스를 만들어보자.

```javascript
class CoffeeMachine {
  waterAmount = 0; // 물통에 차 있는 물의 양

  constructor(power) {
    this.power = power;
    alert( `전력량이 ${power}인 커피머신을 만듭니다.` );
  }

}

// 커피 머신 생성
let coffeeMachine = new CoffeeMachine(100);

// 물 추가
coffeeMachine.waterAmount = 200;
```

현재 프로퍼티 `waterAmount`와 `power`는 public이다. 손쉽게 `waterAmount`와 `power`를 읽고 원하는 값으로 변경하기 쉬운 상태이다.

이제 `waterAmount`를 protected로 바꿔서 `waterAmount`를 통제해 보도록 하자. 예시로 `waterAmount`를 0 미만의 값으로는 설정하지 못하도록 만들어 보자구.

**protected 프로퍼티 명 앞엔 밑줄 `_`이 붙는다.**

자바스크립트에서 강제한 사항은 아니지만, 밑줄은 프로그래머들 사이에서 외부 접근이 불가능한 프로퍼티나 메서드를 나타낼 때 쓴다.

`waterAmount`에 밑줄을 붙여 protected 프로퍼티로 만들어주자.

```javascript
class CoffeeMachine {
  _waterAmount = 0;

  set waterAmount(value) {
    if (value < 0) throw new Error("물의 양은 음수가 될 수 없습니다.");
    this._waterAmount = value;
  }

  get waterAmount() {
    return this._waterAmount;
  }

  constructor(power) {
    this._power = power;
  }

}

// 커피 머신 생성
let coffeeMachine = new CoffeeMachine(100);

// 물 추가
coffeeMachine.waterAmount = -10; // Error: 물의 양은 음수가 될 수 없다.
```

이제 물의 양을 0 미만으로 설정하면 실패한다.
