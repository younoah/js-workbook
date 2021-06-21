## 6.11 화살표 함수 다시 살펴보기

화살표 함수(arrow function)에 대해 다시 보면,  화살표 함수는 단순히 함수를 ‘짧게’ 쓰기 위한 용도로 사용되지 않는다.  화살표 함수는 몇 가지 독특하고 유용한 기능을 제공하는데 다음과 같다.

자바스크립트를 사용하다 보면 저 멀리 동떨어진 곳에서 실행될 작은 함수를 작성해야 하는 상황을 자주 만나게 된다.

- `arr.forEach(func)` – `func`는 `forEach`가 호출될 때 배열 `arr`의 요소 전체를 대상으로 실행된다.
- `setTimeout(func)` – `func`는 내장 스케줄러에 의해 실행된다.
- 기타 등등…

이처럼 자바스크립트에선 함수를 생성하고 그 함수를 어딘가에 전달하는 것이 아주 자연스럽다.

그런데 어딘가에 함수를 전달하게 되면 함수의 컨텍스트를 잃을 수 있다. 이럴 때 화살표 함수를 사용하면 현재 컨텍스트를 잃지 않아 편리하다.



#### 화살표 함수에는 'this’가 없습니다

[메서드와 this](https://ko.javascript.info/object-methods) 챕터에서 화살표 함수엔 `this`가 없다는 것을 배운 바 있는데, 화살표 함수 본문에서 `this`에 접근하면, 외부에서 값을 가져온다. 이런 특징은 객체의 메서드(`showList()`) 안에서 동일 객체의 프로퍼티(`students`)를 대상으로 순회를 하는 데 사용할 수 있다.

```javascript
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};

group.showList();
```

예시의 `forEach`에서 화살표 함수를 사용했기 때문에 화살표 함수 본문에 있는 `this.title`은 화살표 함수 바깥에 있는 메서드인 `showList`가 가리키는 대상과 동일해진다. 즉 `this.title`은 `group.title`과 같다.

위 예시에서 화살표 함수 대신 ‘일반’ 함수를 사용했다면 에러가 발생했을 것이다.

```javascript
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach(function(student) {
      // TypeError: Cannot read property 'title' of undefined
      alert(this.title + ': ' + student)
    });
  }
};

group.showList();
```

에러는 `forEach`에 전달되는 함수의 `this`가 `undefined` 이어서 발생했다. `alert` 함수에서 `undefined.title`에 접근하려 했기 때문에 얼럿 창엔 에러가 출력된다.

그런데 화살표 함수는 `this` 자체가 없기 때문에 이런 에러가 발생하지 않는다. 화살표 함수는 `new`와 함께 실행할 수 없다.

`this`가 없기 때문에 화살표 함수는 생성자 함수로 사용할 수 없다는 제약이 있다. 화살표 함수는 `new`와 함께 호출할 수 없다.

**화살표 함수 vs. bind**

화살표 함수와 일반 함수를 `.bind(this)`를 사용해서 호출하는 것 사이에는 미묘한 차이가 있다.

- `.bind(this)`는 함수의 '한정된 버전(bound version)'을 만든다.
- 화살표 함수는 어떤 것도 바인딩시키지 않는다. 화살표 함수엔 단지 `this`가 없을 뿐이다. 화살표 함수에서 `this`를 사용하면 일반 변수 서칭과 마찬가지로 `this`의 값을 외부 렉시컬 환경에서 찾는다.



## 화살표 함수엔 'arguments’가 없습니다

화살표 함수는 일반 함수와는 다르게 모든 인수에 접근할 수 있게 해주는 유사 배열 객체 `arguments`를 지원하지 않는다.

이런 특징은 현재 `this` 값과 `arguments` 정보를 함께 실어 호출을 포워딩해 주는 데코레이터를 만들 때 유용하게 사용된다.

아래 예시에서 데코레이터 `defer(f, ms)`는 함수를 인자로 받고 이 함수를 래퍼로 감싸 반환하는데, 함수 `f`는 `ms` 밀리초 후에 호출된다.

```javascript
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}

function sayHi(who) {
  alert('안녕, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("철수"); // 2초 후 "안녕, 철수"가 출력된다.
```

화살표 함수를 사용하지 않고 동일한 기능을 하는 데코레이터 함수를 만들면 다음과 같다.

```javascript
function defer(f, ms) {
  return function(...args) {
    let ctx = this;
    setTimeout(function() {
      return f.apply(ctx, args);
    }, ms);
  };
}
```

일반 함수에선 `setTimeout`에 넘겨주는 콜백 함수에서 사용할 변수 `ctx`와 `args`를 반드시 만들어줘야 한다.



> #### 요약
>
> 화살표 함수가 일반 함수와 다른 점은 다음과 같다.
>
> - `this`를 가지지 않는다.
> - `arguments`를 지원하지 않는다.
> - `new`와 함께 호출할 수 없다.
> - 이 외에도 화살표 함수는 `super`가 없다는 특징도 있는데, 아직 `super`에 대해 배우지 않았기 때문에 이번 챕터에선 해당 내용을 다루지 않았고, 화살표 함수와 `super`의 관계는 [클래스 상속](https://ko.javascript.info/class-inheritance) 챕터에서 학습할 수 있다.
>
> **화살표 함수는 컨텍스트가 있는 긴 코드보다는 자체 '컨텍스트’가 없는 짧은 코드를 담을 용도로 만들어졌다. 그리고 이 목적에 잘 들어맞는 특징을 보인다.**







## 프로퍼티 플래그와 설명자

아시다시피 객체엔 프로퍼티가 저장된다. 지금까진 프로퍼티를 단순히 ‘키-값’ 쌍의 관점에서만 다뤘다. 그런데 사실 프로퍼티는 우리가 생각했던 것보다 더 유연하고 강력한 자료구조이다.

이 챕터에선 객체 프로퍼티 추가 구성 옵션 몇 가지를 다루고, 이어지는 챕터에선 이 옵션들을 이용해 손쉽게 getter나 setter 함수를 만드는 법을 알아보자.



#### 프로퍼티 플래그

객체 프로퍼티는 **`값(value)`** 과 함께 플래그(flag)라 불리는 특별한 속성 세 가지를 갖는다.

- **`writable`** – `true`이면 값을 수정할 수 있습니다. 그렇지 않다면 읽기만 가능하다.
- **`enumerable`** – `true`이면 반복문을 사용해 나열할 수 있다. 그렇지 않다면 반복문을 사용해 나열할 수 없다.
- **`configurable`** – `true`이면 프로퍼티 삭제나 플래그 수정이 가능합니다. 그렇지 않다면 프로퍼티 삭제와 플래그 수정이 불가능하다.

프로퍼티 플래그는 특별한 경우가 아니고선 다룰 일이 없기 때문에 여기서 처음 소개하게 되었다. 지금까지 해왔던 '평범한 방식’으로 프로퍼티를 만들면 해당 프로퍼티의 플래그는 모두 `true`가 된다. 이렇게 `true`로 설정된 플래그는 언제든 수정할 수 있다.

자 이제 본격적으로 프로퍼티 플래그에 대해 다뤄보기전에 먼저 플래그를 얻는 방법을 알아보자.

[Object.getOwnPropertyDescriptor](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)메서드를 사용하면 특정 프로퍼티에 대한 정보를 *모두* 얻을 수 있다.

문법:

```javascript
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

- `obj`

  정보를 얻고자 하는 객체

- `propertyName`

  정보를 얻고자 하는 객체 내 프로퍼티

메서드를 호출하면 "프로퍼티 설명자(descriptor)"라고 불리는 객체가 반환되는데, 여기에는 프로퍼티 값과 세 플래그에 대한 정보가 모두 담겨있다.



예시:                      

```javascript
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

메서드 [Object.defineProperty](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)를 사용하면 플래그를 변경할 수 있다.

문법:

```javascript
Object.defineProperty(obj, propertyName, descriptor)
```

- `obj`, `propertyName`

  설명자를 적용하고 싶은 객체와 객체 프로퍼티

- `descriptor`

  적용하고자 하는 프로퍼티 설명자

`defineProperty`메서드는 객체에 해당 프로퍼티가 있으면 플래그를 원하는 대로 변경해준다. 프로퍼티가 없으면 인수로 넘겨받은 정보를 이용해 새로운 프로퍼티를 만든다. 이때 플래그 정보가 없으면 플래그 값은 자동으로 `false`가 된다.



아래 예시를 보면 프로퍼티 `name`이 새로 만들어지고, 모든 플래그 값이 `false`가 된 것을 확인할 수 있다.   

```javascript
let user = {};

Object.defineProperty(user, "name", {
  value: "John"
});

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
 */
```

‘평범한 방식으로’ 객체 프로퍼티 `user.name`을 만들었을 때와 `defineProperty`를 이용해 프로퍼티를 만들었을 때의 가장 큰 차이점은 플래그에 있다. `defineProperty`를 사용해 프로퍼티를 만든 경우, `descriptor`에 플래그 값을 명시하지 않으면 플래그 값이 자동으로 `false`가 됩니다. 플래그 값을 `true`로 설정하려면 `descriptor`에 `true`라고 명시해 주어야 한다. 이제 예시를 통해 플래그의 효과에 대해 알아보자.



#### writable 플래그

`writable` 플래그를 사용해 `user.name`에 값을 쓰지 못하도록(non-writable) 해보자.

```javascript
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete"; // Error: Cannot assign to read only property 'name'
```

이제 `defineProperty`를 사용해 `writable` 플래그를 `true`로 변경하지 않는 한 그 누구도 객체의 이름을 변경할 수 없게 되었다. 에러는 엄격 모드에서만 발생한다.

비 엄격 모드에선 읽기 전용 프로퍼티에 값을 쓰려고 해도 에러가 발생하지 않는다. 다만 이때 값을 변경하는 것은 불가능하다. 비 엄격 모드에선 이와 같이 플래그에서 정한 규칙을 위반하는 행위는 에러 없이 그냥 무시된다.

아래 예시는 위 예시와 동일하게 동작한다. 다만 아래 예시에선 `defineProperty` 메서드를 사용해 프로퍼티를 밑바닥부터 만들어 보았다.

```javascript
let user = { };

Object.defineProperty(user, "name", {
  value: "John",
  // defineProperty를 사용해 새로운 프로퍼티를 만들 땐, 어떤 플래그를 true로 할지 명시해주어야 한다.
  enumerable: true,
  configurable: true
});

alert(user.name); // John
user.name = "Pete"; // Error
```



#### enumerable 플래그

`user`에 커스텀 메서드 `toString`을 추가해보자.

객체 내장 메서드 `toString`은 열거가 불가능(non-enumerable)하기 때문에 `for..in` 사용시 나타나지 않는다. 하지만 커스텀 `toString`을 추가하면 아래와 같이 `for..in`에 `toString`이 나타난다.

```javascript
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

//커스텀 toString은 for...in을 사용해 열거할 수 있다.
for (let key in user) alert(key); // name, toString
```



그런데 특정 프로퍼티의 `enumerable` 플래그 값을 `false`로 설정하면 `for..in` 반복문에 나타나지 않게 할 수 있다. 커스텀 `toString`도 열거가 불가능하게 할 수 있다.

```javascript
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
  enumerable: false
});

// 이제 for...in을 사용해 toString을 열거할 수 없게 되었다.
for (let key in user) alert(key); // name
```

열거가 불가능한 프로퍼티는 `Object.keys`에도 배제된다.

```javascript
alert(Object.keys(user)); // name
```



#### configurable 플래그

구성 가능하지 않음을 나타내는 플래그(non-configurable flag)인 `configurable:false`는 몇몇 내장 객체나 프로퍼티에 기본으로 설정되어있다.

어떤 프로퍼티의 `configurable` 플래그가 `false`로 설정되어 있다면 해당 프로퍼티는 객체에서 지울 수 없다.

내장 객체 `Math`의 `PI` 프로퍼티가 대표적인 예인데, 이 프로퍼티는 쓰기와 열거, 구성이 불가능하다.

```javascript
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```



개발자가 코드를 사용해 `Math.PI` 값을 변경하거나 덮어쓰는 것도 불가능하다.

```javascript
Math.PI = 3; // Error

// 수정도 불가능하지만 지우는 것 역시 불가능하다.
```

`configurable` 플래그를 `false`로 설정하면 돌이킬 방법이 없다. `defineProperty`를 써도 값을 `true`로 되돌릴 수 없다.

`configurable:false`가 만들어내는 구체적인 제약사항은 아래와 같다.

1. `configurable` 플래그를 수정할 수 없음

2. `enumerable` 플래그를 수정할 수 없음.

3. `writable: false`의 값을 `true`로 바꿀 수 없음(`true`를 `false`로 변경하는 것은 가능함).

4. 접근자 프로퍼티 `get/set`을 변경할 수 없음(새롭게 만드는 것은 가능함).

   

이런 특징을 이용하면 아래와 같이 “영원히 변경할 수 없는” 프로퍼티(`user.name`)를 만들 수 있다.                      

```javascript
let user = { };

Object.defineProperty(user, "name", {
  value: "John",
  writable: false,
  configurable: false
});

// user.name 프로퍼티의 값이나 플래그를 변경할 수 없다.
// 아래와 같이 변경하려고 하면 에러가 발생한다.
//   user.name = "Pete"
//   delete user.name
//   Object.defineProperty(user, "name", { value: "Pete" })
Object.defineProperty(user, "name", {writable: true}); // Error
```

"non-configurable"은 "non-writable"과 다르다.

`configurable` 플래그가 `false`이더라도 `writable` 플래그가 `true`이면 프로퍼티 값을 변경할 수 있다.

`configurable: false`는 플래그 값 변경이나 프로퍼티 삭제를 막기 위해 만들어졌지, 프로퍼티 값 변경을 막기 위해 만들어진 게 아니다.



#### Object.defineProperties

[Object.defineProperties(obj, descriptors)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties) 메서드를 사용하면 프로퍼티 여러 개를 한 번에 정의할 수 있다.

문법:

```javascript
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

예시:

```javascript
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

프로퍼티 여러 개를 한 번에 정의해보았다.



#### Object.getOwnPropertyDescriptors

[Object.getOwnPropertyDescriptors(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors) 메서드를 사용하면 프로퍼티 설명자를 전부 한꺼번에 가져올 수 있다.

이 메서드를 `Object.defineProperties`와 함께 사용하면 객체 복사 시 플래그도 함께 복사할 수 있다.

```javascript
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

지금까진 아래처럼 할당 연산자를 사용해 프로퍼티를 복사하는 방법으로 객체를 복사해 왔다.

```javascript
for (let key in user) {
  clone[key] = user[key]
}
```

그런데 이 방법은 플래그는 복사하지 않는다. 플래그 정보도 복사하려면 `Object.defineProperties`를 사용하시기 바란다.

위 샘플 코드처럼 `for..in`을 사용해 객체를 복사하면 심볼형 프로퍼티도 놓치게 된다. 하지만 `Object.getOwnPropertyDescriptors`는 심볼형 프로퍼티를 포함한 프로퍼티 설명자 *전체*를 반환한다.



#### 객체 수정을 막아주는 다양한 메서드

프로퍼티 설명자는 특정 프로퍼티 하나를 대상으로 하는데, 아래 메서드를 사용하면 한 객체 내 프로퍼티 *전체*를 대상으로 하는 제약사항을 만들 수 있다.

- [Object.preventExtensions(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)

  객체에 새로운 프로퍼티를 추가할 수 없게 한다.

- [Object.seal(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)

  새로운 프로퍼티 추가나 기존 프로퍼티 삭제를 막아준다. 프로퍼티 전체에 `configurable: false`를 설정하는 것과 동일한 효과이다.

- [Object.freeze(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

  새로운 프로퍼티 추가나 기존 프로퍼티 삭제, 수정을 막아준다. 프로퍼티 전체에 `configurable: false, writable: false`를 설정하는 것과 동일한 효과이다.

아래 메서드는 위 세 가지 메서드를 사용해서 설정한 제약사항을 확인할 때 사용할 수 있다.

- [Object.isExtensible(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)

  새로운 프로퍼티를 추가하는 게 불가능한 경우 `false`를, 그렇지 않은 경우 `true`를 반환한다.

- [Object.isSealed(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)

  프로퍼티 추가, 삭제가 불가능하고 모든 프로퍼티가 `configurable: false`이면 `true`를 반환한다.

- [Object.isFrozen(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)

  프로퍼티 추가, 삭제, 변경이 불가능하고 모든 프로퍼티가 `configurable: false, writable: false`이면 `true`를 반환한다.

위 메서드들은 실무에선 잘 사용되지 않는다.



## 7.2 프로퍼티 getter와 setter

객체의 프로퍼티는 두 종류로 나뉘는데,

첫 번째 종류는 *데이터 프로퍼티(data property)* 이며, 지금까지 사용한 모든 프로퍼티는 데이터 프로퍼티이다.

두 번째는 *접근자 프로퍼티(accessor property)* 라 불리는 새로운 종류의 프로퍼티이다. 접근자 프로퍼티의 본질은 함수인데, 이 함수는 값을 획득(get)하고 설정(set)하는 역할을 담당한다. 그런데 외부 코드에서는 함수가 아닌 일반적인 프로퍼티처럼 보인다.



#### getter와 setter

접근자 프로퍼티는 'getter(획득자)'와 ‘setter(설정자)’ 메서드로 표현된다. 객체 리터럴 안에서 getter와 setter 메서드는 `get`과 `set`으로 나타낼 수 있다.

```javascript
let obj = {
  get propName() {
    // getter, obj.propName을 실행할 때 실행되는 코드
  },

  set propName(value) {
    // setter, obj.propName = value를 실행할 때 실행되는 코드
  }
};
```

getter 메서드는 `obj.propName`을 사용해 프로퍼티를 읽으려고 할 때 실행되고, setter 메서드는 `obj.propName = value`으로 프로퍼티에 값을 할당하려 할 때 실행된다.

프로퍼티 `name`과 `surname`이 있는 객체 `user`를 만들어보자.

```javascript
let user = {
  name: "John",
  surname: "Smith"
};
```

이 객체에 `fullName` 이라는 프로퍼티를 추가해 `fullName`이 `'John Smith'`가 되도록 해보자. 기존 값을 복사-붙여넣기 하지 않고 `fullName`이 `'John Smith'`가 되도록 하려면 접근자 프로퍼티를 구현하면 된다.

```javascript
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

alert(user.fullName); // John Smith
```

바깥 코드에선 접근자 프로퍼티를 일반 프로퍼티처럼 사용할 수 있다. 접근자 프로퍼티는 이런 아이디어에서 출발했다. 접근자 프로퍼티를 사용하면 함수처럼 *호출* 하지 않고, 일반 프로퍼티에서 값에 접근하는 것처럼 평범하게 `user.fullName`을 사용해 프로퍼티 값을 *얻을 수 있다*. 나머지 작업은 getter 메서드가 뒷단에서 처리해준다.

한편, 위 예시의 `fullName`은 getter 메서드만 가지고 있기 때문에 `user.fullName=`을 사용해 값을 할당하려고 하면 에러가 발생한다.        

```javascript
let user = {
  get fullName() {
    return `...`;
  }
};

user.fullName = "Test"; // Error (프로퍼티에 getter 메서드만 있어서 에러가 발생한다.)
```



`user.fullName`에 setter 메서드를 추가해 에러가 발생하지 않도록 고쳐보자.

```javascript
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
};

// 주어진 값을 사용해 set fullName이 실행된다.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

이렇게 getter와 setter 메서드를 구현하면 객체엔 `fullName`이라는 '가상’의 프로퍼티가 생긴다. 가상의 프로퍼티는 읽고 쓸 순 있지만 실제로는 존재하지 않는다.



#### 접근자 프로퍼티 설명자

데이터 프로퍼티의 설명자와 접근자 프로퍼티의 설명자는 다르다.

접근자 프로퍼티엔 설명자 `value`와 `writable`가 없는 대신에 `get`과 `set`이라는 함수가 있다.

따라서 접근자 프로퍼티는 다음과 같은 설명자를 갖는다.

- **`get`** – 인수가 없는 함수로, 프로퍼티를 읽을 때 동작함

- **`set`** – 인수가 하나인 함수로, 프로퍼티에 값을 쓸 때 호출됨

- **`enumerable`** – 데이터 프로퍼티와 동일함

- **`configurable`** – 데이터 프로퍼티와 동일함

  

아래와 같이 `defineProperty`에 설명자 `get`과 `set`을 전달하면 `fullName`을 위한 접근자를 만들 수 있다.                      

```javascript
let user = {
  name: "John",
  surname: "Smith"
};

Object.defineProperty(user, 'fullName', {
  get() {
    return `${this.name} ${this.surname}`;
  },

  set(value) {
    [this.name, this.surname] = value.split(" ");
  }
});

alert(user.fullName); // John Smith

for(let key in user) alert(key); // name, surname
```



프로퍼티는 접근자 프로퍼티(`get/set` 메서드를 가짐)나 데이터 프로퍼티(`value`를 가짐) 중 한 종류에만 속하고 둘 다에 속할 수 없다는 점을 항상 유의하시기 바란다.



한 프로퍼티에 `get`과 `value`를 동시에 설정하면 에러가 발생한다.

```javascript

// Error: Invalid property descriptor.
Object.defineProperty({}, 'prop', {
  get() {
    return 1
  },

  value: 2
});
```



#### getter와 setter 똑똑하게 활용하기

getter와 setter를 ‘실제’ 프로퍼티 값을 감싸는 래퍼(wrapper)처럼 사용하면, 프로퍼티 값을 원하는 대로 통제할 수 있다.



아래 예시에선 `name`을 위한 setter를 만들어 `user`의 이름이 너무 짧아지는 걸 방지하고 있다. 실제 값은 별도의 프로퍼티 `_name`에 저장된다.

```javascript
let user = {
  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 4) {
      alert("입력하신 값이 너무 짧습니다. 네 글자 이상으로 구성된 이름을 입력하세요.");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); // Pete

user.name = ""; // 너무 짧은 이름을 할당하려 함
```

`user`의 이름은 `_name`에 저장되고, 프로퍼티에 접근하는 것은 getter와 setter를 통해 이뤄진다.

기술적으론 외부 코드에서 `user._name`을 사용해 이름에 바로 접근할 수 있다. 그러나 밑줄 `"_"` 로 시작하는 프로퍼티는 객체 내부에서만 활용하고, 외부에서는 건드리지 않는 것이 관습이다.



#### 호환성을 위해 사용하기

접근자 프로퍼티는 언제 어느 때나 getter와 setter를 사용해 데이터 프로퍼티의 행동과 값을 원하는 대로 조정할 수 있게 해준다는 점에서 유용하다.



데이터 프로퍼티 `name`과 `age`를 사용해서 사용자를 나타내는 객체를 구현한다고 가정해보자.

```javascript
function User(name, age) {
  this.name = name;
  this.age = age;
}

let john = new User("John", 25);

alert( john.age ); // 25
```



그런데 곧 요구사항이 바뀌어서 `age` 대신에 `birthday`를 저장해야 한다고 해보자. `age`보다는 `birthday`가 더 정확하고 편리하기 때문이다.

```javascript
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;
}

let john = new User("John", new Date(1992, 6, 1));
```

이렇게 생성자 함수를 수정하면 기존 코드 중 프로퍼티 `age`를 사용하고 있는 코드도 수정해 줘야 한다.

`age`가 사용되는 부분을 모두 찾아서 수정하는 것도 가능하지만, 시간이 오래 걸린다. 게다가 여러 사람이 `age`를 사용하고 있다면 모두 찾아 수정하는 것 자체가 힘든데, 게다가 `age`는 `user` 안에 있어도 나쁠 것이 없는 프로퍼티이기도 하다.



기존 코드들은 그대로 두고 `age`를 위한 getter를 추가해서 문제를 해결해 보자.

```javascript
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

  // age는 현재 날짜와 생일을 기준으로 계산된다.
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear - this.birthday.getFullYear();
    }
  });
}

let john = new User("John", new Date(1992, 6, 1));

alert( john.birthday ); // birthday를 사용할 수 있다.
alert( john.age );      // age 역시 사용할 수 있다.
```

이제 기존 코드도 잘 작동하고, 멋진 프로퍼티도 새로 생겼다.



## 8.1 프로토타입 상속

개발을 하다 보면 기존에 있는 기능을 가져와 확장해야 하는 경우가 생긴다. 예를들어, 사람에 관한 프로퍼티와 메서드를 가진 `user`라는 객체가 있는데, `user`와 상당히 유사하지만 약간의 차이가 있는 `admin`과 `guest` 객체를 만들어야 한다고 가정해 보면,  "`user`의 메서드를 복사하거나 다시 구현하지 않고 `user`에 약간의 기능을 얹어 `admin`과 `guest` 객체를 만들 수 있지 않을까?"라는 생각이 들 것이다. 바로 여기서 자바스크립트 언어의 고유 기능인 *프로토타입 상속(prototypal inheritance)* 을 이용하면 위와 같은 생각을 실현할 수 있다.



#### [[Prototype\]]

자바스크립트의 객체는 명세서에서 명명한 `[[Prototype]]`이라는 숨김 프로퍼티를 갖는다. 이 숨김 프로퍼티 값은 `null`이거나 다른 객체에 대한 참조가되는데, 다른 객체를 참조하는 경우 참조 대상을 '프로토타입(prototype)'이라 부른다.

<img width="229" alt="Screen Shot 2021-06-21 at 11 24 35 PM" src="https://user-images.githubusercontent.com/79819941/122778530-0c90e480-d2e8-11eb-9e14-129b33a7b41c.png"> 

프로토타입의 동작 방식은 '신비스러운’면이 있다. `object`에서 프로퍼티를 읽으려고 하는데 해당 프로퍼티가 없으면 자바스크립트는 자동으로 프로토타입에서 프로퍼티를 찾기 때문이다. 프로그래밍에선 이런 동작 방식을 '프로토타입 상속’이라 부른다. 언어 차원에서 지원하는 편리한 기능이나 개발 테크닉 중 프로토타입 상속에  기반해 만들어진 것들이 많다.

`[[Prototype]]` 프로퍼티는 내부 프로퍼티이면서 숨김 프로퍼티이지만 다양한 방법을 사용해 개발자가 값을 설정할 수 있는데, 아래 예시처럼 특별한 이름인 `__proto__`을 사용하면 값을 설정할 수 있다.

```javascript
let animal = {
  eats: true
};
let rabbit = {
  jumps: true
};

rabbit.__proto__ = animal;
```

`__proto__`는 `[[Prototype]]`용 getter·setter이다.

`__proto__`는 `[[Prototype]]`과 *다르다*. `__proto__`는 `[[Prototype]]`의 getter(획득자)이자 setter(설정자) 이다.

하위 호환성 때문에 여전히 `__proto__`를 사용할 순 있지만 비교적 근래에 작성된 스크립트에선 `__proto__` 대신 함수 `Object.getPrototypeOf`나 `Object.setPrototypeOf`을 써서 프로토타입을 획득(get)하거나 설정(set)한다. 

`__proto__`는 브라우저 환경에서만 지원하도록 자바스크립트 명세서에서 규정하였는데, 실상은 서버 사이드를 포함한 모든 호스트 환경에서 `__proto__`를 지원한다. `[[Prototype]]`보다는 `__proto__`가 조금 더 직관적이어서 이해하기 쉬우므로, 본 튜토리얼의 예시에선 `__proto__`를 사용하도록 한다.

객체 `rabbit`에서 프로퍼티를 얻고싶은데 해당 프로퍼티가 없다면, 자바스크립트는 자동으로 `animal`이라는 객체에서 프로퍼티를 얻는다.

```javascript
let animal = {
  eats: true
};
let rabbit = {
  jumps: true
};

rabbit.__proto__ = animal; // (*)

// 프로퍼티 eats과 jumps를 rabbit에서도 사용할 수 있게 되었다.
alert( rabbit.eats ); // true (**)
alert( rabbit.jumps ); // true
```

`(*)`로 표시한 줄에선 `animal`이 `rabbit`의 프로토타입이 되도록 설정하였다.

`(**)`로 표시한 줄에서 `alert` 함수가 `rabbit.eats` 프로퍼티를 읽으려 했는데, `rabbit`엔 `eats`라는 프로퍼티가 없다. 이때 자바스크립트는 `[[Prototype]]`이 참조하고 있는 객체인 `animal`에서 `eats`를 얻어낸다. 아래 그림을 밑에서부터 위로 살펴보자.

<img width="235" alt="Screen Shot 2021-06-21 at 11 24 43 PM" src="https://user-images.githubusercontent.com/79819941/122778756-377b3880-d2e8-11eb-9560-3980a291f43a.png"> 

이제 “`rabbit`의 프로토타입은 `animal`이다.” 혹은 "`rabbit`은 `animal`을 상속한다."라고 말 할 수 있게 되었다.

프로토타입을 설정해 준 덕분에 `rabbit`에서도 `animal`에 구현된 유용한 프로퍼티와 메서드를 사용할 수 있게 되었는데, 프로토타입에서 상속받은 프로퍼티를 '상속 프로퍼티(inherited property)'라고 한다.

상속 프로퍼티를 사용해 `animal`에 정의된 메서드를 `rabbit`에서 호출해 보자.                      

```javascript
let animal = {
  eats: true,
  walk() {
    alert("동물이 걷습니다.");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// 메서드 walk는 rabbit의 프로토타입인 animal에서 상속받았다.
rabbit.walk(); // 동물이 걷습니다.
```

아래 그림과 같이 프로토타입(`animal`)에서 `walk`를 자동으로 상속받았기 때문에 `rabbit`에서도 `walk`를 호출할 수 있게 되었다. 

<img width="239" alt="Screen Shot 2021-06-21 at 11 24 49 PM" src="https://user-images.githubusercontent.com/79819941/122778843-4bbf3580-d2e8-11eb-96e4-922fcb337931.png"> 

프로토타입 체인은 지금까지 살펴본 예시들보다 길어질 수 있다.                      

```javascript
let animal = {
  eats: true,
  walk() {
    alert("동물이 걷습니다.");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

let longEar = {
  earLength: 10,
  __proto__: rabbit
};

// 메서드 walk는 프로토타입 체인을 통해 상속받았다.
longEar.walk(); // 동물이 걷습니다.
alert(longEar.jumps); // true (rabbit에서 상속받음)
```

<img width="254" alt="Screen Shot 2021-06-21 at 11 24 56 PM" src="https://user-images.githubusercontent.com/79819941/122778895-5a0d5180-d2e8-11eb-975e-58c7f5bee517.png"> 

프로토타입 체이닝엔 두 가지 제약사항이 있다.

1. 순환 참조(circular reference)는 허용되지 않는다. `__proto__`를 이용해 닫힌 형태로 다른 객체를 참조하면 에러가 발생한다.
2. `__proto__`의 값은 객체나 `null`만 가능합니다. 다른 자료형은 무시된다.

여기에 더하여 객체엔 오직 하나의 `[[Portotype]]`만 있을 수 있다는 당연한 제약도 있다. 객체는 두 개의 객체를 상속받지 못한다.



#### 쓸 때는 프로토타입을 사용하지 않는다.

프로토타입은 프로퍼티를 읽을 때만 사용한다. 프로퍼티를 추가, 수정하거나 지우는 연산은 객체에 직접 해야 하는데 객체 `rabbit`에 메서드 `walk`를 직접 할당해 보자.

```javascript
let animal = {
  eats: true,
  walk() {
    /* rabbit은 이제 이 메서드를 사용하지 않는다. */
  }
};

let rabbit = {
  __proto__: animal
};

rabbit.walk = function() {
  alert("토끼가 깡충깡충 뜁니다.");
};

rabbit.walk(); // 토끼가 깡충깡충 뜁니다.
```

`rabbit.walk()`를 호출하면 프로토타입에 있는 메서드가 실행되지 않고, 객체 `rabbit`에 추가한 메서드가 실행된다.

<img width="234" alt="Screen Shot 2021-06-21 at 11 25 04 PM" src="https://user-images.githubusercontent.com/79819941/122778960-6a253100-d2e8-11eb-8649-49d25bb65bea.png"> 

그런데 접근자 프로퍼티(accessor property)는 setter 함수를 통해서 프로퍼티에 값을 할당하므로 이 규칙이 적용되지 않는다. 접근자 프로퍼티에 값을 할당하는 것은 함수를 호출하는 것과 같기 때문이다.

아래 예시에서 `admin.fullName`이 의도한 대로 잘 작동하는 것을 확인할 수 있다.

```javascript
let user = {
  name: "John",
  surname: "Smith",

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  },

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

let admin = {
  __proto__: user,
  isAdmin: true
};

alert(admin.fullName); // John Smith (*)

// setter 함수가 실행된다!
admin.fullName = "Alice Cooper"; // (**)

alert(admin.fullName); // Alice Cooper , state of admin modified
alert(user.fullName); // John Smith , state of user protected
```

`(*)`로 표시한 줄에서 `admin.fullName`은 프로토타입(`user`)에 있는 getter 함수(`get fullName`)를 호출하고, `(**)`로 표시한 줄의 할당 연산은 프로토타입에 있는 setter 함수(`set fullName`)를 호출한다.



#### 'this’가 나타내는 것

위 예시를 보면 "`set fullName(value)` 본문의 `this`엔 어떤 값이 들어가지?"라는 의문을 가질 수 있다. "프로퍼티 `this.name`과 `this.surname`에 값을 쓰면 그 값이 `user`에 저장될까, 아니면 `admin`에 저장될까?"라는 의문도 생길 수 있다. 답은 간단한데, `this`는 프로토타입에 영향을 받지 않는다.

**메서드를 객체에서 호출했든 프로토타입에서 호출했든 상관없이 `this`는 언제나 `.` 앞에 있는 객체가 된다.**

`admin.fullName=`으로 setter 함수를 호출할 때, `this`는 `user`가 아닌 `admin`이 된다.

객체 하나를 만들고 여기에 메서드를 많이 구현해 놓은 다음, 여러 객체에서 이 커다란 객체를 상속받게 하는 경우가 많기  때문에 이런 특징을 잘 알아둬야 한다. 상속받은 메서드를 사용하더라도 객체는 프로토타입이 아닌 자신의 상태를 수정한다.

예시를 통해 좀 더 알아보면 ‘메서드 저장소’ 역할을 하는 객체 `animal`을 `rabbit`이 상속받게 해보자.

`rabbit.sleep()`을 호출하면 `this.isSleeping`이 객체 `rabbit`에 설정된다.

```javascript
// animal엔 다양한 메서드가 있다.
let animal = {
  walk() {
    if (!this.isSleeping) {
      alert(`동물이 걸어갑니다.`);
    }
  },
  sleep() {
    this.isSleeping = true;
  }
};

let rabbit = {
  name: "하얀 토끼",
  __proto__: animal
};

// rabbit의 프로퍼티 isSleeping을 true로 변경한다.
rabbit.sleep();

alert(rabbit.isSleeping); // true
alert(animal.isSleeping); // undefined (프로토타입에는 isSleeping이라는 프로퍼티가 없다.)
```

위 코드를 실행한 후, 객체의 상태를 그림으로 나타내면 다음과 같다.

<img width="247" alt="Screen Shot 2021-06-21 at 11 25 13 PM" src="https://user-images.githubusercontent.com/79819941/122779105-87f29600-d2e8-11eb-9cc7-c7a3f6662749.png"> 

`rabbit` 말고도 `bird`, `snake` 등이 `animal`을 상속받는다고 해보자. 이 객체들도 `rabbit`처럼 `animal`에 구현된 메서드를 사용할 수 있다. 이때 상속받은 메서드의 `this`는 `animal`이 아닌 실제 메서드가 호출되는 시점의 점(`.`) 앞에 있는 객체가 되는데,  `this`에 데이터를 쓰면 `animal`이 아닌 해당 객체의 상태가 변화한다.

메서드는 공유되지만, 객체의 상태는 공유되지 않는다고 결론 내릴 수 있다.



#### for…in 반복문

`for..in`은 상속 프로퍼티도 순회대상에 포함시킨다.

```javascript
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// Object.keys는 객체 자신의 키만 반환한다.
alert(Object.keys(rabbit)); // jumps

// for..in은 객체 자신의 키와 상속 프로퍼티의 키 모두를 순회한다.
for(let prop in rabbit) alert(prop); // jumps, eats
```

[obj.hasOwnProperty(key)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)를 이용하면 상속 프로퍼티를 순회 대상에서 제외할 수 있다. 이 내장 메서드는 `key`에 대응하는 프로퍼티가 상속 프로퍼티가 아니고 `obj`에 직접 구현되어있는 프로퍼티일 때 `true`를 반환한다.

`obj.hasOwnProperty(key)`를 응용하면 아래 예시에서처럼 상속 프로퍼티를 걸러낼 수 있고, 상속 프로퍼티만을 대상으로 무언가를 할 수도 있다.

```javascript
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

for(let prop in rabbit) {
  let isOwn = rabbit.hasOwnProperty(prop);

  if (isOwn) {
    alert(`객체 자신의 프로퍼티: ${prop}`); // 객체 자신의 프로퍼티: jumps
  } else {
    alert(`상속 프로퍼티: ${prop}`); // 상속 프로퍼티: eats
  }
}
```

위 예시의 상속 관계를 그림으로 나타내면 다음과 같다. `rabbit`은 `animal`을, `animal`은 `Object.prototype`을, `Object.prototype`은 `null`을 상속받고 있다. `animal`이 `Object.prototype`를 상속받는 이유는 객체 리터럴 방식으로 선언하였기 때문이다.

<img width="291" alt="Screen Shot 2021-06-21 at 11 25 19 PM" src="https://user-images.githubusercontent.com/79819941/122779314-b6707100-d2e8-11eb-9cd1-2ba22dc1dd3d.png"> 

그림을 보면 `for..in` 안에서 사용한 메서드 `hasOwnProperty`가 `Object.prototype.hasOwnProperty`에서 왔다는 것을 확인할 수 있다.

그런데 상속 프로퍼티인 `eats`는 얼럿 창에 출력되는데, `hasOwnProperty`는 출력되지 않았다.

이유는 간단한데 `hasOwnProperty`는 열거 가능한(enumerable) 프로퍼티가 아니기 때문이다. `Object.prototype`에 있는 모든 메서드의 `enumerable` 플래그는 `false`인데, `for..in`은 오직 열거 가능한 프로퍼티만 순회 대상에 포함하기 때문에 `hasOwnProperty`는 얼럿창에 출력되지 않는다.

> 키-값을 순회하는 메서드 대부분은 상속 프로퍼티를 제외하고 동작한다.
>
> `Object.keys`, `Object.values` 같이 객체의 키-값을 대상으로 무언가를 하는 메서드 대부분은 상속 프로퍼티를 제외하고 동작한다. 프로토타입에서 상속받은 프로퍼티는 *제외하고*, 해당 객체에서 정의한 프로퍼티만 연산 대상에 포함된다.





>#### **요약**
>
>- 자바스크립트의 모든 객체엔 숨김 프로퍼티 `[[Prototype]]`이 있는데, 이 프로퍼티는 객체나 `null`을 가리킨다.
>- `obj.__proto__`를 사용하면 프로토타입에 접근할 수 있다. `__proto__`는 `[[Prototype]]`의 getter·setter로 쓰이는데, 요즘엔 잘 쓰지 않는다.
>- `[[Prototype]]`이 참조하는 객체를 '프로토타입’이라고 한다.
>- `obj`에서 프로퍼티를 읽거나 메서드를 호출하려는데 해당하는 프로퍼티나 메서드가 없으면 자바스크립트는 프로토타입에서 프로퍼티나 메서드를 찾는다.
>- 접근자 프로퍼티가 아닌 데이터 프로퍼티를 다루고 있다면, 쓰기나 지우기와 관련 연산은 프로토타입을 통하지 않고 객체에 직접 적용된다.
>- 프로토타입에서 상속받은 `method`라도 `obj.method()`를 호출하면 `method` 안의 `this`는 호출 대상 객체인 `obj`를 가리킨다.
>- `for..in` 반복문은 객체 자체에서 정의한 프로퍼티뿐만 아니라 상속 프로퍼티도 순회 대상에 포함한다. 반면, 키-값과 관련된 내장 메서드 대부분은 상속 프로퍼티는 제외하고 객체 자체 프로퍼티만을 대상으로 동작한다.

