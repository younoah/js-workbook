### 객체: 기본

#### 4.1 객체

자바스크립트엔 여덟 가지 자료형(<u>숫자형, bigint형, 문자형, 불린형, 객체형, Null형, Undefined형, 심볼형</u>)이 있다. **이 중 일곱 개는 오직 하나의 데이터(문자열, 숫자 등)만 담을 수 있어 '원시형(primitive type)'이라 부른다.**

그런데 객체형은 원시형과 달리 다양한 데이터를 담을 수 있다. 키로 구분된 데이터 집합이나 복잡한 개체(entity)를 저장할 수 있다. 객체는 자바스크립트 거의 모든 면에 녹아있는 개념이므로 자바스크립트를 잘 다루려면 객체를 잘 이해하고 있어야 한다.

객체는 중괄호 `{…}`를 이용해 만들 수 있다. 중괄호 안에는 ‘키(key): 값(value)’ 쌍으로 구성된 *프로퍼티(property)* 를 여러 개 넣을 수 있는데, `키`엔 문자형, `값`엔 모든 자료형이 허용된다. 프로퍼티 키는 ‘프로퍼티 이름’ 이라고도 부른다.

서랍장을 상상하면 객체를 이해하기 쉽다. 서랍장 안 파일은 프로퍼티, 파일 각각에 붙어있는 이름표는 객체의 키라고 생각하시면 된다. 복잡한 서랍장 안에서 이름표를 보고 원하는 파일을 쉽게 찾을 수 있듯이, 객체에선 키를 이용해 프로퍼티를 쉽게 찾을 수 있다. 추가나 삭제도 마찬가지다.

![스크린샷 2021-04-25 오후 8 08 58](https://user-images.githubusercontent.com/79819941/115991150-2f618e00-a602-11eb-8ba1-4712633e6e4a.png)

빈 객체(빈 서랍장)를 만드는 방법은 두 가지가 있다.

```javascript
let user = new Object(); // '객체 생성자' 문법
let user = {};  // '객체 리터럴' 문법
```

![스크린샷 2021-04-25 오후 8 09 03](https://user-images.githubusercontent.com/79819941/115991173-54560100-a602-11eb-996e-1155b595cf88.png) 

**중괄호 `{...}`를 이용해 객체를 선언하는 것을 *객체 리터럴(object literal)* 이라고 부른다.** 객체를 선언할 땐 주로 이 방법을 사용한다.

## [리터럴과 프로퍼티](https://ko.javascript.info/object#ref-1236)

중괄호 `{...}` 안에는 ‘키: 값’ 쌍으로 구성된 프로퍼티가 들어간다.

```javascript
let user = {     // 객체
  name: "John",  // 키: "name",  값: "John"
  age: 30        // 키: "age", 값: 30
};
```

`'콜론(:)'`을 기준으로 왼쪽엔 키가, 오른쪽엔 값이 위치한다. 프로퍼티 키는 프로퍼티 ‘이름’ 혹은 '식별자’라고도 부른다.

객체 `user`에는 프로퍼티가 두 개 있다.

1. 첫 번째 프로퍼티 – `"name"`(이름)과 `"John"`(값)
2. 두 번째 프로퍼티 – `"age"`(이름)과 `30`(값)

서랍장(객체 `user`) 안에 파일 두 개(프로퍼티 두 개)가 담겨있는데, 각 파일에 “name”, "age"라는 이름표가 붙어있다고 생각하면 쉽다.

![스크린샷 2021-04-25 오후 8 09 08](https://user-images.githubusercontent.com/79819941/115991249-a434c800-a602-11eb-8459-776a6a7bdd19.png) 

서랍장에 파일을 추가하고 뺄 수 있듯이 개발자는 프로퍼티를 추가, 삭제할 수 있다.

점 표기법(dot notation)을 이용하면 프로퍼티 값을 읽는 것도 가능하다.

```javascript
// 프로퍼티 값 얻기
alert( user.name ); // John
alert( user.age ); // 30
```

프로퍼티 값엔 모든 자료형이 올 수 있습니다. 불린형 프로퍼티를 추가해보자.

```javascript
user.isAdmin = true;
```

![스크린샷 2021-04-25 오후 8 09 14](https://user-images.githubusercontent.com/79819941/115991308-c3335a00-a602-11eb-91f8-5f2248b59578.png) 

`delete` 연산자를 사용하면 프로퍼티를 삭제할 수 있다.

```javascript
delete user.age;
```

![스크린샷 2021-04-25 오후 8 15 37](https://user-images.githubusercontent.com/79819941/115991381-068dc880-a603-11eb-985f-9306e29a7d2a.png) 

여러 단어를 조합해 프로퍼티 이름을 만든 경우엔 프로퍼티 이름을 따옴표로 묶어줘야 한다.

```javascript
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // 복수의 단어는 따옴표로 묶어야 한다.
};
```

![스크린샷 2021-04-25 오후 8 09 24](https://user-images.githubusercontent.com/79819941/115991393-1efde300-a603-11eb-8c8a-daaee0473331.png) 

마지막 프로퍼티 끝은 쉼표로 끝날 수 있습니다.

```javascript
let user = {
  name: "John",
  age: 30,
}
```

이런 쉼표를 ‘trailing(길게 늘어지는)’ 혹은 ‘hanging(매달리는)’ 쉼표라고 부른다. 이렇게 끝에 쉼표를 붙이면 모든 프로퍼티가 유사한 형태를 보이기 때문에 프로퍼티를 추가, 삭제, 이동하는 게 쉬워진다.

**상수 객체는 수정될 수 있다.**

주의하세요. `const`로 선언된 객체는 수정될 수 있다.

```javascript
const user = {
  name: "John"
};

user.name = "Pete"; // (*)

alert(user.name); // Pete
```

`(*)`로 표시한 줄에서 오류를 일으키는 것처럼 보일 수 있지만 그렇지 않다. `const`는 `user`의 값을 고정하지만, 그 내용은 고정하지 않는다.

`const`는 `user=...`를 전체적으로 설정하려고 할 때만 오류가 발생한다.





## [대괄호 표기법](https://ko.javascript.info/object#ref-1237)

여러 단어를 조합해 프로퍼티 키를 만든 경우엔, 점 표기법을 사용해 프로퍼티 값을 읽을 수 없다.

```javascript
// 문법 에러가 발생한다.
user.likes birds = true
```

자바스크립트는 위와 같은 코드를 이해하지 못한다. `user.likes`까지는 이해하다가 예상치 못한 `birds`를 만나면 문법 에러를 뱉어낸다.

'점’은 키가 '유효한 변수 식별자’인 경우에만 사용할 수 있다. **유효한 변수 식별자엔 공백이 없어야 하고 또한 숫자로 시작하지 않아야 하며 `$`와 `_`를 제외한 특수 문자가 없어야 한다.**

키가 유효한 변수 식별자가 아닌 경우엔 점 표기법 대신에 '대괄호 표기법(square bracket notation)'이라 불리는 방법을 사용할 수 있습니다. 대괄호 표기법은 키에 어떤 문자열이 있던지 상관없이 동작한다.

```javascript
let user = {};

// set
user["likes birds"] = true;

// get
alert(user["likes birds"]); // true

// delete
delete user["likes birds"];
```

이제 문법 에러가 발생하지 않네요. 대괄호 표기법 안에서 문자열을 사용할 땐 문자열을 따옴표로 묶어줘야 한다는 점에 주의하시기 바랍니다. 따옴표의 종류는 상관없다.

대괄호 표기법을 사용하면 아래 예시에서 변수를 키로 사용한 것과 같이 문자열뿐만 아니라 모든 표현식의 평가 결과를 프로퍼티 키로 사용할 수 있다.

```javascript
let key = "likes birds";

// user["likes birds"] = true; 와 같다.
user[key] = true;
```

변수 `key`는 런타임에 평가되기 때문에 사용자 입력값 변경 등에 따라 값이 변경될 수 있다. 어떤 경우든, 평가가 끝난 이후의 결과가 프로퍼티 키로 사용된다. 이를 응용하면 코드를 유연하게 작성할 수 있다.

```javascript
let user = {
  name: "John",
  age: 30
};

let key = prompt("사용자의 어떤 정보를 얻고 싶으신가요?", "name");

// 변수로 접근
alert( user[key] ); // John (프롬프트 창에 "name"을 입력한 경우)
```

그런데 점 표기법은 이런 방식이 불가능하다.

```javascript
let user = {
  name: "John",
  age: 30
};

let key = "name";
alert( user.key ) // undefined
```





### [계산된 프로퍼티](https://ko.javascript.info/object#ref-1238)

객체를 만들 때 객체 리터럴 안의 프로퍼티 키가 대괄호로 둘러싸여 있는 경우, 이를 *계산된 프로퍼티(computed property)* 라고 부른다.

```javascript
let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");

let bag = {
  [fruit]: 5, // 변수 fruit에서 프로퍼티 이름을 동적으로 받아 온다.
};

alert( bag.apple ); // fruit에 "apple"이 할당되었다면, 5가 출력된다.
```

위 예시에서 `[fruit]`는 프로퍼티 이름을 변수 `fruit`에서 가져오겠다는 것을 의미한다.

사용자가 프롬프트 대화상자에 `apple`을 입력했다면 `bag`엔 `{apple: 5}`가 할당되었을 것이다.

아래 예시는 위 예시와 동일하게 동작한다.

```javascript
let fruit = prompt("어떤 과일을 구매하시겠습니까?", "apple");
let bag = {};

// 변수 fruit을 사용해 프로퍼티 이름을 만들었다.
bag[fruit] = 5;
```

두 방식 중 계산된 프로퍼티를 사용한 예시가 더 깔끔해 보인다.

한편, 다음 예시처럼 대괄호 안에는 복잡한 표현식이 올 수도 있다.

```javascript
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};
```

대괄호 표기법은 프로퍼티 이름과 값의 제약을 없애주기 때문에 점 표기법보다 훨씬 강력하다. 그런데 작성하기 번거롭다는 단점이 있다.

이런 이유로 프로퍼티 이름이 확정된 상황이고, 단순한 이름이라면 처음엔 점 표기법을 사용하다가 뭔가 복잡한 상황이 발생했을 때 대괄호 표기법으로 바꾸는 경우가 많다.





## [단축 프로퍼티](https://ko.javascript.info/object#ref-1239)

실무에선 프로퍼티 값을 기존 변수에서 받아와 사용하는 경우가 종종 있다.

```javascript
function makeUser(name, age) {
  return {
    name: name,
    age: age,
    // ...등등
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

위 예시의 프로퍼티들은 이름과 값이 변수의 이름과 동일하네요. 이렇게 변수를 사용해 프로퍼티를 만드는 경우는 아주 흔한데, *프로퍼티 값 단축 구문(property value shorthand)* 을 사용하면 코드를 짧게 줄일 수 있다.

`name:name` 대신 `name`만 적어주어도 프로퍼티를 설정할 수 있다.

```javascript
function makeUser(name, age) {
  return {
    name, // name: name 과 같음
    age,  // age: age 와 같음
    // ...
  };
}
```

한 객체에서 일반 프로퍼티와 단축 프로퍼티를 함께 사용하는 것도 가능하다.

```javascript
let user = {
  name,  // name: name 과 같음
  age: 30
};
```





## [프로퍼티 이름의 제약사항](https://ko.javascript.info/object#ref-1240)

아시다시피 변수 이름(키)엔 ‘for’, ‘let’, ‘return’ 같은 예약어를 사용하면 안된다.

그런데 객체 프로퍼티엔 이런 제약이 없다.

```javascript
// 예약어를 키로 사용해도 괜찮다.
let obj = {
  for: 1,
  let: 2,
  return: 3
};

alert( obj.for + obj.let + obj.return );  // 6
```

이와 같이 프로퍼티 이름엔 특별한 제약이 없다. 어떤 문자형, 심볼형 값도 프로퍼티 키가 될 수 있다.

문자형이나 심볼형에 속하지 않은 값은 문자열로 자동 형 변환된다.

예시를 살펴봅시다. 키에 숫자 `0`을 넣으면 문자열 `"0"`으로 자동변환된다.

```javascript
let obj = {
  0: "test" // "0": "test"와 동일하다.
};

// 숫자 0은 문자열 "0"으로 변환되기 때문에 두 얼럿 창은 같은 프로퍼티에 접근한다,
alert( obj["0"] ); // test
alert( obj[0] ); // test (동일한 프로퍼티)
```

이와같이 객체 프로퍼티 키에 쓸 수 있는 문자열엔 제약이 없지만, 역사적인 이유 때문에 특별 대우를 받는 이름이 하나 있다. 바로, `__proto__`다.

```javascript
let obj = {};
obj.__proto__ = 5; // 숫자를 할당합니다.
alert(obj.__proto__); // [object Object] - 숫자를 할당했지만 값은 객체가 되었습니다. 의도한대로 동작하지 않네요.
```

원시값 `5`를 할당했는데 무시된 것을 확인할 수 있다.

`__proto__`의 본질은 [프로토타입 상속](https://ko.javascript.info/prototype-inheritance)에서, 이 문제를 어떻게 해결할 수 있을지에 대해선 [프로토타입 메서드와 __proto__가 없는 객체](https://ko.javascript.info/prototype-methods)에서 자세히 다룰 예정이다.





## [‘in’ 연산자로 프로퍼티 존재 여부 확인하기](https://ko.javascript.info/object#ref-1241)

자바스크립트 객체의 중요한 특징 중 하나는 다른 언어와는 달리, 존재하지 않는 프로퍼티에 접근하려 해도 에러가 발생하지 않고 `undefined`를 반환한다는 것이다.

이런 특징을 응용하면 프로퍼티 존재 여부를 쉽게 확인할 수 있다.

```javascript
let user = {};

alert( user.noSuchProperty === undefined ); // true는 '프로퍼티가 존재하지 않음'을 의미한다.
```

이렇게 `undefined`와 비교하는 것 이외에도 연산자 `in`을 사용하면 프로퍼티 존재 여부를 확인할 수 있다.

문법은 다음과 같다.

```javascript
"key" in object
```

```javascript
let user = { name: "John", age: 30 };

alert( "age" in user ); // user.age가 존재하므로 true가 출력된다.
alert( "blabla" in user ); // user.blabla는 존재하지 않기 때문에 false가 출력된다.
```

`in` 왼쪽엔 반드시 *프로퍼티 이름*이 와야 합니다. 프로퍼티 이름은 보통 따옴표로 감싼 문자열이다.

따옴표를 생략하면 아래 예시와 같이 엉뚱한 변수가 조사 대상이 된다.

```javascript
let user = { age: 30 };

let key = "age";
alert( key in user ); // true, 변수 key에 저장된 값("age")을 사용해 프로퍼티 존재 여부를 확인한다.
```

그런데 이쯤 되면 "`undefined`랑 비교해도 충분한데 왜 `in` 연산자가 있는 거지?"라는 의문이 들 수 있다.

대부분의 경우, 일치 연산자를 사용해서 프로퍼티 존재 여부를 알아내는 방법(`"=== undefined"`)은 꽤 잘 동작한다. 그런데 가끔은 이 방법이 실패할 때도 있다. 이럴 때 `in`을 사용하면 프로퍼티 존재 여부를 제대로 판별할 수 있다.

프로퍼티는 존재하는데, 값에 `undefined`를 할당한 예시를 살펴보자.

```javascript
let obj = {
  test: undefined
};

alert( obj.test ); // 값이 `undefined`이므로, 얼럿 창엔 undefined가 출력된다. 그런데 프로퍼티 test는 존재한다.

alert( "test" in obj ); // `in`을 사용하면 프로퍼티 유무를 제대로 확인할 수 있다(true가 출력됨).
```

`obj.test`는 실제 존재하는 프로퍼티입니다. 따라서 `in` 연산자는 정상적으로 true를 반환한다.

`undefined`는 변수는 정의되어 있으나 값이 할당되지 않은 경우에 쓰기 때문에 프로퍼티 값이 `undefined`인 경우는 흔치 않다. 값을 ‘알 수 없거나(unknown)’ 값이 ‘비어 있다는(empty)’ 것을 나타낼 때는 주로 `null`을 사용한다. 위 예시에서 `in` 연산자는 자리에 어울리지 않는 것처럼 보인다.





## [‘for…in’ 반복문](https://ko.javascript.info/object#ref-1242)

`for..in` 반복문을 사용하면 객체의 모든 키를 순회할 수 있다. `for..in`은 앞서 학습했던 `for(;;)` 반복문과는 완전히 다르다.

문법:

```javascript
for (key in object) {
  // 각 프로퍼티 키(key)를 이용하여 본문(body)을 실행한다.
}
```

아래 예시를 실행하면 객체 `user`의 모든 프로퍼티가 출력된다.

```javascript
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // 키
  alert( key );  // name, age, isAdmin
  // 키에 해당하는 값
  alert( user[key] ); // John, 30, true
}
```

`for..in` 반복문에서도 `for(;;)`문처럼 반복 변수(looping variable)를 선언(`let key`)했다는 점에 주목하자.

반복 변수명은 자유롭게 정할 수 있는데,  `'for (let prop in obj)'`같이 `key` 말고 다른 변수명을 사용해도 괜찮다.





### [객체 정렬 방식](https://ko.javascript.info/object#ref-1243)

객체와 객체 프로퍼티를 다루다 보면 "프로퍼티엔 순서가 있을까?"라는 의문이 생기기 마련인데 반복문은 프로퍼티를 추가한 순서대로 실행될지, 그리고 이 순서는 항상 동일할지 궁금해진다.

답은 간단하다. 객체는 '특별한 방식으로 정렬’된다. 정수 프로퍼티(integer property)는 자동으로 정렬되고, 그 외의 프로퍼티

아래 객체엔 국제전화 나라 번호가 담겨있다.

```javascript
let codes = {
  "49": "독일",
  "41": "스위스",
  "44": "영국",
  // ..,
  "1": "미국"
};

for (let code in codes) {
  alert(code); // 1, 41, 44, 49
}
```

현재 개발 중인 애플리케이션의 주 사용자가 독일인이라고 가정해 보자. 나라 번호를 선택하는 화면에서 `49`가 맨 앞에 오도록 하는 게 좋을 것이다.

그런데 코드를 실행해 보면 예상과는 전혀 다른 결과가 출력된다.

- 미국(1)이 첫 번째로 출력된다.
- 그 뒤로 스위스(41), 영국(44), 독일(49)이 차례대로 출력된다.

**이유는 나라 번호(키)가 정수이어서 `1, 41, 44, 49` 순으로 프로퍼티가 자동 정렬되었기 때문이다.**

**정수 프로퍼티? 그게 뭔지?**

**'정수 프로퍼티’라는 용어는 변형 없이 정수에서 왔다 갔다 할 수 있는 문자열을 의미한다.**

문자열 "49"는 정수로 변환하거나 변환한 정수를 다시 문자열로 바꿔도 변형이 없기 때문에 정수 프로퍼티다. 하지만 '+49’와 '1.2’는 정수 프로퍼티가 아니다.

```javascript
// 함수 Math.trunc는 소수점 아래를 버리고 숫자의 정수부만 반환한다.
alert( String(Math.trunc(Number("49"))) ); // '49'가 출력된다. 기존에 입력한 값과 같으므로 정수 프로퍼티다.
alert( String(Math.trunc(Number("+49"))) ); // '49'가 출력된다. 기존에 입력한 값(+49)과 다르므로 정수 프로퍼티가 아니다.
alert( String(Math.trunc(Number("1.2"))) ); // '1'이 출력됩니다. 기존에 입력한 값(1.2)과 다르므로 정수 프로퍼티가 아니다.
```

한편, 키가 정수가 아닌 경우엔 작성된 순서대로 프로퍼티가 나열된다. 예시를 살펴보자.

```javascript
let user = {
  name: "John",
  surname: "Smith"
};
user.age = 25; // 프로퍼티를 하나 추가한다.

// 정수 프로퍼티가 아닌 프로퍼티는 추가된 순서대로 나열된다.
for (let prop in user) {
  alert( prop ); // name, surname, age
}
```

위 예시에서 49(독일 나라 번호)를 가장 위에 출력되도록 하려면 나라 번호가 정수로 취급되지 않도록 속임수를 쓰면 된다. 각 나라 번호 앞에 `"+"`를 붙여보자.

아래와 같다.

```javascript
let codes = {
  "+49": "독일",
  "+41": "스위스",
  "+44": "영국",
  // ..,
  "+1": "미국"
};

for (let code in codes) {
  alert( +code ); // 49, 41, 44, 1
}
```

이제 원하는 대로 독일 나라 번호가 가장 먼저 출력되는 것을 확인할 수 있다.



> ## [요약](https://ko.javascript.info/object#ref-1244)
>
> 객체는 몇 가지 특수한 기능을 가진 연관 배열(associative array)이다.
>
> 객체는 프로퍼티(키-값 쌍)를 저장한다.
>
> - 프로퍼티 키는 문자열이나 심볼이어야 한다. 보통은 문자열이다.
> - 값은 어떤 자료형도 가능하다.
>
> 아래와 같은 방법을 사용하면 프로퍼티에 접근할 수 있다.
>
> - 점 표기법: `obj.property`
> - 대괄호 표기법 `obj["property"]`. 대괄호 표기법을 사용하면 `obj[varWithKey]`같이 변수에서 키를 가져올 수 있다.
>
> 객체엔 다음과 같은 추가 연산자를 사용할 수 있다.
>
> - 프로퍼티를 삭제하고 싶을 때: `delete obj.prop`
> - 해당 key를 가진 프로퍼티가 객체 내에 있는지 확인하고자 할 때: `"key" in obj`
> - 프로퍼티를 나열할 때: `for (let key in obj)`
>
> 지금까진 '순수 객체(plain object)'라 불리는 일반 `객체`에 대해 학습했다.
>
> 자바스크립트에는 일반 객체 이외에도 다양한 종류의 객체가 있다.
>
> - `Array` – 정렬된 데이터 컬렉션을 저장할 때 쓰임
> - `Date` – 날짜와 시간 정보를 저장할 때 쓰임
> - `Error` – 에러 정보를 저장할 때 쓰임
> - 기타 등등
>
> 객체마다 고유의 기능을 제공하는데, 이에 대해선 추후 학습할 것인데  종종 'Array 타입’이나 'Date 타입’이라는 용어를 쓰곤 한다. 사실 Array와 Date는 독립적인 자료형이 아니라 '객체’형에 속한다. 객체에 다양한 기능을 넣어 확장한 또 다른 객체이다.
>
> 객체는 다재다능한 자료구조로 자바스크립트에서 그 영향력이 막강하다. 



#### 4.2 참조에 의한 객체 복사

객체와 원시 타입의 근본적인 차이 중 하나는 **객체는 ‘참조에 의해(by reference)’ 저장되고 복사된다는 것이다.**

원시값(문자열, 숫자, 불린 값)은 ‘값 그대로’ 저장·할당되고 복사되는 반면에 말이다.

```javascript
let message = "Hello!";
let phrase = message;
```

예시를 실행하면 두 개의 독립된 변수에 각각 문자열 `"Hello!"`가 저장된다.

![스크린샷 2021-04-26 오후 11 28 20](https://user-images.githubusercontent.com/79819941/116099705-31514d00-a6e7-11eb-80d0-324a5bae65c3.png) 

그런데 객체의 동작방식은 이와 다르다.

`변수엔 객체가 그대로 저장되는 것이 아니라, 객체가 저장 되어있는 '메모리 주소’인 객체에 대한 '참조 값’이 저장된다.`



그림을 통해 변수 user에 객체를 할당할 때 무슨 일이 일어나는지 알아보자.

```javascript
let user = {
  name: "John"
};
```

![스크린샷 2021-04-26 오후 11 28 25](https://user-images.githubusercontent.com/79819941/116099781-462de080-a6e7-11eb-9bae-5d6f17e7ebb1.png) 

객체는 메모리 내 어딘가에 저장되고, 변수 `user`엔 객체를 '참조’할 수 있는 값이 저장된다.

따라서 **객체가 할당된 변수를 복사할 땐 객체의 참조 값이 복사되고 객체는 복사되지 않는다.**

```javascript
let user = { name: "John" };

let admin = user; // 참조값을 복사함
```

변수는 두 개이지만 각 변수엔 동일 객체에 대한 참조 값이 저장된다.

![스크린샷 2021-04-26 오후 11 28 30](https://user-images.githubusercontent.com/79819941/116099867-5b0a7400-a6e7-11eb-894f-edefe814fdc6.png) 

따라서 객체에 접근하거나 객체를 조작할 땐 여러 변수를 사용할 수 있다.

```javascript
let user = { name: 'John' };

let admin = user;

admin.name = 'Pete'; // 'admin' 참조 값에 의해 변경됨

alert(user.name); // 'Pete'가 출력됨. 'user' 참조 값을 이용해 변경사항을 확인함
```

객체를 서랍장에 비유하면 변수는 서랍장을 열 수 있는 열쇠라고 할 수 있다. 서랍장은 하나, 서랍장을 열 수 있는 열쇠는 두 개인데, 그중 하나(`admin`)를 사용해 서랍장을 열어 정돈한 후, 또 다른 열쇠로 서랍장을 열면 정돈된 내용을 볼 수 있다.





### [참조에 의한 비교](https://ko.javascript.info/object-copy#ref-2824)

객체 비교 시, **동등 연산자 `==`와 일치 연산자 `===`는 동일하게 동작한다.**

**비교 시 피연산자인 두 객체가 동일한 객체인 경우에 참을 반환한다.**

두 변수가 같은 객체를 참조하는 예시를 살펴보자. 일치·동등 비교 모두에서 참이 반환된다.

```javascript
let a = {};
let b = a; // 참조에 의한 복사

alert( a == b ); // true, 두 변수는 같은 객체를 참조한다.
alert( a === b ); // true
```



다른 예시를 살펴보자. 두 객체 모두 비어있다는 점에서 같아 보이지만, 독립된 객체이기 때문에 일치·동등 비교하면 거짓이 반환된다.

```javascript
let a = {};
let b = {}; // 독립된 두 객체

alert( a == b ); // false
```

`obj1 > obj2` 같은 대소 비교나 `obj == 5` 같은 원시값과의 비교에선 객체가 원시형으로 변환된다. 객체가 어떻게 원시형으로 변하는지에 대해선 곧 학습할 예정인데, **이러한 비교(객체끼리의 대소 비교나 원시값과 객체를 비교하는 것)가 필요한 경우는 매우 드물긴 하다.** 대개 코딩 실수 때문에 이런 비교가 발생한다.





## [객체 복사, 병합과 Object.assign](https://ko.javascript.info/object-copy#ref-2825)

객체가 할당된 변수를 복사하면 동일한 객체에 대한 참조 값이 하나 더 만들어진다는 걸 배웠다.

그런데 객체를 복제하고 싶다면 어떻게 해야 할까? 기존에 있던 객체와 똑같으면서 독립적인 객체를 만들고 싶다면 말이다.

방법은 있는데 자바스크립트는 객체 복제 내장 메서드를 지원하지 않기 때문에 조금 어렵다. 사실 객체를 복제해야 할 일은 거의 없다. 참조에 의한 복사로 해결 가능한 일이 대다수이기 때문이다.

정말 복제가 필요한 상황이라면 새로운 객체를 만든 다음 기존 객체의 프로퍼티들을 순회해 원시 수준까지 프로퍼티를 복사하면 된다. 

아래와 같이 하면된다.

```javascript
let user = {
  name: "John",
  age: 30
};

let clone = {}; // 새로운 빈 객체

// 빈 객체에 user 프로퍼티 전부를 복사해 넣습니다.
for (let key in user) {
  clone[key] = user[key];
}

// 이제 clone은 완전히 독립적인 복제본이 되었습니다.
clone.name = "Pete"; // clone의 데이터를 변경합니다.

alert( user.name ); // 기존 객체에는 여전히 John이 있습니다.
```



[Object.assign](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)를 사용하는 방법도 있는데, 문법과 동작방식은 다음과 같다.

```javascript
Object.assign(dest, [src1, src2, src3...])
```

- 첫 번째 인수 `dest`는 목표로 하는 객체다.
- 이어지는 인수 `src1, ..., srcN`는 복사하고자 하는 객체다. `...`은 필요에 따라 얼마든지 많은 객체를 인수로 사용할 수 있다는 것을 나타낸다.
- 객체 `src1, ..., srcN`의 프로퍼티를 `dest`에 복사한다. `dest`를 제외한 인수(객체)의 프로퍼티 전부가 첫 번째 인수(객체)로 복사된다.
- 마지막으로 `dest`를 반환한다.

`assign` 메서드를 사용해 여러 객체를 하나로 병합하는 예시를 살펴보자.

```javascript
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// permissions1과 permissions2의 프로퍼티를 user로 복사합니다.
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```

목표 객체(`user`)에 동일한 이름을 가진 프로퍼티가 있는 경우엔 기존 값이 덮어씌워 집니다.

```javascript
let user = { name: "John" };

Object.assign(user, { name: "Pete" });

alert(user.name); // user = { name: "Pete" }
```

`Object.assign`을 사용하면 반복문 없이도 간단하게 객체를 복사할 수 있습니다.

```javascript
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```

예시를 실행하면 `user`에 있는 모든 프로퍼티가 빈 배열에 복사되고 변수에 할당된다.





## [중첩 객체 복사](https://ko.javascript.info/object-copy#ref-2826)

지금까진 `user`의 모든 프로퍼티가 원시값인 경우만 가정했는데, 프로퍼티는 다른 객체에 대한 참조 값일 수도 있다. 이 경우는 어떻게 해야 할까?

아래와 같이 하면된다.

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

alert( user.sizes.height ); // 182
```

`clone.sizes = user.sizes`로 프로퍼티를 복사하는 것만으론 객체를 복제할 수 없다. `user.sizes`는 객체이기 때문에 참조 값이 복사되기 때문이다. `clone.sizes = user.sizes`로 프로퍼티를 복사하면 `clone`과 `user`는 같은 sizes를 아래와 같이 공유하게 된다.

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, 같은 객체입니다.

// user와 clone는 sizes를 공유합니다.
user.sizes.width++;       // 한 객체에서 프로퍼티를 변경합니다.
alert(clone.sizes.width); // 51, 다른 객체에서 변경 사항을 확인할 수 있습니다.
```

이 문제를 해결하려면 `user[key]`의 각 값을 검사하면서, 그 값이 객체인 경우 객체의 구조도 복사해주는 반복문을 사용해야 합니다. 이런 방식을 '깊은 복사(deep cloning)'라고 한다.

깊은 복사 시 사용되는 표준 알고리즘인 [Structured cloning algorithm](https://html.spec.whatwg.org/multipage/structured-data.html#safe-passing-of-structured-data)을 사용하면 위 사례를 비롯한 다양한 상황에서 객체를 복제할 수 있습니다.

자바스크립트 라이브러리 [lodash](https://lodash.com/)의 메서드인 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)을 사용하면 이 알고리즘을 직접 구현하지 않고도 깊은 복사를 처리할 수 있으므로 참고하자.



>## [요약](https://ko.javascript.info/object-copy#ref-2827)
>
>객체는 참조에 의해 할당되고 복사된다. 변수엔 ‘객체’ 자체가 아닌 메모리상의 주소인 '참조’가 저장된다. 따라서 객체가 할당된 변수를 복사하거나 함수의 인자로 넘길 땐 객체가 아닌 객체의 참조가 복사된다.
>
>그리고 복사된 참조를 이용한 모든 작업(프로퍼티 추가·삭제 등)은 동일한 객체를 대상으로 이뤄진다.
>
>객체의 '진짜 복사본’을 만들려면 '얕은 복사(shallow copy)'를 가능하게 해주는 `Object.assign`이나 '깊은 복사’를 가능하게 해주는 [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep)를 사용하면 된다. 이때 얕은 복사본은 중첩 객체를 처리하지 못한다는 점을 기억해 두자.





#### 4.3 가비지 컬렉션

자바스크립트는 눈에 보이지 않는 곳에서 메모리 관리를 수행한다.

원시값, 객체, 함수 등 우리가 만드는 모든 것은 메모리를 차지한다. 그렇다면 더는 쓸모 없어지게 된 것들은 어떻게 처리될까? 





## [가비지 컬렉션 기준](https://ko.javascript.info/garbage-collection#ref-3018)

자바스크립트는 *도달 가능성(reachability)* 이라는 개념을 사용해 메모리 관리를 수행한다.

**‘도달 가능한(reachable)’ 값은 쉽게 말해 어떻게든 접근하거나 사용할 수 있는 값을 의미한다. 도달 가능한 값은 메모리에서 삭제되지 않는다.**

1. 아래 소개해 드릴 값들은 그 태생부터 도달 가능하기 때문에, 명백한 이유 없이는 삭제되지 않는다.

   - 현재 함수의 지역 변수와 매개변수
   - 중첩 함수의 체인에 있는 함수에서 사용되는 변수와 매개변수
   - 전역 변수
   - 기타 등등

   이런 값은 *루트(root)* 라고 부른다.

2. 루트가 참조하는 값이나 체이닝으로 루트에서 참조할 수 있는 값은 도달 가능한 값이 된다.

   전역 변수에 객체가 저장되어있다고 가정해 보자, 이 객체의 프로퍼티가 또 다른 객체를 참조하고 있다면, 프로퍼티가 참조하는 객체는 도달 가능한 값이 된다. 이 객체가 참조하는 다른 모든 것들도 도달 가능하다고 여겨진다. 

자바스크립트 엔진 내에선 [가비지 컬렉터(garbage collector)](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))가 끊임없이 동작한다. 가비지 컬렉터는 모든 객체를 모니터링하고, 도달할 수 없는 객체는 삭제한다.





## [간단한 예시](https://ko.javascript.info/garbage-collection#ref-3019)

```javascript
// user엔 객체 참조 값이 저장된다.
let user = {
  name: "John"
};
```

![스크린샷 2021-04-27 오전 1 05 05](https://user-images.githubusercontent.com/79819941/116114933-cdce1c00-a6f4-11eb-9afc-2281397f98a1.png) 

이 그림에서 화살표는 객체 참조를 나타낸다. 전역 변수 `"user"`는 `{name: "John"}` (줄여서 John)이라는 객체를 참조한다. John의 프로퍼티 `"name"`은 원시값을 저장하고 있기 때문에 객체 안에 표현했다.

`user`의 값을 다른 값으로 덮어쓰면 참조(화살표)가 사라진다.

```javascript
user = null;
```

![스크린샷 2021-04-27 오전 1 05 09](https://user-images.githubusercontent.com/79819941/116115141-0d950380-a6f5-11eb-8f68-7eda5c3005fb.png) 

이제 John은 도달할 수 없는 상태가 되었습니다. John에 접근할 방법도, John을 참조하는 것도 모두 사라져버렸다. 가비지 컬렉터는 이제 John에 저장된 데이터를 삭제하고, John을 메모리에서 삭제한다.





## [참조 두 개](https://ko.javascript.info/garbage-collection#ref-3020)

참조를 `user`에서 `admin`으로 복사했다고 가정해보자.

```javascript
// user엔 객체 참조 값이 저장된다.
let user = {
  name: "John"
};

let admin = user;
```

![스크린샷 2021-04-27 오전 1 05 14](https://user-images.githubusercontent.com/79819941/116115234-26051e00-a6f5-11eb-972f-96818b586940.png) 

그리고 위에서 한것 처럼 `user`의 값을 다른 값으로 덮어써 보자.

```javascript
user = null;
```

전역 변수 `admin`을 통하면 여전히 객체 John에 접근할 수 있기 때문에 John은 메모리에서 삭제되지 않는다. 이 상태에서 `admin`을 다른 값(null 등)으로 덮어쓰면 John은 메모리에서 삭제될 수 있다.





## [연결된 객체](https://ko.javascript.info/garbage-collection#ref-3021)

이제 가족관계를 나타내는 복잡한 예시를 살펴보겠다.

```javascript
function marry(man, woman) {
  woman.husband = man;
  man.wife = woman;

  return {
    father: man,
    mother: woman
  }
}

let family = marry({
  name: "John"
}, {
  name: "Ann"
});
```

함수 `marry`(결혼하다)는 매개변수로 받은 두 객체를 서로 참조하게 하면서 '결혼’시키고, 두 객체를 포함하는 새로운 객체를 반환하며, 메모리 구조는 아래와 같이 나타낼 수 있다. 

![스크린샷 2021-04-27 오전 1 05 19](https://user-images.githubusercontent.com/79819941/116115452-5f3d8e00-a6f5-11eb-8aed-7aeb230c412d.png) 

지금은 모든 객체가 도달 가능한 상태다. 이제 참조 두 개를 지워보겠다.

```javascript
delete family.father;
delete family.mother.husband;
```

![스크린샷 2021-04-27 오전 1 05 24](https://user-images.githubusercontent.com/79819941/116115659-72e8f480-a6f5-11eb-926e-c2a72a4c6825.png) 

삭제한 두 개의 참조 중 하나만 지웠다면, 모든 객체가 여전히 도달 가능한 상태였을 것이다.

하지만 참조 두 개를 지우면 John으로 들어오는 참조(화살표)는 모두 사라져 John은 도달 가능한 상태에서 벗어난다.

![스크린샷 2021-04-27 오전 1 05 29](https://user-images.githubusercontent.com/79819941/116115945-91e78680-a6f5-11eb-9b95-2c1929381549.png) 

외부로 나가는 참조는 도달 가능한 상태에 영향을 주지 않는다. 외부에서 들어오는 참조만이 도달 가능한 상태에 영향을 준다. John은 이제 도달 가능한 상태가 아니기 때문에 메모리에서 제거되며  John에 저장된 데이터(프로퍼티) 역시 메모리에서 사라진다.

가비지 컬렉션 후 메모리 구조는 아래와 같다.

![스크린샷 2021-04-27 오전 1 05 35](https://user-images.githubusercontent.com/79819941/116116110-bd6a7100-a6f5-11eb-94b5-e1bff972e18c.png) 



## [도달할 수 없는 섬](https://ko.javascript.info/garbage-collection#ref-3022)

객체들이 연결되어 섬 같은 구조를 만드는데, 이 섬에 도달할 방법이 없는 경우, 섬을 구성하는 객체 전부가 메모리에서 삭제된다. 

근원 객체 `family`가 아무것도 참조하지 않도록 해 보자.

```javascript
family = null;
```

이제 메모리 내부 상태는 다음과 같아진다.

![스크린샷 2021-04-27 오전 1 05 39](https://user-images.githubusercontent.com/79819941/116116245-e5f26b00-a6f5-11eb-88e8-c4fba004fedd.png) 

도달할 수 없는 섬 예제는 도달 가능성이라는 개념이 얼마나 중요한지 보여준다.

John과 Ann은 여전히 서로를 참조하고 있고, 두 객체 모두 외부에서 들어오는 참조를 갖고 있지만, 이것만으로는 충분하지 않다는걸 보여준다. `"family"` 객체와 루트의 연결이 사라지면 루트 객체를 참조하는 것이 아무것도 없게 된다. 섬 전체가 도달할 수 없는 상태가 되고, 섬을 구성하는 객체 전부가 메모리에서 제거된다.





## [내부 알고리즘](https://ko.javascript.info/garbage-collection#ref-3023)

'mark-and-sweep’이라 불리는 가비지 컬렉션 기본 알고리즘에 대해 알아보자.

'가비지 컬렉션’은 대개 다음 단계를 거쳐 수행된다.

- 가비지 컬렉터는 루트(root) 정보를 수집하고 이를 ‘mark(기억)’ 한다.
- 루트가 참조하고 있는 모든 객체를 방문하고 이것들을 ‘mark’ 한다.
- mark 된 모든 객체에 방문하고 *그 객체들이* 참조하는 객체도 mark 한다. 한번 방문한 객체는 전부 mark 하기 때문에 같은 객체를 다시 방문하는 일은 없다.
- 루트에서 도달 가능한 모든 객체를 방문할 때까지 위 과정을 반복한다.
- mark 되지 않은 모든 객체를 메모리에서 삭제한다.

다음과 같은 객체 구조가 있다고 해보자.

![스크린샷 2021-04-27 오전 1 05 48](https://user-images.githubusercontent.com/79819941/116116499-2d78f700-a6f6-11eb-9bb7-7c98199abb39.png) 

오른편에 '도달할 수 없는 섬’이 보인다. 이제 가비지 컬렉터의 ‘mark-and-sweep’ 알고리즘이 이것을 어떻게 처리하는지 보자. 첫 번째 단계에선 루트를 mark 한다.

 ![스크린샷 2021-04-27 오전 1 05 53](https://user-images.githubusercontent.com/79819941/116116554-3c5fa980-a6f6-11eb-8b5f-145944a2b2bf.png) 

이후 루트가 참조하고 있는 것들을 mark 한다.

![스크린샷 2021-04-27 오전 1 05 57](https://user-images.githubusercontent.com/79819941/116116628-526d6a00-a6f6-11eb-9bc3-d407c3f2409c.png)  

도달 가능한 모든 객체를 방문할 때까지, mark 한 객체가 참조하는 객체를 계속해서 mark 한다.

![스크린샷 2021-04-27 오전 1 06 01](https://user-images.githubusercontent.com/79819941/116116679-62854980-a6f6-11eb-9976-b3eb8c23bca4.png) 

방문할 수 없었던 객체를 메모리에서 삭제한다.

![스크린샷 2021-04-27 오전 1 06 04](https://user-images.githubusercontent.com/79819941/116116716-6d3fde80-a6f6-11eb-9680-45fd2d56713b.png) 

루트에서 페인트를 들이붓는다고 상상하면 이 과정을 이해하기 쉬운데 루트를 시작으로 참조를 따라가면서 도달가능한 객체 모두에 페인트가 칠해진다고 생각하면 된다. 이때 페인트가 묻지 않은 객체는 메모리에서 삭제된다.

지금까지 가비지 컬렉션이 어떻게 동작하는지에 대한 개념을 알아보았다. 자바스크립트 엔진은 실행에 영향을 미치지 않으면서 가비지 컬렉션을 더 빠르게 하는 다양한 최적화 기법을 적용한다.



최적화 기법:

- **generational collection(세대별 수집)** – 객체를 '새로운 객체’와 '오래된 객체’로 나눈다. 객체 상당수는 생성 이후 제 역할을 빠르게 수행해 금방 쓸모가 없어지는데, 이런 객체를 '새로운 객체’로 구분한다. 가비지 컬렉터는 이런 객체를 공격적으로 메모리에서 제거된다. 일정 시간 이상 동안 살아남은 객체는 '오래된 객체’로 분류하고, 가비지 컬렉터가 덜 감시한다.
- **incremental collection(점진적 수집)** – 방문해야 할 객체가 많다면 모든 객체를 한 번에 방문하고 mark 하는데 상당한 시간이 소모된다. 가비지 컬렉션에 많은 리소스가 사용되어 실행 속도도 눈에 띄게 느려질 것이다. 자바스크립트 엔진은 이런 현상을 개선하기 위해 가비지 컬렉션을 여러 부분으로 분리한 다음, 각 부분을 별도로 수행한다. 작업을 분리하고, 변경 사항을 추적하는 데 추가 작업이 필요하긴 하지만, 긴 지연을 짧은 지연 여러 개로 분산시킬 수 있다는 장점이 있다.
- **idle-time collection(유휴 시간 수집)** – 가비지 컬렉터는 실행에 주는 영향을 최소화하기 위해 CPU가 유휴 상태일 때에만 가비지 컬렉션을 실행한다.

이 외에도 다양한 최적화 기법과 가비지 컬렉션 알고리즘이 있다. 다양한 기법과 알고리즘을 소개해 드리고 싶지만, 엔진마다 세부 사항이나 기법이 다르기 때문에 여기서 그만한다. 엔진이 발전하면 기법도 달라지기 때문에 학습해야 할 이유가 진짜 없다면 ‘심화’ 학습은 그리 가치 있지 않다고 생각한다. 



>## [요약](https://ko.javascript.info/garbage-collection#ref-3024)
>
>지금까지 알아본 내용을 요약해 보자.
>
>- 가비지 컬렉션은 엔진이 자동으로 수행하므로 개발자는 이를 억지로 실행하거나 막을 수 없다.
>- 객체는 도달 가능한 상태일 때 메모리에 남는다.
>- 참조된다고 해서 도달 가능한 것은 아니며, 서로 연결된 객체들도 도달 불가능할 수 있다.
>
>모던 자바스크립트 엔진은 좀 더 발전된 가비지 컬렉션 알고리즘을 사용한다.
>
>어떤 알고리즘을 사용하는지 궁금하다면 ‘The Garbage Collection Handbook: The Art of Automatic Memory Management’(저자 – R. Jones et al)를 참고하자.
>
>저수준(low-level) 프로그래밍에 익숙하다면, [A tour of V8: Garbage Collection](http://jayconrod.com/posts/55/a-tour-of-v8-garbage-collection)을 읽어보자. V8 가비지 컬렉터에 대한 자세한 내용을 확인해 볼 수 있다.
>
>[V8 공식 블로그](https://v8.dev/)에도 메모리 관리 방법 변화에 대한 내용이 올라온다다. 가비지 컬렉션을 심도 있게 학습하려면 V8 내부구조를 공부하거나 V8 엔지니어로 일했던 [Vyacheslav Egorov](http://mrale.ph/)의 블로그를 읽는 것도 좋다. 여러 엔진 중 ‘V8’ 엔진을 언급하는 이유는 인터넷에서 관련 글을 쉽게 찾을 수 있기 때문이다. V8과 타 엔진들은 동작 방법이 비슷한데, 가비지 컬렉션 동작 방식에는 많은 차이가 있다.
>
>저수준 최적화가 필요한 상황이라면, 엔진에 대한 조예가 깊어야 하는데, 먼저 자바스크립트에 익숙해진 후에 엔진에 대해 학습하는 것을 추천한다.





#### 4.4 메서드와 'this'

객체는 사용자(user), 주문(order) 등과 같이 실제 존재하는 개체(entity)를 표현하고자 할 때 생성된다.

```javascript
let user = {
  name: "John",
  age: 30
};
```

사용자는 현실에서 장바구니에서 물건 선택하기, 로그인하기, 로그아웃하기 등의 행동을 한다. 이와 마찬가지로 사용자를 나타내는 객체 user도 특정한 *행동*을 할 수 있다.

자바스크립트에선 객체의 프로퍼티에 함수를 할당해 객체에게 행동할 수 있는 능력을 부여해준다.





## [메서드 만들기](https://ko.javascript.info/object-methods#ref-2690)

객체 `user`에게 인사할 수 있는 능력을 부여해 보자.

```javascript
let user = {
  name: "John",
  age: 30
};

user.sayHi = function() {
  alert("안녕하세요!");
};

user.sayHi(); // 안녕하세요!
```

함수 표현식으로 함수를 만들고, 객체 프로퍼티 `user.sayHi`에 함수를 할당해 주었다.

이제 객체에 할당된 함수를 호출하면 user가 인사를 해준다.

이렇게 객체 프로퍼티에 할당된 함수를 *메서드(method)* 라고 부른다.

위 예시에선 `user`에 할당된 `sayHi`가 메서드이다.

메서드는 아래와 같이 이미 정의된 함수를 이용해서 만들 수도 있다.

```javascript
let user = {
  // ...
};

// 함수 선언
function sayHi() {
  alert("안녕하세요!");
};

// 선언된 함수를 메서드로 등록
user.sayHi = sayHi;

user.sayHi(); // 안녕하세요!
```



**객체 지향 프로그래밍**

객체를 사용하여 개체를 표현하는 방식을 [객체 지향 프로그래밍(object-oriented programming, OOP)](https://en.wikipedia.org/wiki/Object-oriented_programming) 이라 부른다.

OOP는 그 자체만으로도 학문의 분야를 만드는 중요한 주제이다. 올바른 개체를 선택하는 방법, 개체 사이의 상호작용을 나타내는 방법 등에 관한 의사결정은 객체 지향 설계를 기반으로 이뤄진다. 객체 지향 프로그래밍 관련 추천도서로는 에릭 감마의 ‘GoF의 디자인 패턴’, 그래디 부치의 ‘UML을 활용한 객체지향 분석 설계’ 등이 있다.





### [메서드 단축 구문](https://ko.javascript.info/object-methods#ref-2691)

객체 리터럴 안에 메서드를 선언할 때 사용할 수 있는 단축 문법을 소개해본다.

```javascript
// 아래 두 객체는 동일하게 동작한다.

user = {
  sayHi: function() {
    alert("Hello");
  }
};

// 단축 구문을 사용하니 더 깔끔해 보인다.
user = {
  sayHi() { // "sayHi: function()"과 동일하다.
    alert("Hello");
  }
};
```

위처럼 `function`을 생략해도 메서드를 정의할 수 있다.

일반적인 방법과 단축 구문을 사용한 방법이 완전히 동일하진 않다. 객체 상속과 관련된 미묘한 차이가 존재하는데 지금으로선 이 차이가 중요하지 않기 때문에 넘어가자.





## [메서드와 ‘this’](https://ko.javascript.info/object-methods#ref-2692)

메서드는 객체에 저장된 정보에 접근할 수 있어야 제 역할을 할 수 있다. 모든 메서드가 그런 건 아니지만, 대부분의 메서드가 객체 프로퍼티의 값을 활용한다.

`user.sayHi()`의 내부 코드에서 객체 `user`에 저장된 이름(name)을 이용해 인사말을 만드는 경우가 이런 경우에 속한다.

**메서드 내부에서 `this` 키워드를 사용하면 객체에 접근할 수 있다.**

이때 '점 앞’의 `this`는 객체를 나타냅니다. 정확히는 메서드를 호출할 때 사용된 객체를 나타낸다.



```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // 'this'는 '현재 객체'를 나타낸다.
    alert(this.name);
  }

};

user.sayHi(); // John
```

`user.sayHi()`가 실행되는 동안에 `this`는 `user`를 나타낸다.

`this`를 사용하지 않고 외부 변수를 참조해 객체에 접근하는 것도 가능하다.

```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(user.name); // 'this' 대신 'user'를 이용함
  }

};
```

그런데 이렇게 외부 변수를 사용해 객체를 참조하면 예상치 못한 에러가 발생할 수 있다. `user`를 복사해 다른 변수에 할당(`admin = user`)하고, `user`는 전혀 다른 값으로 덮어썼다고 가정해 보자. `sayHi()`는 원치 않는 값(null)을 참조할 것이다.



```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert( user.name ); // Error: Cannot read property 'name' of null
  }

};


let admin = user;
user = null; // user를 null로 덮어쓴다.

admin.sayHi(); // sayHi()가 엉뚱한 객체를 참고하면서 에러가 발생했다.
```

`alert` 함수가 `user.name` 대신 `this.name`을 인수로 받았다면 에러가 발생하지 않았을 것이다.





## [자유로운 “this”](https://ko.javascript.info/object-methods#ref-2693)

자바스크립트의 `this`는 다른 프로그래밍 언어의 `this`와 동작 방식이 다릅니다. 자바스크립트에선 모든 함수에 `this`를 사용할 수 있다.

아래와 같이 코드를 작성해도 문법 에러가 발생하지 않는다.

```javascript
function sayHi() {
  alert( this.name );
}
```

`this` 값은 런타임에 결정됩니다. 컨텍스트에 따라 달라진다.

동일한 함수라도 다른 객체에서 호출했다면 'this’가 참조하는 값이 달라진다.

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (점과 대괄호는 동일하게 동작함)
```

규칙은 간단하다. `obj.f()`를 호출했다면 `this`는 `f`를 호출하는 동안의 `obj`입니다. 위 예시에선 `obj`가 `user`나 `admin`을 참조할 것이다.



**객체 없이 호출하기: `this == undefined`**

객체가 없어도 함수를 호출할 수 있다.

```javascript
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

위와 같은 코드를 엄격 모드에서 실행하면, `this`엔 `undefined`가 할당됩니다. `this.name`으로 name에 접근하려고 하면 에러가 발생한다.

그런데 엄격 모드가 아닐 때는 `this`가 *전역 객체*를 참조한다. 브라우저 환경에선 `window`라는 전역 객체를 참조하는데 이런 동작 차이는 `"use strict"`가 도입된 배경이기도 하다. 

이런 식의 코드는 대개 실수로 작성된 경우가 많다. 함수 본문에 `this`가 사용되었다면, 객체 컨텍스트 내에서 함수를 호출할 것이라고 예상하시면 된다.



**자유로운 `this`가 만드는 결과**

다른 언어를 사용하다 자바스크립트로 넘어온 개발자는 `this`를 혼동하기 쉽다. `this`는 항상 메서드가 정의된 객체를 참조할 것이라고 착각하죠. 이런 개념을 'bound `this`'라고 한다.

자바스크립트에서 `this`는 런타임에 결정된다. 메서드가 어디서 정의되었는지에 상관없이 `this`는 ‘점 앞의’ 객체가 무엇인가에 따라 ‘자유롭게’ 결정된다.

이렇게 `this`가 런타임에 결정되면 좋은 점도 있고 나쁜 점도 있다. 함수(메서드)를 하나만 만들어 여러 객체에서 재사용할 수 있다는 것은 장점이지만, 이런 유연함이 실수로 이어질 수 있다는 것은 단점이다.

자바스크립트가 `this`를 다루는 방식이 좋은지, 나쁜지는 우리가 판단할 문제가 아니다. 개발자는 `this`의 동작 방식을 충분히 이해하고 장점을 취하면서 실수를 피하는 데만 집중하면 된다.





## ['this’가 없는 화살표 함수](https://ko.javascript.info/object-methods#ref-2694)

화살표 함수는 일반 함수와는 달리 ‘고유한’ `this`를 가지지 않는다. 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 ‘평범한’ 외부 함수에서 `this` 값을 가져온다.

아래 예시에서 함수 `arrow()`의 `this`는 외부 함수 `user.sayHi()`의 `this`가 된다.

```javascript
let user = {
  firstName: "보라",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // 보라
```

별개의 `this`가 만들어지는 건 원하지 않고, 외부 컨텍스트에 있는 `this`를 이용하고 싶은 경우 화살표 함수가 유용하다. 이에 대한 자세한 내용은 별도의 챕터, [화살표 함수 다시 살펴보기](https://ko.javascript.info/arrow-functions)에서 다룬다.



>## [요약](https://ko.javascript.info/object-methods#ref-2695)
>
>- 객체 프로퍼티에 저장된 함수를 '메서드’라고 부른다.
>- `object.doSomthing()`은 객체를 '행동’할 수 있게 해준다.
>- 메서드는 `this`로 객체를 참조한다.
>
>`this` 값은 런타임에 결정된다.
>
>- 함수를 선언할 때 `this`를 사용할 수 있다. 다만, 함수가 호출되기 전까지 `this`엔 값이 할당되지 않는다.
>- 함수를 복사해 객체 간 전달할 수 있다.
>- 함수를 객체 프로퍼티에 저장해 `object.method()`같이 ‘메서드’ 형태로 호출하면 `this`는 `object`를 참조한다.
>
>화살표 함수는 자신만의 `this`를 가지지 않는다는 점에서 독특한데 화살표 함수 안에서 `this`를 사용하면, 외부에서 `this` 값을 가져온다.



#### 4.5 'new' 연산자와 생성자 함수

#### 4.6 옵셔널 체이닝 '?'

#### 4.7 심볼형

#### 4.8 객체를 원시형으로 변환하기

