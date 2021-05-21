## 4.5 'new' 연산자와 생성자 함수

객체 리터럴 `{...}` 을 사용하면 객체를 쉽게 만들 수 있다. 그런데 개발을 하다 보면 유사한 객체를 여러 개 만들어야 할 때가 생기곤 하는데, 특히 복수의 사용자, 메뉴 내 다양한 아이템을 객체로 표현하려고 하는 경우가 그렇다.

`"new"` 연산자와 생성자 함수를 사용하면 유사한 객체 여러 개를 번거롭지 않게 쉽고 빠르게 만들 수 있다.







#### 생성자 함수

생성자 함수(constructor function)와 일반 함수에 기술적인 차이는 없지만, 생성자 함수는 아래 두 관례를 따른다.

1. 함수 이름의 **첫 글자는 대문자**로 시작한다.
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

**모든 함수는 생성자 함수가 될 수 있다.** `new`를 붙여 실행한다면 어떤 함수라도 위에 언급된 알고리즘이 실행된다. 이름 "첫 글자가 대문자"인 함수는 `new`를 붙여 실행해야 한다는 점도 잊지 말자.



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

위 생성자 함수는 익명 함수이기 때문에 어디에도 저장되지 않는다. 처음 만들 때부터 단 한 번만 호출할 목적으로 만들었기 때문에 재사용이 불가능하다. 이렇게 익명 생성자 함수를 이용하면 재사용은 막으면서 코드를 **캡슐화** 할 수 있다.





#### new.target과 생성자 함수

**심화 학습**

자바스크립트의 모든 문법을 학습하고 싶지 않다면 넘어가도 좋을 정도로 이 절에서 소개할 문법은 자주 쓰이지 않는다. 

`new.target` 프로퍼티를 사용하면 함수가 `new`와 함께 호출되었는지 아닌지를 알 수 있는데, 일반적인 방법으로 함수를 호출했다면 `new.target`은 undefined를 반환한다. 반면 `new`와 함께 호출한 경우엔 `new.target`은 함수 자체를 반환해준다.

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





#### 생성자와 return문

**생성자 함수엔 보통 `return` 문이 없다. 반환해야 할 것들은 모두 `this`에 저장되고, `this`는 자동으로 반환되기 때문에 반환문을 명시적으로 써 줄 필요가 없다.**

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

`return`문이 있는 생성자 함수는 거의 없고, 튜토리얼의 완성도를 위해 특이 케이스를 소개한 것이다.



**괄호 생략하기**

인수가 없는 생성자 함수는 괄호를 생략해 호출할 수 있다.

```javascript
let user = new User; // <-- 괄호가 없음
// 아래 코드는 위 코드와 똑같이 동작한다.
let user = new User();
```

명세서엔 괄호를 생략해도 된다고 정의되어 있지만, "좋은 스타일"은 아니다.





#### 생성자 내 메서드

생성자 함수를 사용하면 매개변수를 이용해 객체 내부를 자유롭게 구성할 수 있다. 엄청난 유연성이 확보된다. 🤸

지금까진 `this`에 프로퍼티를 더해주는 예시만 살펴봤는데, 메서드를 더해주는 것도 가능한데, 아래 예시에서 `new User(name)`는 프로퍼티 `name`과 메서드 `sayHi`를 가진 객체를 만들어준다.

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





>#### 요약
>
>- 생성자 함수(짧게 줄여서 생성자)는 일반 함수이다. 다만, 일반 함수와 구분하기 위해 함수 이름 첫 글자를 대문자로 쓴다.
>- 생성자 함수는 반드시 `new` 연산자와 함께 호출해야 한다. `new`와 함께 호출하면 내부에서 `this`가 암시적으로 만들어지고, 마지막엔 `this`가 반환된다.
>
>유사한 객체를 여러 개 만들 때 생성자 함수가 유용하다.
>
>자바스크립트는 언어 차원에서 다양한 생성자 함수를 제공한다. 
>
>날짜를 나타내는 데 쓰이는 `Date`, 집합(set)을 나타내는 데 쓰이는 `Set` 등의 내장 객체는 이런 생성자 함수를 이용해 만들 수 있다. 














## 4.6 옵셔널 체이닝 '?.'

>  ❗**최근에 추가됨**
>
> 스펙에 추가된 지 얼마 안 된 문법이다. 그렇기 때문에 구식 브라우저는 폴리필이 필요하다.
>
> 옵셔널 체이닝(optional chaining) `?.`을 사용하면 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근할 수 있다.







#### 옵셔널 체이닝이 필요한 이유

아래와 같이 사용자가 여러 명 있는데 그중 몇 명은 주소 정보를 가지고 있지 않다고 가정해 보면  `user.address.street`를 사용해 주소 정보에 접근하면 에러가 발생할 수 있다.

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







#### 옵셔널 체이닝의 등장

`?.`은 `?.`'앞’의 평가 대상이 `undefined`나 `null`이면 평가를 멈추고 `undefined`를 반환하는데, 옵셔널 체이닝을 사용해 `user.address.street`에 안전하게 접근해보자.

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

사용자 주소를 다루는 위 예시에서 논리상 `user`는 반드시 있어야 하는데 `address`는 필수값이 아니다. 그러니 `user.address?.street`를 사용하는 것이 바람직하다. 실수로 인해 `user`에 값을 할당하지 않았다면 바로 알아낼 수 있도록 해야 한다. 그렇지 않으면 에러를 조기에 발견하지 못하고 디버깅이 어려워진다.



**`?.`앞의 변수는 꼭 선언되어 있어야 한다.**

변수 `user`가 선언되어있지 않으면 `user?.anything` 평가시 에러가 발생한다.

```javascript
// ReferenceError: user is not defined
user?.address;
```

`let/const/var user` 가 있어야 한다.  옵셔널 체이닝은 선언 된 변수에 대해서만 동작한다.





#### 단락 평가

`?.`는 왼쪽 평가대상에 값이 없으면 즉시 평가를 멈춘다. 참고로 이런 평가 방법을 단락 평가(short-circuit)라고 부른다.

그렇기 때문에 함수 호출을 비롯한 `?.` 오른쪽에 있는 부가 동작은 `?.`의 평가가 멈췄을 때 더는 일어나지 않는다.

```javascript
let user = null;
let x = 0;

user?.sayHi(x++); // 아무 일도 일어나지 않습니다.

alert(x); // 0, x는 증가하지 않습니다.
```





#### [?.()와 ?.]

`?.`은 연산자가 아니라.  `?.`은 함수나 대괄호와 함께 동작하는 특별한 문법 구조체(syntax construct)이다.

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



> #### 요약
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











## 4.7 심볼형

자바스크립트는 **객체 프로퍼티 키로 오직 문자형과 심볼형만을 허용**하는데, 숫자형, 불린형 모두 불가능하고 오직 문자형과 심볼형만 가능하다.

지금까지는 프로퍼티 키가 문자형인 경우만 살펴보았는데, 이번 챕터에선 프로퍼티 키로 심볼값을 사용해 보면서, 심볼형 키를 사용할 때의 이점에 대해 살펴보도록 하자.





#### 심볼



**'심볼(symbol)'은 유일한 식별자(unique identifier)를 만들고 싶을 때 사용**하며, `Symbol()`을 사용하면 심볼값을 만들 수 있다.

```javascript
// id는 새로운 심볼이 된다.
let id = Symbol();
```



**심볼을 만들 때 심볼 이름이라 불리는 설명을 붙일 수도 있다. 심볼 이름은 디버깅 시 아주 유용하다.**

```javascript
// 심볼 id에는 "id"라는 설명이 붙는다.
let id = Symbol("id");
```

심볼은 유일성이 보장되는 자료형이기 때문에, 설명이 동일한 심볼을 여러 개 만들어도 각 심볼값은 다르다. **심볼에 붙이는 설명(심볼 이름)은 어떤 것에도 영향을 주지 않는 이름표 역할만을 한다.**



설명이 같은 심볼 두 개를 만들고 이를 비교해보자. 동일 연산자(`==`)로 비교 시 `false`가 반환되는 것을 확인할 수 있다.

```javascript
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false
```

참고로 Ruby 등의 언어에서도 '심볼’과 유사한 개념을 사용하는데, 자바스크립트의 심볼은 이들 언어에 쓰이는 심볼과는 다르기 때문에 혼동하지 말자.





**심볼은 문자형으로 자동 형 변환되지 않는다.**

자바스크립트에선 문자형으로의 암시적 형 변환이 비교적 자유롭게 일어나는 편이다. `alert` 함수가 거의 모든 값을 인자로 받을 수 있는 이유가 이 때문이다. 그러나 심볼은 예외인데, **심볼형 값은 다른 자료형으로 암시적 형 변환(자동 형 변환)되지 않는다.**



아래 예시에서 `alert`는 에러를 발생시킨다.

```javascript
let id = Symbol("id");
alert(id); // TypeError: Cannot convert a Symbol value to a string
```

**문자열과 심볼은 근본이 다르기 때문에 우연히라도 서로의 타입으로 변환돼선 안 된다. 자바스크립트에선 '언어 차원의 보호장치(language guard)'를 마련해 심볼형이 다른 형으로 변환되지 않게 막아준다.**



심볼을 반드시 출력해줘야 하는 상황이라면 아래와 같이 `.toString()` 메서드를 명시적으로 호출해주면 된다.

```javascript
let id = Symbol("id");
alert(id.toString()); // Symbol(id)가 얼럿 창에 출력됨
```



`symbol.description` 프로퍼티를 이용하면 설명만 보여주는 것도 가능하다.

```javascript
let id = Symbol("id");
alert(id.description); // id
```





####  ‘숨김’ 프로퍼티

심볼을 이용하면 ‘숨김(hidden)’ 프로퍼티를 만들 수 있다. **숨김 프로퍼티는 외부 코드에서 접근이 불가능하고 값도 덮어쓸 수 없는 프로퍼티이다.**

서드파티 코드에서 가지고 온 `user`라는 객체가 여러 개 있고, `user`를 이용해 어떤 작업을 해야 하는 상황이라고 가정해 보자.  그리고 `user`에 식별자를 붙여주고 식별자는 심볼을 이용해 만들도록 하자.

```javascript
let user = { // 서드파티 코드에서 가져온 객체
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // 심볼을 키로 사용해 데이터에 접근할 수 있다.
```

그런데 문자열 `"id"`를 키로 사용해도 되는데 `Symbol("id")`을 사용한 이유가 무엇일까?

`user`는 서드파티 코드에서 가지고 온 객체이므로 함부로 새로운 프로퍼티를 추가할 수 없다. 그런데 심볼은 서드파티 코드에서 접근할 수 없기 때문에, 심볼을 사용하면 서드파티 코드가 모르게 `user`에 식별자를 부여할 수 있다.

상황 하나를 더 가정해보자. 제3의 스크립트(자바스크립트 라이브러리 등)에서 `user`를 식별해야 하는 상황이 벌어졌다고 해보면, `user`의 원천인 서드파티 코드, 현재 작성 중인 스크립트, 제3의 스크립트가 각자 서로의 코드도 모른 채 `user`를 식별해야 하는 상황이 벌어졌다.

제3의 스크립트에선 아래와 같이 `Symbol("id")`을 이용해 전용 식별자를 만들어 사용할 수 있다.

```javascript
// ...
let id = Symbol("id");

user[id] = "제3 스크립트 id 값";
```

심볼은 유일성이 보장되므로 우리가 만든 식별자와 제3의 스크립트에서 만든 식별자가 충돌하지 않는다. 이름이 같더라도 말이다.

만약 심볼 대신 문자열 `"id"`를 사용해 식별자를 만들었다면 충돌이 발생할 *가능성이* 있다.

```javascript
let user = { name: "John" };

// 문자열 "id"를 사용해 식별자를 만들었다.
user.id = "스크립트 id 값";

// 만약 제3의 스크립트가 우리 스크립트와 동일하게 문자열 "id"를 이용해 식별자를 만들었다면...

user.id = "제3 스크립트 id 값"
// 의도치 않게 값이 덮어 쓰여서 우리가 만든 식별자는 무의미해진다.
```





#### Symbols in a literal

객체 리터럴 `{...}`을 사용해 객체를 만든 경우, 대괄호를 사용해 심볼형 키를 만들어야 한다.

```javascript
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123 // "id": 123은 안됨
};
```

`"id: 123"`이라고 하면, 심볼 `id`가 아니라 문자열 "id"가 키가 된다.





#### 심볼은 for…in 에서 배제된다

키가 심볼인 프로퍼티는 `for..in` 반복문에서 배제된다.

```javascript
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name과 age만 출력되고, 심볼은 출력되지 않는다.

// 심볼로 직접 접근하면 잘 작동한다.
alert( "직접 접근한 값: " + user[id] );
```

`Object.keys(user)`에서도 키가 심볼인 프로퍼티는 배제된다. '심볼형 프로퍼티 숨기기(hiding symbolic property)'라 불리는 이런 원칙 덕분에 외부 스크립트나 라이브러리는 심볼형 키를 가진 프로퍼티에 접근하지 못한다.

그런데 [Object.assign](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)은 키가 심볼인 프로퍼티를 배제하지 않고 객체 내 모든 프로퍼티를 복사한다.

```javascript
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);

alert( clone[id] ); // 123
```

뭔가 모순이 있는 것 같아 보이지만, 이는 의도적으로 설계된 것이다. 객체를 복사하거나 병합할 때, 대개 `id` 같은 심볼을 포함한 프로퍼티 *전부*를 사용하고 싶어 할 것이라는 생각에서 이렇게 설계되었다.





#### 전역 심볼

앞서 살펴본 것처럼, 심볼은 이름이 같더라도 모두 별개로 취급된다. 그런데 이름이 같은 심볼이 같은 개체를 가리키길 원하는 경우도 가끔 있다. 애플리케이션 곳곳에서 심볼 `"id"`를 이용해 특정 프로퍼티에 접근해야 한다고 가정해 보자.

*전역 심볼 레지스트리(global symbol registry)* 는 이런 경우를 위해 만들어졌다. 전역 심볼 레지스트리 안에 심볼을 만들고 해당 심볼에 접근하면, 이름이 같은 경우 항상 동일한 심볼을 반환해준다.

레지스트리 안에 있는 심볼을 읽거나, 새로운 심볼을 생성하려면 `Symbol.for(key)`를 사용하면 된다.

이 메서드를 호출하면 이름이 `key`인 심볼을 반환한다. 조건에 맞는 심볼이 레지스트리 안에 없으면 새로운 심볼 `Symbol(key)`을 만들고 레지스트리 안에 저장한다.

```javascript
// 전역 레지스트리에서 심볼을 읽는다.
let id = Symbol.for("id"); // 심볼이 존재하지 않으면 새로운 심볼을 만든다.

// 동일한 이름을 이용해 심볼을 다시 읽는다(좀 더 멀리 떨어진 코드에서도 가능하다).
let idAgain = Symbol.for("id");

// 두 심볼은 같습니다.
alert( id === idAgain ); // true
```

전역 심볼 레지스트리 안에 있는 심볼은 *전역 심볼*이라고 불린다. 애플리케이션에서 광범위하게 사용해야 하는 심볼이라면 전역 심볼을 사용하자.

자바스크립트에선 전역 심볼에만 이런 특징이 적용된다.





#### Symbol.keyFor

전역 심볼을 찾을 때 사용되는 `Symbol.for(key)`에 반대되는 메서드도 있다. `Symbol.keyFor(sym)`를 사용하면 이름을 얻을 수 있다.

```javascript
// 이름을 이용해 심볼을 찾음
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

// 심볼을 이용해 이름을 얻음
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

`Symbol.keyFor`는 전역 심볼 레지스트리를 뒤져서 해당 심볼의 이름을 얻어낸다. 검색 범위가 전역 심볼 레지스트리이기 때문에 전역 심볼이 아닌 심볼에는 사용할 수 없다. 전역 심볼이 아닌 인자가 넘어오면 `Symbol.keyFor`는 `undefined`를 반환한다.



전역 심볼이 아닌 모든 심볼은 `description` 프로퍼티가 있다. 일반 심볼에서 이름을 얻고 싶으면 `description` 프로퍼티를 사용하면 된다.

```javascript
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, 전역 심볼
alert( Symbol.keyFor(localSymbol) ); // undefined, 전역 심볼이 아님

alert( localSymbol.description ); // name
```





#### 시스템 심볼

'시스템 심볼(system symbol)'은 자바스크립트 내부에서 사용되는 심볼이다. 시스템 심볼을 활용하면 객체를 미세 조정할 수 있다.

명세서 내의 표, [잘 알려진 심볼(well-known symbols)](https://tc39.github.io/ecma262/#sec-well-known-symbols)에서 어떤 시스템 심볼이 있는지 살펴보자.

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- 기타 등등





>#### 요약
>
>`Symbol`은 원시형 데이터로, 유일무이한 식별자를 만드는 데 사용된다.
>
>`Symbol()`을 호출하면 심볼을 만들 수 있다. 설명(이름)은 선택적으로 추가할 수 있다.
>
>심볼은 이름이 같더라도 값이 항상 다르다. 이름이 같을 때 값도 같길 원한다면 전역 레지스트리를 사용해야 한다. `Symbol.for(key)`는 `key`라는 이름을 가진 전역 심볼을 반환한다. `key`라는 이름을 가진 전역 심볼이 없으면 새로운 전역 심볼을 만들어준다. `key`가 같다면 `Symbol.for`는 어디서 호출하든 상관없이 항상 같은 심볼을 반환해 준다.
>
>
>
>심볼의 주요 유스 케이스는 다음과 같다.
>
>1. 객체의 ‘숨김’ 프로퍼티 – 외부 스크립트나 라이브러리에 ‘속한’ 객체에 새로운 프로퍼티를 추가해 주고 싶다면 심볼을 만들고, 이를 프로퍼티 키로 사용하면 된다. 키가 심볼인 경우엔 `for..in`의 대상이 되지 않아서 의도치 않게 프로퍼티가 수정되는 것을 예방할 수 있다. 외부 스크립트나 라이브러리는 심볼 정보를 갖고 있지 않아서 프로퍼티에 직접 접근하는 것도 불가능하다. 심볼형 키를 사용하면 프로퍼티가 우연히라도 사용되거나 덮어씌워 지는 걸 예방할 수 있다. 이런 특징을 이용하면 원하는 것을 객체 안에 ‘은밀하게’ 숨길 수 있다. 그렇게 되면, 외부 스크립트에선 우리가 숨긴 것을 절대 볼 수 없다.
>
>
>
>2. 자바스크립트 내부에서 사용되는 시스템 심볼은 `Symbol.*`로 접근할 수 있다. 시스템 심볼을 이용하면 내장 메서드 등의 기본 동작을 입맛대로 변경할 수 있다.
>
>
>
>사실 심볼을 완전히 숨길 방법은 없다. 
>
>내장 메서드 [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)를 사용하면 모든 심볼을 볼 수 있고, 
>
>메서드 [Reflect.ownKeys(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)는 심볼형 키를 포함한 객체의 *모든* 키를 반환해준다. 
>
>그런데 대부분의 라이브러리, 내장 함수 등은 이런 메서드를 사용하지 않는다.











## 4.8 객체를 원시형으로 변환하기

`obj1 + obj2` 처럼 객체끼리 더하는 연산을 하거나, `obj1 - obj2` 처럼 객체끼리 빼는 연산을 하면 어떤 일이 일어날까? 또한, `alert(obj)`로 객체를 출력할 때는 무슨 일이 발생할까?

이 모든 경우에 **자동 형 변환이 일어난다.** 객체는 원시값으로 변환되고, 그 후 의도한 연산이 수행된다.

1. 단 하나의 예외도 없이 객체는 논리 평가 시 `true`를 반환한다. 따라서 **객체는 숫자형이나 문자형으로만 형 변환이 일어난다**고 생각하면 된다.
2. 숫자형으로의 형 변환은 객체끼리 빼는 연산을 할 때나 수학 관련 함수를 적용할 때 일어난다. 객체 `Date`끼리 차감하면(`date1 - date2`) 두 날짜의 시간 차이가 반환된다. 
3. 문자형으로의 형 변환은 대개 `alert(obj)`같이 객체를 출력하려고 할 때 일어난다.





#### ToPrimitive

특수 객체 메서드를 사용하면 숫자형이나 문자형으로의 형 변환을 원하는 대로 조절할 수 있다.

객체 형 변환은 세 종류로 구분되는데, 'hint’라 불리는 값이 구분 기준이 된다. 'hint’가 무엇인지는 [명세서](https://tc39.github.io/ecma262/#sec-toprimitive)에 자세히 설명되어 있는데, ‘목표로 하는 자료형’ 정도로 이해하시면 될 것 같다.

- `"string"`

  `alert` 함수같이 문자열을 기대하는 연산을 수행할 때는(객체-문자형 변환), hint가 `string`이 된다.`// 객체를 출력하려고 함 alert(obj); // 객체를 프로퍼티 키로 사용하고 있음 anotherObj[obj] = 123;`

- `"number"`

  수학 연산을 적용하려 할 때(객체-숫자형 변환), hint는 `number`가 된다.`// 명시적 형 변환 let num = Number(obj); // (이항 덧셈 연산을 제외한) 수학 연산 let n = +obj; // 단항 덧셈 연산 let delta = date1 - date2; // 크고 작음 비교하기 let greater = user1 > user2;`

- `"default"`

  연산자가 기대하는 자료형이 ‘확실치 않을 때’ hint는 `default`가 된다. 아주 드물게 발생한다.이항 덧셈 연산자 `+`는 피연산자의 자료형에 따라 문자열을 합치는 연산을 할 수도 있고 숫자를 더해주는 연산을 할 수도 있다. 따라서 `+`의 인수가 객체일는 hint가 `default`가 된다.동등 연산자 `==`를 사용해 객체-문자형, 객체-숫자형, 객체-심볼형끼리 비교할 때도, 객체를 어떤 자료형으로 바꿔야 할지 확신이 안 서므로 hint는 default가 된다.`// 이항 덧셈 연산은 hint로 `default`를 사용합니다. let total = obj1 + obj2; // obj == number 연산은 hint로 `default`를 사용한다. if (user == 1) { ... };`크고 작음을 비교할 때 쓰이는 연산자 `<`, `>` 역시 피연산자에 문자형과 숫자형 둘 다를 허용하는데, 이 연산자들은 hint를 'number’로 고정한다. hint가 'default’가 되는 일이 없다. 이는 하위 호환성 때문에 정해진 규칙이다.실제 일을 할 때는 이런 사항을 모두 외울 필요는 없다. `Date` 객체를 제외한 모든 내장 객체는 hint가 `"default"`인 경우와 `"number"`인 경우를 동일하게 처리하기 때문이다. 우리도 커스텀 객체를 만들 땐 이런 규칙을 따르면 된다.



**`"boolean"` hint는 없다.**

hint는 총 세 가지이다. 아주 간단하다.

‘boolean’ hint는 존재하지 않는다. 모든 객체는 그냥 `true`로 평가된다. 게다가 우리도 내장 객체에 사용되는 규칙처럼 `"default"`와 `"number"`를 동일하게 처리하면, 결국엔 두 종류의 형 변환(객체-문자형, 객체-숫자형)만 남게 된다.

**자바스크립트는 형 변환이 필요할 때, 아래와 같은 알고리즘에 따라 원하는 메서드를 찾고 호출한다.**

1. 객체에 `obj[Symbol.toPrimitive](hint)`메서드가 있는지 찾고, 있다면 메서드를 호출한다. `Symbol.toPrimitive`는 시스템 심볼로, 심볼형 키로 사용된다.

2. 1에 해당하지 않고 hint가

   ```javascript
   "string"
   ```

   이라면,

   - `obj.toString()`이나 `obj.valueOf()`를 호출한다(존재하는 메서드만 실행됨).

3. 1과 2에 해당하지 않고, hint가

   ```javascript
   "number"
   ```

   나 

   ```javascript
   "default"
   ```

   라면

   - `obj.valueOf()`나 `obj.toString()`을 호출합니다(존재하는 메서드만 실행됨).





#### Symbol.toPrimitive

첫 번째 메서드부터 살펴보자. 자바스크립트엔 `Symbol.toPrimitive`라는 내장 심볼이 존재하는데, 이 심볼은 아래와 같이 목표로 하는 자료형(hint)을 명명하는 데 사용된다.

```javascript
obj[Symbol.toPrimitive] = function(hint) {
  // 반드시 원시값을 반환해야 한다.
  // hint는 "string", "number", "default" 중 하나가 될 수 있다.
};
```

실제 돌아가는 예시를 살펴보는 게 좋을 것 같다. `user` 객체에 객체-원시형 변환 메서드 `obj[Symbol.toPrimitive](hint)`를 구현해보자.

```javascript
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

// 데모:
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

이렇게 메서드를 구현해 놓으면 `user`는 hint에 따라 (자기 자신을 설명해주는) 문자열로 변환되기도 하고 (가지고 있는 돈의 액수를 나타내는) 숫자로 변환되기도 한다. `user[Symbol.toPrimitive]`를 사용하면 메서드 하나로 모든 종류의 형 변환을 다룰 수 있다.





#### toString과 valueOf

`toString`과 `valueOf`는 심볼이 생기기 이전부터 존재해 왔던 ‘평범한’ 메서드이다. 이 메서드를 이용하면 '구식’이긴 하지만 형 변환을 직접 구현할 수 있다.

객체에 `Symbol.toPrimitive`가 없으면 자바스크립트는 아래 규칙에 따라 `toString`이나 `valueOf`를 호출한다.

- hint가 'string’인 경우: `toString -> valueOf` 순(`toString`이 있다면 `toString`을 호출, `toString`이 없다면 `valueOf`를 호출함)
- 그 외: `valueOf -> toString` 순

이 메서드들은 반드시 원시값을 반환해야한다. `toString`이나 `valueOf`가 객체를 반환하면 그 결과는 무시된다. 마치 메서드가 처음부터 없었던 것처럼 되어버린다.

일반 객체는 기본적으로 `toString`과 `valueOf`에 적용되는 다음 규칙을 따른다.

- `toString`은 문자열 `"[object Object]"`을 반환한다.
- `valueOf`는 객체 자신을 반환한다.



```javascript
let user = {name: "John"};

alert(user); // [object Object]
alert(user.valueOf() === user); // true
```

이런 이유 때문에 `alert`에 객체를 넘기면 `[object Object]`가 출력되는 것이다.

여기서 `valueOf`는 튜토리얼의 완성도를 높이고 헷갈리는 것을 줄여주려고 언급했다. 앞서 본 바와 같이 `valueOf`는 객체 자신을 반환하기 때문에 그 결과가 무시된다. 그저 역사적인 이유때문이다. 우리는 그냥 이 메서드가 존재하지 않는다고 생각하면 된다.



아래 `user`는 `toString`과 `valueOf`를 조합해 만들었는데, `Symbol.toPrimitive`를 사용한 위쪽 예시와 동일하게 동작한다.

```javascript
let user = {
  name: "John",
  money: 1000,

  // hint가 "string"인 경우
  toString() {
    return `{name: "${this.name}"}`;
  },

  // hint가 "number"나 "default"인 경우
  valueOf() {
    return this.money;
  }

};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
alert(user + 500); // valueOf -> 1500
```

출력 결과가 `Symbol.toPrimitive`를 사용한 예제와 완전히 동일하다는 걸 확인할 수 있다.

그런데 간혹 모든 형 변환을 한 곳에서 처리해야 하는 경우도 생깁니다. 이럴 땐 아래와 같이 `toString`만 구현해 주면 된다.

```javascript
let user = {
  name: "John",

  toString() {
    return this.name;
  }
};

alert(user); // toString -> John
alert(user + 500); // toString -> John500
```

객체에 `Symbol.toPrimitive`와 `valueOf`가 없으면, `toString`이 모든 형 변환을 처리한다.





#### 반환 타입

위에서 소개해드린 세 개의 메서드는 'hint’에 명시된 자료형으로의 형 변환을 보장해 주지 않는다.

`toString()`이 항상 문자열을 반환하리라는 보장이 없고, `Symbol.toPrimitive`의 hint가 `"number"`일 때 항상 숫자형 자료가 반환되리라는 보장이 없다.

확신할 수 있는 단 한 가지는 객체가 아닌 원시값을 반환해 준다는 것뿐이다.

**과거의 잔재**

`toString`이나 `valueOf`가 객체를 반환해도 에러가 발생하지 않는다. 다만 이때는 반환 값이 무시되고, 메서드 자체가 존재하지 않았던 것처럼 동작한다. 이렇게 동작하는 이유는 과거 자바스크립트엔 '에러’라는 개념이 잘 정립되어있지 않았기 때문이다.

반면에 `Symbol.toPrimitive`는 *무조건* 원시자료를 반환해야 한다. 그렇지 않으면 에러가 발생한다.





#### 추가 형 변환

지금까지 살펴본 바와 같이 상당수의 연산자와 함수가 피연산자의 형을 변환시킨다. 곱셈을 해주는 연산자 `*`는 피연산자를 숫자형으로 변환시킨다.

객체가 피연산자일 때는 다음과 같은 단계를 거쳐 형 변환이 일어난다.

1. 객체는 원시형으로 변화된다. 변환 규칙은 위에서 설명했다.
2. 변환 후 원시값이 원하는 형이 아닌 경우엔 또다시 형 변환이 일어난다.



```javascript
let obj = {
  // 다른 메서드가 없으면 toString에서 모든 형 변환을 처리한다.
  toString() {
    return "2";
  }
};

alert(obj * 2); // 4, 객체가 문자열 "2"로 바뀌고, 곱셈 연산 과정에서 문자열 "2"는 숫자 2로 변경된다.
```

1. `obj * 2`에선 객체가 원시형으로 변화되므로 `toString`에의해 `obj`는 문자열 `"2"`가 된다.
2. 곱셈 연산은 문자열은 숫자형으로 변환시키므로 `"2" * 2`는 `2 * 2`가 된다.

그런데 이항 덧셈 연산은 위와 같은 상황에서 문자열을 연결한다.

```javascript
let obj = {
  toString() {
    return "2";
  }
};

alert(obj + 2); // 22("2" + 2), 문자열이 반환되기 때문에 문자열끼리의 병합이 일어났다.
```





> #### 요약
>
> 원시값을 기대하는 내장 함수나 연산자를 사용할 때 객체-원시형으로의 형 변환이 자동으로 일어난다.
>
> 객체-원시형으로의 형 변환은 hint를 기준으로 세 종류로 구분할 수 있다.
>
> - `"string"` (`alert` 같이 문자열을 필요로 하는 연산)
> - `"number"` (수학 연산)
> - `"default"` (드물게 발생함)
>
> 연산자별로 어떤 hint가 적용되는지는 명세서에서 찾아볼 수 있다. 연산자가 기대하는 피연산자를 '확신할 수 없을 때’에는 hint가 `"default"`가 된다. 이런 경우는 아주 드물게 발생한다. 내장 객체는 대개 hint가 `"default"`일 때와 `"number"`일 때를 동일하게 처리한다. 따라서 실무에선 hint가 `"default"`인 경우와 `"number"`인 경우를 합쳐서 처리하는 경우가 많다.
>
> 객체-원시형 변환엔 다음 알고리즘이 적용된다.
>
> 1. 객체에 `obj[Symbol.toPrimitive](hint)`메서드가 있는지 찾고, 있다면 호출한다.
>
> 2. 1에 해당하지 않고 hint가
>
>    ```javascript
>    "string"
>    ```
>
>    이라면, `obj.toString()`이나 `obj.valueOf()`를 호출한다.
>
> 3. 1과 2에 해당하지 않고, hint가
>
>    ```javascript
>    "number"
>    ```
>
>    나
>
>    ```javascript
>    "default"
>    ```
>
>    라면`obj.valueOf()`나 `obj.toString()`을 호출한다.
>
> `obj.toString()`만 사용해도 '모든 변환’을 다 다룰 수 있기 때문에, 실무에선 `obj.toString()`만 구현해도 충분한 경우가 많다. 반환 값도 ‘사람이 읽고 이해할 수 있는’ 형식이기 때문에 실용성 측면에서 다른 메서드에 뒤처지지 않는다. `obj.toString()`은 로깅이나 디버깅 목적으로도 자주 사용한다.

