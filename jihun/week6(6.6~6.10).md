## 6.6 객체로서의 함수와 기명 함수 표현식

자바스크립트에서 함수는 값으로 취급된다. 모든 값은 자료형을 가지고 있는데, 그렇다면 함수의 자료형은 무엇일까?

<br />

바로, 함수는 객체다!!

<br />

함수는 호출이 가능한(callable) '행동 객체’라고 이해하면 쉽고, 함수를 호출 할 수 있을 뿐만 아니라 객체처럼 함수에 프로퍼티를 추가·제거하거나 참조를 통해 전달할 수도 있다.



#### ‘name’ 프로퍼티

함수 객체엔 몇 가지 쓸만한 프로퍼티가 있는데, ‘name’ 프로퍼티를 사용하면 함수 이름을 가져올 수 있다.

```javascript
function sayHi() {
  alert("Hi");
}

alert(sayHi.name); // sayHi
```

<br />

함수 객체에 이름을 할당해주는 로직은 익명 함수라도 자동으로 이름을 할당한다.

```javascript
let sayHi = function() {
  alert("Hi");
};

alert(sayHi.name); // sayHi (익명 함수이지만 이름이 있다!)
```

<br />

기본값을 사용해 이름을 할당한 경우에도 마찬가지다.

```javascript
function f(sayHi = function() {}) {
  alert(sayHi.name); // sayHi (이름이 있다!)
}

f();
```

<br />

자바스크립트 명세서에서 정의된 이 기능을 **'contextual name’**이라고 부르는데, 이름이 없는 함수의 이름을 지정할 땐 컨텍스트에서 이름을 가져온다.

또한, 객체 메서드의 이름도 ‘name’ 프로퍼티를 이용해 가져올 수 있다.

```javascript
let user = {

  sayHi() {
    // ...
  },

  sayBye: function() {
    // ...
  }

}

alert(user.sayHi.name); // sayHi
alert(user.sayBye.name); // sayBye
```

<br />

그런데 객체 메서드 이름은 함수처럼 자동 할당이 되지 않는다. 적절한 이름을 추론하는 게 불가능한 상황이 있는데, 이때 name 프로퍼티엔 빈 문자열이 저장된다.

```javascript
// 배열 안에서 함수를 생성함
let arr = [function() {}];

alert( arr[0].name ); // <빈 문자열>
// 엔진이 이름을 설정할 수 없어서 name 프로퍼티의 값이 빈 문자열이 됨
```

실무에서서는 대부분의 함수는 이름이 있으므로 위와 같은 상황은 잘 발생하지 않는다고 한다.



#### ‘length’ 프로퍼티

내장 프로퍼티 `length`는 함수 매개변수의 개수를 반환한다. 

```javascript
function f1(a) {}
function f2(a, b) {}
function many(a, b, ...more) {}

alert(f1.length); // 1
alert(f2.length); // 2
alert(many.length); // 2
```

위 예시를 통해 나머지 매개변수는 개수에 포함되지 않는다.

한편, `length` 프로퍼티는 다른 함수 안에서 동작하는 함수의 [타입을 검사(type introspection)](https://en.wikipedia.org/wiki/Type_introspection) 할 때도 종종 사용되는데, 

질문에 쓰일 `question`과 질문에 대한 답에 따라 호출할 임의의 수의 `handler` 함수를 함께 받는 함수 `ask`를 예시로 이에 대해 알아보자.

사용자가 답을 제출하면 `ask`는 핸들러 함수를 호출한다. 이때 우리는 두 종류의 핸들러 함수를 `ask`에 전달할 수 있다.

- 인수가 없는 함수로, 사용자가 OK를 클릭했을 때만 호출됨
- 인수가 있는 함수로, 사용자가 OK를 클릭하든 Cancel을 클릭하든 호출됨

그리고 `handler.length` 프로퍼티를 사용하면 상황에 맞는 `handler`를 호출할 수 있다.

사용자가 긍정적인 대답을 했을 때 사용 할 인수가 없는 핸들러를 하나 만들고, 사용자의 응답 종류와 관계없이 범용적으로 사용할만한 핸들러도 구축해 `ask` 내부에서 `handler.length`와 함께 사용하면 된다.

```javascript
function ask(question, ...handlers) {
  let isYes = confirm(question);

  for(let handler of handlers) {
    if (handler.length == 0) {
      if (isYes) handler();
    } else {
      handler(isYes);
    }
  }

}

// 사용자가 OK를 클릭한 경우, 핸들러 두 개를 모두 호출함
// 사용자가 Cancel을 클릭한 경우, 두 번째 핸들러만 호출함
ask("질문 있으신가요?", () => alert('OK를 선택하셨습니다.'), result => alert(result));
```

인수의 종류에 따라(위 예시에선 인수의 `length` 프로퍼티 값에 따라) 인수를 다르게 처리하는 방식을 프로그래밍 언어에선 [다형성(polymorphism)](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) 이라고 부른다. 자바스크립트 라이브러리를 뜯어보다 보면 다형성이 곳곳에서 사용되고 있다는 것을 확인할 수 있다.

<br />

#### 커스텀 프로퍼티

함수에 자체적으로 만든 프로퍼티를 추가할 수도 있다.

이런 특징을 이용해 함수 호출 횟수를 `counter` 프로퍼티에 저장해보자.

```javascript
function sayHi() {
  alert("Hi");

  // 함수를 몇 번 호출했는지 세보자.
  sayHi.counter++;
}
sayHi.counter = 0; // 초깃값

sayHi(); // Hi
sayHi(); // Hi

alert( `호출 횟수: ${sayHi.counter}회` ); // 호출 횟수: 2회
```



**프로퍼티는 변수가 아니다.**

`sayHi.counter = 0`와 같이 함수에 프로퍼티를 할당해도 함수 내에 지역변수 `counter`가 만들어지지 않는다. `counter` 프로퍼티와 변수 `let counter`는 전혀 관계가 없다.

프로퍼티를 저장하는 것처럼 함수를 객체처럼 다룰 수 있지만, 이는 실행에 아무 영향을 끼치지 않는다. 변수는 함수 프로퍼티가 아니고 함수 프로퍼티는 변수가 아니기 때문이다. 둘 사이에는 공통점이 없다.

클로저는 함수 프로퍼티로 대체할 수 있다. [변수의 유효범위와 클로저](https://ko.javascript.info/closure) 챕터에서 살펴본 바 있는 counter 함수를 함수 프로퍼티를 사용해 바꿔보도록 하자.

```javascript
function makeCounter() {

  // let count = 0 대신 아래 메서드(프로퍼티)를 사용함

  function counter() {
    return counter.count++;
  };

  counter.count = 0;

  return counter;
}

let counter = makeCounter();
alert( counter() ); // 0
alert( counter() ); // 1
```

이제 `count`는 외부 렉시컬 환경이 아닌 함수 프로퍼티에 바로 저장된다.

그런데 과연 이렇게 함수 프로퍼티에 정보를 저장하는 게 클로저를 사용하는 것보다 나은 방법일까?

두 방법의 차이점은 `count` 값이 외부 변수에 저장되어있는 경우 드러나는데, 클로저를 사용한 경우엔 외부 코드에서 `count`에 접근할 수 없다. 오직 중첩함수 내에서만 `count` 값을 수정할 수 있다. 반면 함수 프로퍼티를 사용해 `count`를 함수에 바인딩시킨 경우엔 다음 예시와 같이 외부에서 값을 수정할 수 있다.

```javascript
function makeCounter() {

  function counter() {
    return counter.count++;
  };

  counter.count = 0;

  return counter;
}

let counter = makeCounter();

counter.count = 10;
alert( counter() ); // 10
```

따라서 구현 방법은 목적에 따라 선택하면 될 것 같다.

<br />

#### 기명 함수 표현식

기명 함수 표현식(Named Function Expression, NFE)은 이름이 있는 함수 표현식을 나타내는 용어dl다.

먼저, 일반 함수 표현식을 살펴보자.

```javascript
let sayHi = function(who) {
  alert(`Hello, ${who}`);
};
```

<br />

여기에 이름을 붙여보면,

```javascript
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};
```

이렇게 이름을 붙인다고 해서 뭐가 달라지는 걸까? 그리고,  `"func"`이라는 이름은 어떤 경우에 붙이는 걸까?

먼저 이렇게 이름을 붙여도 위 함수는 여전히 함수 표현식이라는 점에 주목해야 한다. `function` 뒤에 `"func"`이라는 이름을 붙이더라도 여전히 표현식을 할당한 형태를 유지하기 때문에 함수 선언문으로 바뀌지 않는다.

이름을 추가한다고 해서 기존에 동작하던 기능이 동작하지 않는 일은 발생하지 않는다.

`sayHi()`로 호출하는 것도 여전히 가능하다.

```javascript
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};

sayHi("John"); // Hello, John
```

대신 `func`과 같은 이름을 붙이면 두 가지가 변화가 생기는데, 이 두 변화 때문에 기명 함수 표현식을 사용하는 것이다.

1. 이름을 사용해 함수 표현식 내부에서 자기 자신을 참조할 수 있다.
2. 기명 함수 표현식 외부에선 그 이름을 사용할 수 없다.

함수 `sayHi`를 예시로 이에 대해 살펴보자. 함수 `sayHi`는 `who`에 값이 없는 경우, 인수 `"Guest"`를 받고 자기 자신을 호출한다.

```javascript
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // func를 사용해서 자신을 호출한다.
  }
};

sayHi(); // Hello, Guest

// 하지만 아래와 같이 func를 호출하는 건 불가능하다.
func(); // Error, func is not defined (기명 함수 표현식 밖에서는 그 이름에 접근할 수 없다.)
```

그런데 여기서 왜 중첩 호출을 할 때 `sayHi`대신 `func`을 사용했을까?

사실 대부분의 개발자는 아래와 같이 코드를 작성하곤 한다.

```javascript
let sayHi = function(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    sayHi("Guest");
  }
};
```

하지만 이렇게 코드를 작성하면 외부 코드에 의해 `sayHi`가 변경될 수 있다는 문제가 생긴다. 함수 표현식을 새로운 변수에 할당하고, 기존 변수에 `null`을 할당하면 에러가 발생한다.

```javascript
let sayHi = function(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    sayHi("Guest"); // TypeError: sayHi is not a function
  }
};

let welcome = sayHi;
sayHi = null;

welcome(); // 중첩 sayHi 호출은 더 이상 불가능하다!
```

에러는 함수가 `sayHi`를 자신의 외부 렉시컬 환경에서 가지고 오기 때문에 발생한다. 지역(local) 렉시컬 환경엔 `sayHi`가 없기 때문에 외부 렉시컬 환경에서 `sayHi`를 찾는데, 함수 호출 시점에 외부 렉시컬 환경의 `sayHi`엔 `null`이 저장되어있기 때문에 에러가 발생한다.

함수 표현식에 이름을 붙여주면 바로 이런 문제를 해결할 수 있다.

코드를 수정해 보자.

```javascript
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // 원하는 값이 제대로 출력된다.
  }
};

let welcome = sayHi;
sayHi = null;

welcome(); // Hello, Guest (중첩 호출이 제대로 동작함)
```

`"func"`이라는 이름은 함수 지역 수준(function-local)에 존재하므로 외부 렉시컬 환경에서 찾지 않아도 된다. 함수 표현식에 붙인 이름은 현재 함수만 참조하도록 명세서에 정의되어있기 때문에 외부 렉시컬 환경에선 보이지도 않는다.

이렇게 기명 함수 표현식을 이용하면 `sayHi`나 `welcome` 같은 외부 변수의 변경과 관계없이 `func`이라는 '내부 함수 이름’을 사용해 언제든 함수 표현식 내부에서 자기 자신을 호출할 수 있다.

**함수 선언문엔 내부 이름을 지정할 수 없다.**

지금까지 살펴본 '내부 이름’은 함수 표현식에만 사용할 수 있고, 함수 선언문엔 사용할 수 없습니다. 함수 선언문엔 ‘내부’ 이름을 지정할 수 있는 문법이 없다.

개발을 하다 보면 믿을만한 내부 이름이 필요할 때가 생기곤 하는데, 이 때 바로 함수 선언문을 기명 함수 표현식으로 다시 정의하면 된다.



> #### 요약
>
> 함수는 객체이다.
>
> 객체로서의 함수에서 사용 할 수 있는 프로퍼티 두 가지
>
> - `name` – 함수의 이름이 저장된다. 대개는 함수 선언부에서 이름을 가져오는데, 선언부에 이름이 없는 경우엔 자바스크립트 엔진이 컨텍스트(할당 등)를 이용해 이름을 추론한다.
> - `length` – 함수 선언부에 있는 인수의 수로 나머지 매개변수는 포함되지 않는다.
>
> 함수 표현식으로 함수를 정의하였는데 이름이 있다면 이를 기명 함수 표현식이라 부른다. 기명 함수 표현식의 이름은 재귀 호출과 같이 함수 내부에서 자기 자신을 호출하고자 할 때 사용할 수 있다.
>
> 함수엔 다양한 프로퍼티를 추가할 수 있다. 널리 쓰이는 자바스크립트 라이브러리 상당수에서 이런 커스텀 프로퍼티를 잘 활용하고 있다.
>
> 이런 라이브러리들은 ‘주요’ 함수 하나를 만들고 여기에 다양한 ‘헬퍼’ 함수를 붙이는 식으로 구성된다. [jQuery](https://jquery.com/)는 이름이 `$`인 주요 함수로 이루어져 있다. [lodash](https://lodash.com/)는 주요 함수 `_`에 `_.clone`, `_.keyBy`등의 프로퍼티를 추가하는 식으로 구성된다. 자세한 정보는 lodash [공식 문서](https://lodash.com/docs)에서 찾아볼 수 있고, 이렇게 함수 하나에 다양한 헬퍼 함수를 붙여 라이브러리를 만들면 라이브러리 하나가 전역 변수 하나만 사용하므로 전역 공간을 더럽히지 않는다는 장점이 있으며, 또 이름 충돌도 방지할 수 있다.
>
> 이렇게 객체로서의 함수 특징을 이용해 커스텀 프로퍼티를 만들면 함수는 자기 자신을 이용해 원하는 일을 수행할 수 있고, 함수 자기 자신에 딸린 프로퍼티의 기능도 사용할 수 있다는 장점이 있다.



## 6.7 new Function 문법

함수 표현식과 함수 선언문 이외에 함수를 만들 수도 있는 방법이 하나 더 있는데, 잘 사용하지 않는 방식이긴 하다.

`new Function` 문법을 사용하면 함수를 만들 수 있다.

```javascript
let func = new Function ([arg1, arg2, ...argN], functionBody);
```

새로 만들어지는 함수는 인수 `arg1...argN`과 함수 본문 `functionBody`로 구성된다.

인수 두 개가 있는 함수를 직접 만들어 보면서 `new Function` 문법에 대해 이해해보도록 하자.

```javascript
let sum = new Function('a', 'b', 'return a + b');

alert( sum(1, 2) ); // 3
```

인수가 없고 함수 본문만 있는 함수를 만들어보자.

```javascript
let sayHi = new Function('alert("Hello")');

sayHi(); // Hello
```

기존에 사용하던 방법과 `new Function`을 사용해 함수를 만드는 방법의 가장 큰 차이는 런타임에 받은 문자열을 사용해 함수를 만들 수 있다는 점이다.

함수 표현식과 함수 선언문은 개발자가 직접 스크립트를 작성해야만 함수를 만들 수 있었다.

그러나 `new Function`이라는 문법을 사용하면 어떤 문자열도 함수로 바꿀 수 있다. 서버에서 전달받은 문자열을 이용해 새로운 함수를 만들고 이를 실행하는 것도 가능하다.

```javascript
let str = ... 서버에서 동적으로 전달받은 문자열(코드 형태) ...

let func = new Function(str);
func();
```

서버에서 코드를 받거나 템플릿을 사용해 함수를 동적으로 컴파일해야 하는 경우, 복잡한 웹 애플리케이션을 구현할 때와 같이 아주 특별한 경우에 `new Function`을 사용할 수 있다.



#### 클로저

함수는 특별한 프로퍼티 `[[Environment]]`에 저장된 정보를 이용해 자기 자신이 태어난 곳을 기억한다. `[[Environment]]`는 함수가 만들어진 렉시컬 환경을 참조한다. 그런데 `new Function`을 이용해 함수를 만들면 함수의 `[[Environment]]` 프로퍼티가 현재 렉시컬 환경이 아닌 전역 렉시컬 환경을 참조하게 된다.

따라서 `new Function`을 이용해 만든 함수는 외부 변수에 접근할 수 없고, 오직 전역 변수에만 접근할 수 있다.

```javascript
function getFunc() {
  let value = "test";

  let func = new Function('alert(value)');

  return func;
}

getFunc()(); // ReferenceError: value is not defined
```

일반적인 방법을 사용해 함수를 정의한 예시와 비교해보자.

```javascript
function getFunc() {
  let value = "test";

  let func = function() { alert(value); };

  return func;
}

getFunc()(); // getFunc의 렉시컬 환경에 있는 값 "test"가 출력된다.
```

지금 당장은 `new Function`이 제공하는 특수한 기능이 익숙하지 않을 수 있는데, 실무에선 이 기능이 아주 유용하게 쓰인다.

문자열을 사용해서 함수를 만들어야 한다고 가정해보자. 스크립트를 작성하는 시점엔 어떻게 함수를 짤지 알 수 없어서 기존의 함수 선언 방법은 사용하지 못하는데, 스크립트 실행 시점 즈음엔 함수를 어떻게 짜야 할 지 아이디어가 떠오를 수 있을 것이다. 이때, 서버를 비롯한 외부 출처를 통해 코드를 받아올 수 있다. 그런데 이렇게 만들어진 새 함수는 기존 스크립트와 문제없이 상호작용할 수 있어야 한다.

`new Function`으로 만든 새로운 함수 내부에서 외부 변수에 접근하려 할 때, 기존 함수 선언 방식으로 작성한 함수와 동일한 동작이 보장되어야 한다. 그런데 스크립트가 프로덕션 서버에 반영되기 전, *압축기(minifier)* 에 의해 압축될 때 문제가 발생한다. 압축기는 스크립트에서 주석이나 여분의 공백 등을 없애 코드 크기를 줄여주는 특수한 프로그램인데 압축기가 지역 변수 이름을 짧게 바꾸면서 문제가 발생한다.

구체적으로 어떤 부분이 문제가 되는지 예시를 통해 알아보자. 함수 내부에 `let userName`라는 변수가 있으면 이 지역변수는 압축기에 의해 `let a` 등(짧은 이름)으로 대체되는데, 이때 `userName` 모두가 `a`로 교체된다. `userName`은 지역변수이고, 함수 외부에선 함수 내부에 있는 변수에 접근할 수 없기 때문에 이렇게 해도 전혀 문제가 없다. 압축기는 단순히 변수를 찾아서 바꾸지 않고 코드 구조를 분석해 기존에 작성한 코드의 기능을 망가뜨리지 않으면서 영리하게 제 역할을 수행한다.

이런 동작 방식 때문에 `new Function` 문법으로 만든 함수 내부에서 외부 변수에 접근하려고 하면 `userName`은 이미 이름이 변경되었기 때문에 찾을 수 없게 된다.

**압축기가 동작한 이후엔, `new Function`으로 만든 함수 내부에서 외부 렉시컬 환경에 접근하려고 할 때 문제가 발생할 수 있다.**

이런 실수로부터 예방하기 위해 `new Function`에는 특별한 기능이 있다. 함수 내부에서 외부 변수에 접근하는 것은 아키텍처 관점에서도 좋지 않고 에러에 취약하다. `new Function`으로 만든 함수에 무언갈 넘겨주고 싶다면 인수를 사용하자.

> #### 요약
>
> ```javascript
> let func = new Function ([arg1, arg2, ...argN], functionBody);
> ```
>
> 인수를 한꺼번에 모아(쉼표로 구분) 전달할 수도 있다.
>
> 아래 세 선언 방식은 동일하게 동작한다.
>
> ```javascript
> new Function('a', 'b', 'return a + b'); // 기본 문법
> new Function('a,b', 'return a + b'); // 쉼표로 구분
> new Function('a , b', 'return a + b'); // 쉼표와 공백으로 구분
> ```
>
> `new Function`을 이용해 만든 함수의 `[[Environment]]`는 외부 렉시컬 환경이 아닌 전역 렉시컬 환경을 참조하므로 외부 변수를 사용할 수 없다. 단점 같아 보이는 특징이긴 하지만 에러를 예방해 준다는 관점에선 장점이 되기도 한다. 구조상으론 매개변수를 사용해 값을 받는 게 더 낫다. 압축기에 의한 에러도 방지할 수 있다.



## 6.8 setTimeout과 setInterval을 이용한 호출 스케줄링

일정 시간이 지난 후에 원하는 함수를 예약 실행(호출)할 수 있게 하는 것을 '호출 스케줄링(scheduling a call)'이라고 gㅏㄴ다.

호출 스케줄링을 구현하는 방법은 두 가지가 있다.

- `setTimeout`을 이용해 일정 시간이 지난 후에 함수를 실행하는 방법
- `setInterval`을 이용해 일정 시간 간격을 두고 함수를 실행하는 방법

자바스크립트 명세서엔 `setTimeout`과 `setInterval`가 명시되어있지 않다. 하지만 시중에 나와 있는 모든 브라우저, Node.js를 포함한 자바스크립트 호스트 환경 대부분이 이와 유사한 메서드와 내부 스케줄러를 지원한다.



#### setTimeout

```javascript
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
```

매개변수:

- `func|code`

  실행하고자 하는 코드로, 함수 또는 문자열 형태이다. 대개는 이 자리에 함수가 들어갑니다. 하위 호환성을 위해 문자열도 받을 수 있게 해놓았지만 추천하진 않는다.

- `delay`

  실행 전 대기 시간으로, 단위는 밀리초(millisecond, 1000밀리초 = 1초)이며 기본값은 0이다.

- `arg1`, `arg2`…

  함수에 전달할 인수들로, IE9 이하에선 지원하지 않는다.

예시를 통해 `setTimeout`을 어떻게 쓸 수 있는지 알아봅시다. 아래 코드를 실행하면 1초 후에 `sayHi()`가 호출된다.

```javascript
function sayHi() {
  alert('안녕하세요.');
}

setTimeout(sayHi, 1000);
```



아래와 같이 함수에 인수를 넘겨줄 수도 있다.                      

```javascript
function sayHi(who, phrase) {
  alert( who + ' 님, ' + phrase );
}

setTimeout(sayHi, 1000, "홍길동", "안녕하세요."); // 홍길동 님, 안녕하세요.
```



`setTimeout`의 첫 번째 인수가 문자열이면 자바스크립트는 이 문자열을 이용해 함수를 만든다.

아래 예시가 정상적으로 동작하는 이유이다.

```javascript
setTimeout("alert('안녕하세요.')", 1000);
```



그런데 이렇게 문자열을 사용하는 방법은 추천하지 않는다. 되도록 다음 예시와 같이 익명 화살표 함수를 사용하자.                      

```javascript
setTimeout(() => alert('안녕하세요.'), 1000);
```



초보 개발자는 `setTimeout`에 함수를 넘길 때, 함수 뒤에 `()` 을 붙이는 실수를 하곤 하는데, 함수를 실행하지 말고 넘겨라.

```javascript
// 잘못된 코드
setTimeout(sayHi(), 1000);
```

`setTimeout`은 함수의 참조 값을 받도록 정의되어 있는데 `sayHi()`를 인수로 전달하면 *함수 실행 결과*가 전달되어 버린다. 그런데 `sayHi()`엔 반환문이 없다. 호출 결과는 `undefined`가 되겠죠. 따라서 `setTimeout`은 스케줄링할 대상을 찾지 못해, 원하는 대로 코드가 동작하지 않는다.



#### clearTimeout으로 스케줄링 취소하기

`setTimeout`을 호출하면 '타이머 식별자(timer identifier)'가 반환된다. 스케줄링을 취소하고 싶을 땐 이 식별자(아래 예시에서 `timerId`)를 사용하면 된다.

스케줄링 취소하기:

```javascript
let timerId = setTimeout(...);
clearTimeout(timerId);
```



아래 예시는 함수 실행을 계획해 놓았다가 중간에 마음이 바뀌어 계획해 놓았던 것을 취소한 상황을 코드로 표현하고 있다. 예시를 실행해도 스케줄링이 취소되었기 때문에 아무런 변화가 없는 것을 확인할 수 있다.                      

```javascript

let timerId = setTimeout(() => alert("아무런 일도 일어나지 않는다."), 1000);
alert(timerId); // 타이머 식별자

clearTimeout(timerId);
alert(timerId); // 위 타이머 식별자와 동일함 (취소 후에도 식별자의 값은 null이 되지 않는다.)
```

예시를 실행하면 `alert` 창이 2개가 뜨는데, 이 얼럿 창을 통해 브라우저 환경에선 타이머 식별자가 숫자라는 걸 알 수 있다. 다른 호스트 환경에선 타이머 식별자가 숫자형 이외의 자료형일 수 있다. 참고로 Node.js에서 `setTimeout`을 실행하면 타이머 객체가 반환한다.

다시 한번 말씀드리자면, 스케줄링에 관한 명세는 따로 존재하지 않는다. 명세가 없기 때문에 호스트 환경마다 약간의 차이가 있을 수밖에 없다.

참고로 브라우저는 HTML5의 [timers section](https://www.w3.org/TR/html5/webappapis.html#timers)을 준수하고 있다.



#### setInterval

`setInterval` 메서드는 `setTimeout`과 동일한 문법을 사용한다.

```javascript
let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)
```

인수 역시 동일하지만, `setTimeout`이 함수를 단 한 번만 실행하는 것과 달리 `setInterval`은 함수를 주기적으로 실행하게 만든다. 함수 호출을 중단하려면 `clearInterval(timerId)`을 사용하면 된다. 

다음 예시를 실행하면 메시지가 2초 간격으로 보이다가 5초 이후에는 더 이상 메시지가 보이지 않는다.

```javascript
// 2초 간격으로 메시지를 보여줌
let timerId = setInterval(() => alert('째깍'), 2000);

// 5초 후에 정지
setTimeout(() => { clearInterval(timerId); alert('정지'); }, 5000);
```

`alert` 창이 떠 있더라도 타이머는 멈추지 않는다.

Chrome과 Firefox를 포함한 대부분의 브라우저는 `alert/confirm/prompt` 창이 떠 있는 동안에도 내부 타이머를 멈추지 않는다. 위 예시를 실행하고 첫 번째 `alert` 창이 떴을 때 몇 초간 기다렸다가 창을 닫으면, 두 번째 `alert` 창이 바로 나타나는 것을 보고 이를 확인할 수 있다. 이런 이유로 얼럿 창은 명시한 지연 시간인 2초보다 더 짧은 간격으로 뜨게 된다.



#### 중첩 setTimeout

무언가를 일정 간격을 두고 실행하는 방법에는 크게 2가지가 있는데, 하나는 `setInterval`을 이용하는 방법이고, 다른 하나는 아래 예시와 같이 중첩 `setTimeout`을 이용하는 방법이다.

```javascript
/** setInterval을 이용하지 않고 아래와 같이 중첩 setTimeout을 사용함
let timerId = setInterval(() => alert('째깍'), 2000);
*/

let timerId = setTimeout(function tick() {
  alert('째깍');
  timerId = setTimeout(tick, 2000); // (*)
}, 2000);
```

다섯 번째 줄의 `setTimeout`은 `(*)`로 표시한 줄의 실행이 종료되면 다음 호출을 스케줄링한다.

중첩 `setTimeout`을 이용하는 방법은 `setInterval`을 사용하는 방법보다 유연하다. 호출 결과에 따라 다음 호출을 원하는 방식으로 조정해 스케줄링 할 수 있기 때문이다.

5초 간격으로 서버에 요청을 보내 데이터를 얻는다고 가정해 보면, 서버가 과부하 상태라면 요청 간격을 10초, 20초, 40초 등으로 증가시켜주는 게 좋을 것이다.

아래는 이를 구현한 의사 코드이다.

```javascript
let delay = 5000;

let timerId = setTimeout(function request() {
  ...요청 보내기...

  if (서버 과부하로 인한 요청 실패) {
    // 요청 간격을 늘립니다.
    delay *= 2;
  }

  timerId = setTimeout(request, delay);

}, delay);
```



CPU 소모가 많은 작업을 주기적으로 실행하는 경우에도 `setTimeout`을 재귀 실행하는 방법이 유용하다. 작업에 걸리는 시간에 따라 다음 작업을 유동적으로 계획할 수 있기 때문=이다.

**중첩 `setTimeout`을 이용하는 방법은 지연 간격을 보장하지만 `setInterval`은 이를 보장하지 않는다.**

아래 두 예시를 비교해 보면, 첫 번째 예시에선 `setinterval`을 이용했다.

```javascript
let i = 1;
setInterval(function() {
  func(i++);
}, 100);
```



두 번째 예시에선 중첩 `setTimeout`을 이용하였다.

```javascript
let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);
```

첫 번째 예시에선, 내부 스케줄러가 `func(i++)`를 100밀리초마다 실행한다.

![Screen Shot 2021-06-14 at 9 12 54 PM](https://user-images.githubusercontent.com/79819941/121890550-709a3280-cd55-11eb-8aac-4cdac0078521.png) 

**`setInterval`을 사용하면 `func`호출 사이의 지연 간격이 실제 명시한 간격(100ms)보다 짧아집니다!**

이는 `func`을 실행하는 데 ‘소모되는’ 시간도 지연 간격에 포함시키기 때문입니다. 지극히 정상적인 동작이죠.

그렇다면 `func`을 실행하는 데 걸리는 시간이 명시한 지연 간격보다 길 때 어떤 일이 발생할까요?

이런 경우는 엔진이 `func`의 실행이 종료될 때까지 기다려줍니다. `func`의 실행이 종료되면 엔진은 스케줄러를 확인하고, 지연 시간이 지났으면 다음 호출을 *바로* 시작합니다.

따라서 함수 호출에 걸리는 시간이 매번 `delay` 밀리초보다 길면, 모든 함수가 쉼 없이 계속 연속 호출됩니다.

한편, 중첩 `setTimeout`을 이용하면 다음과 같이 실행 흐름이 이어집니다.

![Screen Shot 2021-06-14 at 9 13 00 PM](https://user-images.githubusercontent.com/79819941/121890485-5f512600-cd55-11eb-9b82-a7af87fb0e44.png) 

**중첩 `setTimeout`을 사용하면 명시한 지연(여기서는 100ms)이 보장된다.**

이렇게 지연 간격이 보장되는 이유는 이전 함수의 실행이 종료된 이후에 다음 함수 호출에 대한 계획이 세워지기 때문이다.



#### 가비지 컬렉션과 setInterval·setTimeout

`setInterval`이나 `setTimeout`에 함수를 넘기면, 함수에 대한 내부 참조가 새롭게 만들어지고 이 참조 정보는 스케줄러에 저장된다. 따라서 해당 함수를 참조하는 것이 없어도 `setInterval`과 `setTimeout`에 넘긴 함수는 가비지 컬렉션의 대상이 되지 않는다.

```javascript
// 스케줄러가 함수를 호출할 때까지 함수는 메모리에 유지된다.
setTimeout(function() {...}, 100);
```

`setInterval`의 경우는, `clearInterval`이 호출되기 전까지 함수에 대한 참조가 메모리에 유지된다.

그런데 이런 동작 방식에는 부작용이 하나 있다. 외부 렉시컬 환경을 참조하는 함수가 있다고 가정해 보면, 이 함수가  메모리에 남아있는 동안엔 외부 변수 역시 메모리에 남아있기 마련이다. 그런데 이렇게 되면 실제 함수가 차지했어야 하는 공간보다 더 많은 메모리 공간이 사용된다. 이런 부작용을 방지하고 싶다면 스케줄링할 필요가 없어진 함수는 아무리 작더라도 취소하도록  한다.



## 대기 시간이 0인 setTimeout

`setTimeout(func, 0)`이나 `setTimeout(func)`을 사용하면 `setTimeout`의 대기 시간을 0으로 설정할 수 있다.

이렇게 대기 시간을 0으로 설정하면 `func`을 ‘가능한 한’ 빨리 실행할 수 있지만, 이때 스케줄러는 현재 실행 중인 스크립트의 처리가 종료된 이후에 스케줄링한 함수를 실행한다.

이런 특징을 이용하면 현재 스크립트의 실행이 종료된 ‘직후에’ 원하는 함수가 실행될 수 있게 할 수 있다.

예시를 실행하면 얼럿창에 'Hello’와 'World’가 순서대로 출력되는 것을 확인할 수 있다.

```javascript
setTimeout(() => alert("World"));

alert("Hello");
```

예시에서 첫 번째 줄은 '0밀리초 후에 함수 호출하기’라는 할 일을 '계획표에 기록’해주는 역할을  한다. 그런데 스케줄러는 현재 스크립트(alert 함수)의 실행이 종료되고 나서야 '계획표에 어떤 할 일이 적혀있는지  확인’하므로, `Hello`가 먼저, `World`은 그다음에 출력된다.

**브라우저 환경에서 실제 대기 시간은 0이 아니다.**

브라우저는 [HTML5 표준](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timers)에서 정한 중첩 타이머 실행 간격 관련 제약을 준수하며, 해당 표준엔 "다섯 번째 중첩 타이머 이후엔 대기 시간을 최소 4밀리초 이상으로 강제해야 한다."라는 제약이 명시되어있다.

예시를 보며 이 제약 사항을 이해해보자. 예시 내 `setTimeout`은 지연 없이 함수 run을 다시 호출할 수 있게 스케줄링 되어 있다. 배열 `times`에는 실제 지연 간격에 대한 정보가 기록되도록 해놓았는데, 배열 times에 어떤 값이 저장되는지 알아보자.                   

```javascript
let start = Date.now();
let times = [];

setTimeout(function run() {
  times.push(Date.now() - start); // 이전 호출이 끝난 시점과 현재 호출이 시작된 시점의 시차를 기록

  if (start + 100 < Date.now()) alert(times); // 지연 간격이 100ms를 넘어가면, array를 얼럿창에 띄워줌
  else setTimeout(run); // 지연 간격이 100ms를 넘어가지 않으면 재스케줄링함
});

// 출력창 예시:
// 1,1,1,1,9,15,20,24,30,35,40,45,50,55,59,64,70,75,80,85,90,95,100
```

앞쪽 타이머들은 스펙에 적힌 것처럼 지연 없이 바로 실행된다. 그런데 다섯 번째 중첩 타이머 이후엔 지연 간격이 4밀리초 이상이 되어 `9, 15, 20, 24...`와 같은 값이 저장되는 것을 확인할 수 있다.

이런 제약은 `setTimeout`뿐만 아니라 `setInterval`에도 적용된다. `setInterval(f)`도 처음 몇 번은 함수 `f`를 지연 없이 실행하지만, 나중엔 지연 간격을 4밀리초 이상으로 늘려버린다.

이는 오래전부터 있던 제약인데, 구식 스크립트 중 일부는 아직 이 제약에 의존하는 경우가 있어서 명세서를 변경하지 못하고 있는 상황이다.

한편, 서버 측엔 이런 제약이 없다. Node.js의 [process.nextTick](https://nodejs.org/api/process.html)과 [setImmediate](https://nodejs.org/api/timers.html)를 이용하면 비동기 작업을 지연 없이 실행할 수 있고 위에서 언급된 제약은 브라우저에 한정된다.

## 

> #### 요약
>
> - `setInterval(func, delay, ...args)`과 `setTimeout(func, delay, ...args)`은 `delay`밀리초 후에 `func`을 규칙적으로, 또는 한번 실행하도록 해준다.
> - `setInterval·setTimeout`을 호출하고 반환받은 값을 `clearInterval·clearTimeout`에 넘겨주면 스케줄링을 취소할 수 있다.
> - 중첩 `setTimeout`을 사용하면 `setInterval`을 사용한 것보다 유연하게 코드를 작성할 수 있다. 여기에 더하여 *지연 간격* 보장이라는 장점도 있다.
> - 대기 시간이 0인 setTimeout(`setTimeout(func, 0)` 혹은 `setTimeout(func)`)을 사용하면 ‘현재 스크립트의 실행이 완료된 후 가능한 한 빠르게’ 원하는 함수를 호출할 수 있다.
> - 지연 없이 중첩 `setTimeout`을 5회 이상 호출하거나 지연 없는 `setInterval`에서 호출이 5회 이상 이뤄지면, 4밀리초 이상의 지연 간격이 강제로 더해진다. 이는 브라우저에만 적용되는 사항이며, 하위 호환성을 위해 유지되고 있다.
>
> 스케줄링 메서드를 사용할 땐 명시한 지연 간격이 *보장*되지 않을 수도 있다는 점에 유의해야 한다.
>
> 아래와 같은 상황에서 브라우저 내 타이머가 느려지면 지연 간격이 보장되지 않는다.
>
> - CPU가 과부하 상태인 경우
> - 브라우저 탭이 백그라운드 모드인 경우
> - 노트북이 배터리에 의존해서 구동 중인 경우
>
> 이런 상황에서 타이머의 최소 지연 시간은 300밀리초에서 심하면 1,000밀리초까지 늘어나며, 연장 시간은 브라우저나 구동 중인 운영 체제의 성능 설정에 따라 다르다.

