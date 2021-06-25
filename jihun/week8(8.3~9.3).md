## 8.3 네이티브 프로토타입

`prototype` 프로퍼티는 자바스크립트 내부에서도 광범위하게 사용되는데, 모든 내장 생성자 함수에서 `prototype` 프로퍼티를 사용한다.

첫 번째로 자세히 살펴본 다음 어떻게 내장 객체에 새 기능을 추가하여 프로토타입 프로퍼티를 사용하는지 알아본다.



#### Object.prototype

빈 객체를 표현한다고 해 보자.

```javascript
let obj = {};
alert( obj ); // "[object Object]" ?????
```

`"[object Object]"` 문자열을 생성하는 코드는 어디에 있을까? `toString` 메서드에서 이 문자열을 생성한다는 것을 앞서 배워서 알고 있지만 도대체 코드는 어디에 있는 걸까? `obj`는 비어 있는데 말이다.

`obj = new Object()`를 줄이면 `obj = {}`가 된다. `Object`는 내장 객체 생성자 함수인데, 이 생성자 함수의 `prototype`은 `toString`을 비롯한 다양한 메서드가 구현되어있는 거대한 객체를 참조한다.

아래 그림을 보면, 

![Screen Shot 2021-06-24 at 10 39 44 PM](https://user-images.githubusercontent.com/79819941/123272965-31ca6080-d53d-11eb-90c0-63c24ed0df7c.png) 

`new Object()`를 호출하거나 리터럴 문법 `{...}`을 사용해 객체를 만들 때, 새롭게 생성된 객체의 `[[Prototype]]`은 이전 챕터에서 언급한 규칙에 따라 `Object.prototype`을 참조한다.

![Screen Shot 2021-06-24 at 10 41 14 PM](https://user-images.githubusercontent.com/79819941/123273123-5292b600-d53d-11eb-8058-137e48371280.png) 

따라서 `obj.toString()`을 호출하면 `Object.prototype`에서 해당 메서드를 가져오게 되는 것이다.

아래 예시를 통해 이를 확인할 수 있다.

```javascript
let obj = {};

alert(obj.__proto__ === Object.prototype); // true

alert(obj.toString === obj.__proto__.toString); //true
alert(obj.toString === Object.prototype.toString); //true
```

그런데 이때 `Object.prototype` 위의 체인엔 `[[Prototype]]`이 없다는 점을 주의해야 한다.

```javascript
alert(Object.prototype.__proto__); // null
```



#### 다른 내장 프로토타입

`Array`, `Date`, `Function`을 비롯한 내장 객체들 역시 프로토타입에 메서드를 저장해 놓는다.

배열 `[1, 2, 3]`을 만들면 기본 `new Array()` 생성자가 내부에서 사용되기 때문에 `Array.prototype`이 배열 `[1, 2, 3]`의 프로토타입이 된다. 또, `Array.prototype`은 배열 메서드도 제공한다. 이런 내부 동작은 메모리 효율을 높여주는 장점을 가져다준다.

명세서에선 모든 내장 프로토타입의 꼭대기엔 `Object.prototype`이 있어야 한다고 규정하는데, 이런 규정 때문에 몇몇 사람들은 "모든 것은 객체를 상속받는다."라는 말을 한다.

세 개의 내장 객체를 이용해 전체적인 그림을 그리면 다음과 같다.

![Screen Shot 2021-06-24 at 10 43 42 PM](https://user-images.githubusercontent.com/79819941/123273495-a8fff480-d53d-11eb-813f-7957392b59f2.png) 

각 내장 객체의 프로토타입을 확인해 보자.

```javascript
let arr = [1, 2, 3];

// arr은 Array.prototype을 상속받음?
alert( arr.__proto__ === Array.prototype ); // true

// arr은 Object.prototype을 상속받음?
alert( arr.__proto__.__proto__ === Object.prototype ); // true

// 체인 맨 위엔 null이 있다.
alert( arr.__proto__.__proto__.__proto__ ); // null
```

체인 상의 프로토타입엔 중복 메서드가 있을 수 있다. `Array.prototype`엔 요소 사이에 쉼표를 넣어 요소 전체를 합친 문자열을 반환하는 자체 메서드 `toString`가 있다.

```javascript
let arr = [1, 2, 3]
alert(arr); // 1,2,3 <-- Array.prototype.toString 의 결과
```

그런데 `Object.prototype`에도 메서드 `toString`이 있다. 이렇게 중복 메서드가 있을 때는 체인 상에서 가까운 곳에 있는 메서드가 사용되는데 `Array.prototype`이 체인 상에서 더 가깝기 때문에 `Array.prototype`의 `toString`이 사용된다.

![Screen Shot 2021-06-24 at 10 44 54 PM](https://user-images.githubusercontent.com/79819941/123273727-dd73b080-d53d-11eb-9cf6-2e11b595900d.png) 

Chrome 개발자 콘솔과 같은 도구를 사용하면 상속 관계를 확인할 수 있다. `console.dir`를 사용하면 내장 객체의 상속 관계를 확인하는 데 도움이 된다.

![img](https://ko.javascript.info/article/native-prototypes/console_dir_array.png) 

배열이 아닌 다른 내장 객체들 또한 같은 방법으로 동작한다. 함수도 마찬가지이다. 함수는 내장 객체 `Function`의 생성자를 사용해 만들어지는데 `call`, `apply`를 비롯한 함수에서 사용할 수 있는 메서드는 `Fuction.prototype`에서 가져온다. 함수에도 `toString`이 구현되어 있다.

```javascript
function f() {}

alert(f.__proto__ == Function.prototype); // true
alert(f.__proto__.__proto__ == Object.prototype); // true, 객체에서 상속받음
```



#### 원시값

문자열과 숫자 불린값을 다루는 것은 엄청 까다롭다. 문자열과 숫자 불린값은 객체가 아닌데 이런 원시값들의 프로퍼티에 접근하려고 하면 내장 생성자 `String`, `Number`, `Boolean`을 사용하는 임시 래퍼(wrapper) 객체가 생성된다. 임시 래퍼 객체는 이런 메서드를 제공하고 난 후에 사라진다. 래퍼 객체는 보이지 않는 곳에서 만들어지며 최적화는 엔진이 담당한다. 그런데 명세서에선 각 자료형에 해당하는 래퍼 객체의 메서드를 프로토타입 안에 구현해 놓고 `String.prototype`, `Number.prototype`, `Boolean.prototype`을 사용해 쓸 수 있도록 규정한다.

**`null`과 `undefined`에 대응하는 래퍼 객체는 없다.**

특수 값인 `null`과 `undefined`는 문자열과 숫자 불린값과는 거리가 있다. `null`과 `undefined`에 대응하는 래퍼 객체는 없다. 따라서 `null`과 `undefined`에선 메서드와 프로퍼티를 이용할 수 없습니다. 프로토타입도 물론 사용할 수 없다.



#### 네이티브 프로토타입 변경하기

네이티브 프로토타입을 수정할 수 있다. `String.prototype`에 메서드를 하나 추가하면 모든 문자열에서 해당 메서드를 사용할 수 있다.

```javascript
String.prototype.show = function() {
  alert(this);
};

"BOOM!".show(); // BOOM!
```

개발을 하다 보면 "새로운 내장 메서드를 만드는 게 좋지 않을까?"라는 생각이 들 때가 있다. 네이티브 프로토타입에 새 내장 메서드를 추가하고 싶은 생각이 한번 쯤은 들 건데 이는 좋지 않은 방법이다.



**중요:**

프로토타입은 전역으로 영향을 미치기 때문에 프로토타입을 조작하면 충돌이 날 가능성이 높다고 한다. 두 라이브러리에서 동시에 `String.prototype.show` 메서드를 추가하면 한 라이브러리의 메서드가 다른 라이브러리의 메서드를 덮어쓰기 때문이다. 그렇기 때문에 이런 이유로 네이티브 프로토타입을 수정하는 것을 추천하지 않는다.

**모던 프로그래밍에서 네이티브 프로토타입 변경을 허용하는 경우는 딱 하나뿐인데 바로 폴리필을 만들 때이다.**

폴리필은 자바스크립트 명세서에 있는 메서드와 동일한 기능을 하는 메서드 구현체를 의미한다. 명세서에는 정의되어 있으나 특정 자바스크립트 엔진에서는 해당 기능이 구현되어있지 않을 때 폴리필을 사용한다.

폴리필을 직접 구현하고 난 후 폴리필을 내장 프로토타입에 추가할 때만 네이티브 프로토타입을 변경하자.

```javascript
if (!String.prototype.repeat) { // repeat이라는 메서드가 없다고 가정해보자
  // 프로토타입에 repeat를 추가

  String.prototype.repeat = function(n) {
    // string을 n회 반복(repeat)한다.

    // 실제 이 메서드를 구현하려면 코드는 더 복잡해질것이다.
    // 전체 알고리즘은 명세서에서 확인할 수 있다.
    // 그런데 완벽하지 않은 폴리필이라도 충분히 쓸만하다.
    return new Array(n + 1).join(this);
  };
}

alert( "La".repeat(3) ); // LaLaLa
```

