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

