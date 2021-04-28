## 4.5 'new' 연산자와 생성자 함수

객체 리터럴 `{...}` 을 사용하면 객체를 쉽게 만들 수 있다. 그런데 개발을 하다 보면 유사한 객체를 여러 개 만들어야 할 때가 생기곤 합니다. 복수의 사용자, 메뉴 내 다양한 아이템을 객체로 표현하려고 하는 경우가 그렇다.

`"new"` 연산자와 생성자 함수를 사용하면 유사한 객체 여러 개를 쉽게 만들 수 있다.





#### [생성자 함수](https://ko.javascript.info/constructor-new#ref-3179)

생성자 함수(constructor function)와 일반 함수에 기술적인 차이는 없다. 다만 생성자 함수는 아래 두 관례를 따른다.

1. 함수 이름의 첫 글자는 대문자로 시작한다.
2. 반드시 `"new"` 연산자를 붙여 실행한다.

```javascript
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```

`new User(...)`를 써서 함수를 실행하면 아래와 같은 알고리즘이 동작한다.

1. 빈 객체를 만들어 `this`에 할당한다.
2. 함수 본문을 실행합니다. `this`에 새로운 프로퍼티를 추가해 `this`를 수정한다.
3. `this`를 반환한다.



예시를 이용해 `new User(...)`가 실행되면 무슨 일이 일어나는지 살펴 보도록 하자.

```javascript
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this;  (this가 암시적으로 반환됨)
}
```

이제 `let user = new User("Jack")`는 아래 코드를 입력한 것과 동일하게 동작한다.

```javascript
let user = {
  name: "Jack",
  isAdmin: false
};
```

`new User("Jack")`이외에도 `new User("Ann")`, `new User("Alice")` 등을 이용하면 손쉽게 사용자 객체를 만들 수 있다. 객체 리터럴 문법으로 일일이 객체를 만드는 방법보다 훨씬 간단하고 읽기 쉽게 객체를 만들 수 있게 되었다.

생성자의 의의는 바로 여기에 있다. 재사용할 수 있는 객체 생성 코드를 구현하는 것이다.

잠깐! 모든 함수는 생성자 함수가 될 수 있다는 점을 잊지 말자. `new`를 붙여 실행한다면 어떤 함수라도 위에 언급된 알고리즘이 실행된다. 이름 "첫 글자가 대문자"인 함수는 `new`를 붙여 실행해야 한다는 점도 잊지 말자.

**new function() { … }**

재사용할 필요가 없는 복잡한 객체를 만들어야 한다고 가정해보면, 많은 양의 코드가 필요할 것이다. 이럴 땐 아래와 같이 코드를 익명 생성자 함수로 감싸주는 방식을 사용할 수 있다.

```javascript
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // 사용자 객체를 만들기 위한 여러 코드.
  // 지역 변수, 복잡한 로직, 구문 등의
  // 다양한 코드가 여기에 들어간다.
};
```

위 생성자 함수는 익명 함수이기 때문에 어디에도 저장되지 않는다. 처음 만들 때부터 단 한 번만 호출할 목적으로 만들었기 때문에 재사용이 불가능하다. 이렇게 익명 생성자 함수를 이용하면 재사용은 막으면서 코드를 캡슐화 할 수 있다.





#### [new.target과 생성자 함수](https://ko.javascript.info/constructor-new#ref-3180)

**심화 학습**

이 절에서 소개할 문법은 자주 쓰이지 않는다. 자바스크립트의 모든 문법을 학습하고 싶지 않다면 넘어가셔도 좋다.

`new.target` 프로퍼티를 사용하면 함수가 `new`와 함께 호출되었는지 아닌지를 알 수 있다.

일반적인 방법으로 함수를 호출했다면 `new.target`은 undefined를 반환한다. 반면 `new`와 함께 호출한 경우엔 `new.target`은 함수 자체를 반환해준다.

```javascript
function User() {
  alert(new.target);
}

// "new" 없이 호출함
User(); // undefined

//"new"를 붙여 호출함
new User(); // function User { ... }
```

함수 본문에서 `new.target`을 사용하면 해당 함수가 `new`와 함께 호출되었는지(“in constructor mode”) 아닌지(“in regular mode”)를 확인할 수 있다.

이를 활용해 일반적인 방법으로 함수를 호출해도 `new`를 붙여 호출한 것과 같이 동작하도록 만들어보자.

```javascript
function User(name) {
  if (!new.target) { // new 없이 호출해도
    return new User(name); // new를 붙여준다.
  }

  this.name = name;
}

let john = User("John"); // 'new User'를 쓴 것처럼 바꿔준다.
alert(john.name); // John
```

라이브러리를 분석하다 보면 위와 같은 방식이 쓰인 걸 발견할 때가 있을 것이다. 이런 방식을 사용하면 `new`를 붙여 함수를 호출하든 아니든 코드가 동일하게 동작하기 때문에, 좀 더 유연하게 코드를 작성할 수 있다.

그런데 `new`를 생략하면 코드가 정확히 무슨 일을 하는지 알기 어렵다. `new`가 붙어있으면 새로운 객체를 만든다는 걸 누구나 알 수 있는 반면에 말이다. 이 방법은 정말 필요한 경우에만 사용하시고 남발하지 말자.





#### [생성자와 return문](https://ko.javascript.info/constructor-new#ref-3181)

생성자 함수엔 보통 `return` 문이 없다. 반환해야 할 것들은 모두 `this`에 저장되고, `this`는 자동으로 반환되기 때문에 반환문을 명시적으로 써 줄 필요가 없다.

그런데 만약 `return` 문이 있다면 어떤 일이 벌어질까? 아래와 같은 간단한 규칙이 적용된다.

- 객체를 `return` 한다면, `this` 대신 객체가 반환된다.
- 원시형을 `return` 한다면, `return`문이 무시된다.

`return` 뒤에 객체가 오면 생성자 함수는 해당 객체를 반환해주고, 이 외의 경우는 `this`가 반환된다.

아래 예시에선 첫 번째 규칙이 적용돼, `return`은 `this`를 무시하고 객체를 반환한다.

```javascript
function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- this가 아닌 새로운 객체를 반환함
}

alert( new BigUser().name );  // Godzilla
```

아무것도 `return`하지 않는 예시를 살펴보자. 원시형을 반환하는 경우와 마찬가지로 두 번째 규칙이 적용된다.

```javascript
function SmallUser() {

  this.name = "John";

  return; // <-- this를 반환함
}

alert( new SmallUser().name );  // John
```

`return`문이 있는 생성자 함수는 거의 없다. 여기선 튜토리얼의 완성도를 위해 특이 케이스를 소개하였다.

**괄호 생략하기**

인수가 없는 생성자 함수는 괄호를 생략해 호출할 수 있다.

```javascript
let user = new User; // <-- 괄호가 없음
// 아래 코드는 위 코드와 똑같이 동작한다.
let user = new User();
```

명세서엔 괄호를 생략해도 된다고 정의되어 있지만, "좋은 스타일"은 아니다.





#### [생성자 내 메서드](https://ko.javascript.info/constructor-new#ref-3182)

생성자 함수를 사용하면 매개변수를 이용해 객체 내부를 자유롭게 구성할 수 있다. 엄청난 유연성이 확보된다..

지금까진 `this`에 프로퍼티를 더해주는 예시만 살펴봤는데, 메서드를 더해주는 것도 가능하다.

아래 예시에서 `new User(name)`는 프로퍼티 `name`과 메서드 `sayHi`를 가진 객체를 만들어준다.

```javascript
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

let john = new User("John");

john.sayHi(); // My name is: John

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```

참고로, [class](https://ko.javascript.info/classes) 문법을 사용하면 생성자 함수를 사용하는 것과 마찬가지로 복잡한 객체를 만들 수 있다.



>#### [요약](https://ko.javascript.info/constructor-new#ref-3183)
>
>- 생성자 함수(짧게 줄여서 생성자)는 일반 함수이다. 다만, 일반 함수와 구분하기 위해 함수 이름 첫 글자를 대문자로 쓴다.
>- 생성자 함수는 반드시 `new` 연산자와 함께 호출해야 한다. `new`와 함께 호출하면 내부에서 `this`가 암시적으로 만들어지고, 마지막엔 `this`가 반환된다.
>
>유사한 객체를 여러 개 만들 때 생성자 함수가 유용하다.
>
>자바스크립트는 언어 차원에서 다양한 생성자 함수를 제공한다. 날짜를 나타내는 데 쓰이는 `Date`, 집합(set)을 나타내는 데 쓰이는 `Set` 등의 내장 객체는 이런 생성자 함수를 이용해 만들 수 있다. 







## 4.6 옵셔널 체이닝 '?.'

>  ❗**최근에 추가됨**
>
> 스펙에 추가된 지 얼마 안 된 문법이다. 그렇기 때문에 구식 브라우저는 폴리필이 필요하다.
>
> 옵셔널 체이닝(optional chaining) `?.`을 사용하면 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근할 수 있다.





#### [옵셔널 체이닝이 필요한 이유](https://ko.javascript.info/optional-chaining#ref-3201)

이제 막 자바스크립트를 배우기 시작했다면 옵셔널 체이닝이 등장하게 된 배경 상황을 직접 겪어보지 않았을 겁니다. 몇 가지 사례를 재현하면서 왜 옵셔널 체이닝이 등장했는지 알아보자.

사용자가 여러 명 있는데 그중 몇 명은 주소 정보를 가지고 있지 않다고 가정해봅시다. 이럴 때 `user.address.street`를 사용해 주소 정보에 접근하면 에러가 발생할 수 있다.

```javascript
let user = {}; // 주소 정보가 없는 사용자

alert(user.address.street); // TypeError: Cannot read property 'street' of undefined
```

또 다른 사례는 브라우저에서 동작하는 코드를 개발할 때 발생할 수 있는 문제로, 페이지에 존재하지 않는 요소에 접근해 요소의 정보를 가져오려 할 때 발생한다.

```javascript
// querySelector(...) 호출 결과가 null인 경우 에러 발생
let html = document.querySelector('.my-element').innerHTML;
```

명세서에 `?.`이 추가되기 전엔 이런 문제들을 해결하기 위해 `&&` 연산자를 사용하곤 했었다.

예시:

```javascript
let user = {}; // 주소 정보가 없는 사용자

alert( user && user.address && user.address.street ); // undefined, 에러가 발생하지 않는다.
```

중첩 객체의 특정 프로퍼티에 접근하기 위해 거쳐야 할 구성요소들을 AND로 연결해 실제 해당 객체나 프로퍼티가 있는지 확인하는 방법을 사용했었죠. 그런데 이렇게 AND를 연결해서 사용하면 코드가 아주 길어진다는 단점이 있다.





#### [옵셔널 체이닝의 등장](https://ko.javascript.info/optional-chaining#ref-3202)

`?.`은 `?.`'앞’의 평가 대상이 `undefined`나 `null`이면 평가를 멈추고 `undefined`를 반환합니다.

**설명이 장황해지지 않도록 지금부턴 평가후 결과가 `null`이나 `undefined`가 아닌 경우엔 값이 ‘있다’, '존재한다’라고 표현하겠습니다.**

옵셔널 체이닝을 사용해 `user.address.street`에 안전하게 접근해보자.

```javascript
let user = {}; // 주소 정보가 없는 사용자

alert( user?.address?.street ); // undefined, 에러가 발생하지 않는다.
```

`user?.address`로 주소를 읽으면 아래와 같이 `user` 객체가 존재하지 않더라도 에러가 발생하지 않는다.

```javascript
let user = null;

alert( user?.address ); // undefined
alert( user?.address.street ); // undefined
```

위 예시를 통해 우리는 `?.`은 `?.` ‘앞’ 평가 대상에만 동작되고, 확장은 되지 않는다는 사실을 알 수 있다.

위의 예제에서, `user?.` 는  `user` 만  `null/undefined` 가 되도록 허용한다.

반면에  `user` 가 존재한다면  `user.address` 속성이 있어야 하며,  `user?.address.street` 은 두번째 점에서 오류가 있다.



**옵셔널 체이닝을 남용하지 말자.**

`?.`는 존재하지 않아도 괜찮은 대상에만 사용해야 한다.

사용자 주소를 다루는 위 예시에서 논리상 `user`는 반드시 있어야 하는데 `address`는 필수값이 아니다. 그러니 `user.address?.street`를 사용하는 것이 바람직하다.

실수로 인해 `user`에 값을 할당하지 않았다면 바로 알아낼 수 있도록 해야 한다. 그렇지 않으면 에러를 조기에 발견하지 못하고 디버깅이 어려워진다.

**`?.`앞의 변수는 꼭 선언되어 있어야 한다.**

변수 `user`가 선언되어있지 않으면 `user?.anything` 평가시 에러가 발생한다.

```javascript
// ReferenceError: user is not defined
user?.address;
```

`let/const/var user` 가 있어야 한다.  옵셔널 체이닝은 선언 된 변수에 대해서만 동작한다.





#### [단락 평가](https://ko.javascript.info/optional-chaining#ref-3203)

`?.`는 왼쪽 평가대상에 값이 없으면 즉시 평가를 멈춘다. 참고로 이런 평가 방법을 단락 평가(short-circuit)라고 부른다.

그렇기 때문에 함수 호출을 비롯한 `?.` 오른쪽에 있는 부가 동작은 `?.`의 평가가 멈췄을 때 더는 일어나지 않는다.

```javascript
let user = null;
let x = 0;

user?.sayHi(x++); // 아무 일도 일어나지 않습니다.

alert(x); // 0, x는 증가하지 않습니다.
```





#### [?.()와 ?.[\]](https://ko.javascript.info/optional-chaining#ref-3204)

`?.`은 연산자가 아닙니다. `?.`은 함수나 대괄호와 함께 동작하는 특별한 문법 구조체(syntax construct)이다.

함수 관련 예시와 함께 존재 여부가 확실치 않은 함수를 호출할 때 `?.()`를 어떻게 쓸 수 있는지 알아보자.

한 객체엔 메서드 `admin`이 있지만 다른 객체엔 없는 상황이다.

```javascript
let user1 = {
  admin() {
    alert("관리자 계정입니다.");
  }
}

let user2 = {};

user1.admin?.(); // 관리자 계정입니다.
user2.admin?.();
```

두 상황 모두에서 user 객체는 존재하기 때문에 `admin` 프로퍼티는 `.`만 사용해 접근했다.

그리고 난 후 `?.()`를 사용해 `admin`의 존재 여부를 확인했다. `user1`엔 `admin`이 정의되어 있기 때문에 메서드가 제대로 호출되었다. 반면 `user2`엔 `admin`이 정의되어 있지 않았음에도 불구하고 메서드를 호출하면 에러 없이 그냥 평가가 멈추는 것을 확인할 수 있다.

`.`대신 대괄호 `[]`를 사용해 객체 프로퍼티에 접근하는 경우엔 `?.[]`를 사용할 수도 있다. 위 예시와 마찬가지로 `?.[]`를 사용하면 객체 존재 여부가 확실치 않은 경우에도 안전하게 프로퍼티를 읽을 수 있다.

```javascript
let user1 = {
  firstName: "Violet"
};

let user2 = null; // user2는 권한이 없는 사용자라고 가정해보자.

let key = "firstName";

alert( user1?.[key] ); // Violet
alert( user2?.[key] ); // undefined

alert( user1?.[key]?.something?.not?.existing); // undefined
```

`?.`은 `delete`와 조합해 사용할 수도 있다.

```javascript
delete user?.name; // user가 존재하면 user.name을 삭제한다.
```

**`?.`은 읽기나 삭제하기에는 사용할 수 있지만 쓰기에는 사용할 수 없다.**

`?.`은 할당 연산자 왼쪽에서 사용할 수 없다.

```javascript
// user가 존재할 경우 user.name에 값을 쓰려는 의도로 아래와 같이 코드를 작성해 보았다.

user?.name = "Violet"; // SyntaxError: Invalid left-hand side in assignment
// 에러가 발생하는 이유는 undefined = "Violet"이 되기 때문이다.
```



> #### [요약](https://ko.javascript.info/optional-chaining#ref-3205)
>
>옵셔널 체이닝 문법 `?.`은 세 가지 형태로 사용할 수 있다.
>
>1. `obj?.prop` – `obj`가 존재하면 `obj.prop`을 반환하고, 그렇지 않으면 `undefined`를 반환함
>2. `obj?.[prop]` – `obj`가 존재하면 `obj[prop]`을 반환하고, 그렇지 않으면 `undefined`를 반환함
>3. `obj?.method()` – `obj`가 존재하면 `obj.method()`를 호출하고, 그렇지 않으면 `undefined`를 반환함
>
>여러 예시를 통해 살펴보았듯이 옵셔널 체이닝 문법은 꽤 직관적이고 사용하기도 쉽습니다. `?.` 왼쪽 평가 대상이 `null`이나 `undefined`인지 확인하고 `null`이나 `undefined`가 아니라면 평가를 계속 진행한다.
>
>`?.`를 계속 연결해서 체인을 만들면 중첩 프로퍼티들에 안전하게 접근할 수 있다.
>
>`?.`은 `?.`왼쪽 평가대상이 없어도 괜찮은 경우에만 선택적으로 사용해야 한다.
>
>꼭 있어야 하는 값인데 없는 경우에 `?.`을 사용하면 프로그래밍 에러를 쉽게 찾을 수 없으므로 이런 상황을 만들지 말도록 하자.

