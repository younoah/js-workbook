## 5.7 맵과 셋

지금까진 아래와 같은 복잡한 자료구조를 학습해 보았다.

- 객체 – 키가 있는 컬렉션을 저장함
- 배열 – 순서가 있는 컬렉션을 저장함

하지만 현실 세계를 반영하기엔 이 두 자료구조 만으론 부족해서 `맵(Map)`과 `셋(Set)`이 등장하게 되었다.



#### 맵

[맵(Map)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)은 키가 있는 데이터를 저장한다는 점에서 `객체`와 유사하지만, `맵`은 키에 다양한 자료형을 허용한다는 점에서 차이가 있다.



맵에는 다음과 같은 주요 메서드와 프로퍼티가 있다.

- `new Map()` – 맵을 만든다.
- `map.set(key, value)` – `key`를 이용해 `value`를 저장한다.
- `map.get(key)` – `key`에 해당하는 값을 반환한다. `key`가 존재하지 않으면 `undefined`를 반환한다.
- `map.has(key)` – `key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.
- `map.delete(key)` – `key`에 해당하는 값을 삭제한다.
- `map.clear()` – 맵 안의 모든 요소를 제거한다.
- `map.size` – 요소의 개수를 반환한다.





```javascript
let map = new Map();

map.set('1', 'str1');   // 문자형 키
map.set(1, 'num1');     // 숫자형 키
map.set(true, 'bool1'); // 불린형 키

// 객체는 키를 문자형으로 변환한다는 걸 기억하자.
// 맵은 키의 타입을 변환시키지 않고 그대로 유지한다. 따라서 아래의 코드는 출력되는 값이 다르다.
alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```



맵은 객체와 달리 키를 문자형으로 변환하지 않는다. 키엔 자료형 제약이 없다.

**`map[key]`는 `Map`을 쓰는 바른 방법이 아니다.**

`map[key] = 2`로 값을 설정하는 것 같이 `map[key]`를 사용할 수 있긴 하지만, 이 방법은 `map`을 일반 객체처럼 취급하게 된다. 따라서 여러 제약이 생기게 된다. `map`을 사용할 땐 `map`전용 메서드 `set`, `get` 등을 사용해야만 한다.

**맵은 키로 객체를 허용한다.**

```javascript
let john = { name: "John" };

// 고객의 가게 방문 횟수를 세본다고 가정해 보자.
let visitsCountMap = new Map();

// john을 맵의 키로 사용하자.
visitsCountMap.set(john, 123);

alert( visitsCountMap.get(john) ); // 123
```

객체를 키로 사용할 수 있다는 점은 `맵`의 가장 중요한 기능 중 하나이다. `객체`에는 문자열 키를 사용할 수 있다. 하지만 객체 키는 사용할 수 없다.



객체형 키를 `객체`에 써보면,

```javascript
let john = { name: "John" };

let visitsCountObj = {}; // 객체를 하나 만든다.

visitsCountObj[john] = 123; // 객체(john)를 키로 해서 객체에 값(123)을 저장해보자.

// 원하는 값(123)을 얻으려면 아래와 같이 키가 들어갈 자리에 `object Object`를 써줘야한다.
alert( visitsCountObj["[object Object]"] ); // 123
```

`visitsCountObj`는 객체이기 때문에 모든 키를 문자형으로 변환시킨다. 이 과정에서 `john`은 문자형으로 변환되어 `"[object Object]"`가 된다.



**`맵`이 키를 비교하는 방식**

`맵`은 [SameValueZero](https://tc39.github.io/ecma262/#sec-samevaluezero)라 불리는 알고리즘을 사용해 값의 등가 여부를 확인한다. 이 알고리즘은 일치 연산자 `===`와 거의 유사하지만, `NaN`과 `NaN`을 같다고 취급하는 것에서 일치 연산자와 차이가 있다. 따라서 맵에선 `NaN`도 키로 쓸 수 있다.

이 알고리즘은 수정하거나 커스터마이징 하는 것이 불가능하다.



**체이닝**

`map.set`을 호출할 때마다 맵 자신이 반환된다. 이를 이용하면 `map.set`을 '체이닝(chaining)'할 수 있다.

```javascript
map.set('1', 'str1')
  .set(1, 'num1')
  .set(true, 'bool1');
```



#### 맵의 요소에 반복 작업하기

다음 세 가지 메서드를 사용해 `맵`의 각 요소에 반복 작업을 할 수 있다.

- `map.keys()` – 각 요소의 키를 모은 반복 가능한(iterable, 이터러블) 객체를 반환한다.
- `map.values()` – 각 요소의 값을 모은 이터러블 객체를 반환한다.
- `map.entries()` – 요소의 `[키, 값]`을 한 쌍으로 하는 이터러블 객체를 반환한다. 이 이터러블 객체는 `for..of`반복문의 기초로 쓰인다.

```javascript
let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',    50]
]);

// 키(vegetable)를 대상으로 순회한다.
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // cucumber, tomatoes, onion
}

// 값(amount)을 대상으로 순회한다.
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// [키, 값] 쌍을 대상으로 순회한다.
for (let entry of recipeMap) { // recipeMap.entries()와 동일하다.
  alert(entry); // cucumber,500 ...
}
```



**맵은 삽입 순서를 기억한다.**

`맵`은 값이 삽입된 순서대로 순회를 실시한다. `객체`가 프로퍼티 순서를 기억하지 못하는 것과는 다르다.

여기에 더하여 `맵`은 `배열`과 유사하게 내장 메서드 `forEach`도 지원한다.

```javascript
// 각 (키, 값) 쌍을 대상으로 함수를 실행
recipeMap.forEach( (value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 ...
});
```



#### Object.entries: 객체를 맵으로 바꾸기

각 요소가 키-값 쌍인 배열이나 이터러블 객체를 초기화 용도로 `맵`에 전달해 새로운 `맵`을 만들 수 있다.

아래와 같이 말이다.

```javascript
// 각 요소가 [키, 값] 쌍인 배열
let map = new Map([
  ['1',  'str1'],
  [1,    'num1'],
  [true, 'bool1']
]);

alert( map.get('1') ); // str1
```

평범한 객체를 가지고 `맵`을 만들고 싶다면 내장 메서드 [Object.entries(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)를 활용해야 한다. 이 메서드는 객체의 키-값 쌍을 요소(`[key, value]`)로 가지는 배열을 반환한다.



```javascript
let obj = {
  name: "John",
  age: 30
};

let map = new Map(Object.entries(obj));

alert( map.get('name') ); // John
```

`Object.entries`를 사용해 객체 `obj`를 배열 `[ ["name","John"], ["age", 30] ]`로 바꾸고, 이 배열을 이용해 새로운 `맵`을 만들어보았다.



#### Object.fromEntries: 맵을 객체로 바꾸기

방금까진 `Object.entries(obj)`를 사용해 평범한 객체를 `맵`으로 바꾸는 방법에 대해 알아보았다.

이젠 이 반대인 맵을 객체로 바꾸는 방법에 대해 알아보겠다. `Object.fromEntries`를 사용하면 가능하다. 이 메서드는 각 요소가 `[키, 값]` 쌍인 배열을 객체로 바꿔준다.

```javascript
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);

// now prices = { banana: 1, orange: 2, meat: 4 }

alert(prices.orange); // 2
```

`Object.fromEntries`를 사용해 `맵`을 객체로 바꿔보자.

자료가 `맵`에 저장되어있는데, 서드파티 코드에서 자료를 객체형태로 넘겨받길 원할 때 이 방법을 사용할 수 있다.

```javascript
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

let obj = Object.fromEntries(map.entries()); // 맵을 일반 객체로 변환 (*)

// 맵이 객체가 되었습니다!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange); // 2
```

`map.entries()`를 호출하면 맵의 `[키, 값]`을 요소로 가지는 이터러블을 반환한다. `Object.fromEntries`를 사용하기 위해 딱 맞는 형태이다.

`(*)`로 표시한 줄을 좀 더 짧게 줄이는 것도 가능하다.

```javascript
let obj = Object.fromEntries(map); // .entries()를 생략함
```

`Object.fromEntries`는 인수로 이터러블 객체를 받기 때문에 짧게 줄인 코드도 이전 코드와 동일하게 동작한다. 꼭 배열을 전달해줄 필요는 없다. 그리고 `map`에서의 일반적인 반복은 `map.entries()`를 사용했을 때와 같은 키-값 쌍을 반환한다. 따라서 `map`과 동일한 키-값을 가진 일반 객체를 얻게 된다.





#### 셋

`셋(Set)`은 중복을 허용하지 않는 값을 모아놓은 특별한 컬렉션이다. 셋에 키가 없는 값이 저장되며, 주요 메서드는 다음과 같다.

- `new Set(iterable)` – 셋을 만든다. `이터러블` 객체를 전달받으면(대개 배열을 전달받음) 그 안의 값을 복사해 셋에 넣어준다.
- `set.add(value)` – 값을 추가하고 셋 자신을 반환한다.
- `set.delete(value)` – 값을 제거한다. 호출 시점에 셋 내에 값이 있어서 제거에 성공하면 `true`, 아니면 `false`를 반환한다.
- `set.has(value)` – 셋 내에 값이 존재하면 `true`, 아니면 `false`를 반환한다.
- `set.clear()` – 셋을 비운다.
- `set.size` – 셋에 몇 개의 값이 있는지 세준다.



셋 내에 동일한 값(value)이 있다면 `set.add(value)`을 아무리 많이 호출하더라도 아무런 반응이 없을 것이다. 셋 내의 값에 중복이 없는 이유가 바로 이 때문이다.

방문자 방명록을 만든다고 가정해 보자. 한 방문자가 여러 번 방문해도 방문자를 중복해서 기록하지 않겠다고 결정 내린 상황이다. 즉, 한 방문자는 '단 한 번만 기록’되어야 한다.

이때 적합한 자료구조가 바로 `셋` 이다.

```javascript
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// 어떤 고객(john, mary)은 여러 번 방문할 수 있다.
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// 셋에는 유일무이한 값만 저장된다.
alert( set.size ); // 3

for (let user of set) {
  alert(user.name); // // John, Pete, Mary 순으로 출력된다.
}
```

`셋` 대신 배열을 사용하여 방문자 정보를 저장한 후, 중복 값 여부는 배열 메서드인 [arr.find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)를 이용해 확인할 수도 있다. 하지만 `arr.find`는 배열 내 요소 전체를 뒤져 중복 값을 찾기 때문에, 셋보다 성능 면에서 떨어진다. 반면, `셋`은 값의 유일무이함을 확인하는데 최적화 되어있다.





#### 셋의 값에 반복 작업하기

`for..of`나 `forEach`를 사용하면 셋의 값을 대상으로 반복 작업을 수행할 수 있다.

```javascript
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// forEach를 사용해도 동일하게 동작한다.
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

`forEach`에 쓰인 콜백 함수는 세 개의 인수를 받는데, 첫 번째는 `값`, 두 번째도 *같은 값*인 `valueAgain`을 받고 있다. 세 번째는 목표하는 객체(셋)이며 동일한 값이 인수에 두 번 등장하고 있다.

이렇게 구현된 이유는 `맵`과의 호환성 때문인데 `맵`의 `forEach`에 쓰인 콜백이 세 개의 인수를 받을 때를 위해서다. 이상해 보이겠지만 이렇게 구현해 놓았기 때문에 `맵`을 `셋`으로 혹은 `셋`을 `맵`으로 교체하기가 쉽다.



`셋`에도 `맵`과 마찬가지로 반복 작업을 위한 메서드가 있다.

- `set.keys()` – 셋 내의 모든 값을 포함하는 이터러블 객체를 반환한다.
- `set.values()` – `set.keys`와 동일한 작업을 합니다. `맵`과의 호환성을 위해 만들어진 메서드이다.
- `set.entries()` – 셋 내의 각 값을 이용해 만든 `[value, value]` 배열을 포함하는 이터러블 객체를 반환한다. `맵`과의 호환성을 위해 만들어졌다.





> #### 요약
>
> `맵`은 키가 있는 값이 저장된 컬렉션이다.
>
> 주요 메서드와 프로퍼티:
>
> - `new Map([iterable])` – 맵을 만든다. `[key,value]`쌍이 있는 `iterable`(예: 배열)을 선택적으로 넘길 수 있는데, 이때 넘긴 이터러블 객체는 맵 초기화에 사용된다.
> - `map.set(key, value)` – 키를 이용해 값을 저장한다.
> - `map.get(key)` – 키에 해당하는 값을 반환한다. `key`가 존재하지 않으면 `undefined`를 반환한다.
> - `map.has(key)` – 키가 있으면 `true`, 없으면 `false`를 반환한다.
> - `map.delete(key)` – 키에 해당하는 값을 삭제한다.
> - `map.clear()` – 맵 안의 모든 요소를 제거한다.
> - `map.size` – 요소의 개수를 반환한다.
>
> 
>
> 일반적인 `객체`와의 차이점:
>
> - 키의 타입에 제약이 없다. 객체도 키가 될 수 있다.
> - `size` 프로퍼티 등의 유용한 메서드나 프로퍼티가 있다.
>
> `셋`은 중복이 없는 값을 저장할 때 쓰이는 컬렉션이다.
>
> 
>
> 주요 메서드와 프로퍼티:
>
> - `new Set([iterable])` – 셋을 만든다. `iterable` 객체를 선택적으로 전달받을 수 있는데(대개 배열을 전달받음), 이터러블 객체 안의 요소는 셋을 초기화하는데 쓰인다.
> - `set.add(value)` – 값을 추가하고 셋 자신을 반환한다. 셋 내에 이미 `value`가 있는 경우 아무런 작업을 하지 않는다.
> - `set.delete(value)` – 값을 제거한다. 호출 시점에 셋 내에 값이 있어서 제거에 성공하면 `true`, 아니면 `false`를 반환한다.
> - `set.has(value)` – 셋 내에 값이 존재하면 `true`, 아니면 `false`를 반환한다.
> - `set.clear()` – 셋을 비운다.
> - `set.size` – 셋에 몇 개의 값이 있는지 세준다.
>
> `맵`과 `셋`에 반복 작업을 할 땐, 해당 컬렉션에 요소나 값을 추가한 순서대로 반복 작업이 수행된다. 따라서 이 두 컬렉션은 정렬이 되어있지 않다고 할 수 없다. 그렇지만 컬렉션 내 요소나 값을 재 정렬하거나 (배열에서 인덱스를 이용해 요소를 가져오는 것처럼) 숫자를 이용해 특정 요소나 값을 가지고 오는 것은 불가능하다.







## 5.8 위크맵과 위크셋

 [가비지 컬렉션](https://ko.javascript.info/garbage-collection)에서 배웠듯이 자바스크립트 엔진은 도달 가능한 (그리고 추후 사용될 가능성이 있는) 값을 메모리에 유지한다.



```javascript
let john = { name: "John" };

// 위 객체는 john이라는 참조를 통해 접근할 수 있다.

// 그런데 참조를 null로 덮어쓰면 위 객체에 더 이상 도달이 가능하지 않게 되어
john = null;

// 객체가 메모리에서 삭제된다.
```

자료구조를 구성하는 요소도 자신이 속한 자료구조가 메모리에 남아있는 동안 대개 도달 가능한 값으로 취급되어 메모리에서 삭제되지 않는다. 객체의 프로퍼티나 배열의 요소, 맵이나 셋을 구성하는 요소들이 이에 해당한다.

예를 들어보면, 배열에 객체 하나를 추가해 보겠다. 이때 배열이 메모리에 남아있는 한, 배열의 요소인 이 객체도 메모리에 남아있게 되는데 이 객체를 참조하는 것이 아무것도 없더라도 말이다.

아래 코드를 통해 확인해보자.

```javascript
let john = { name: "John" };

let array = [ john ];

john = null; // 참조를 null로 덮어씀

// john을 나타내는 객체는 배열의 요소이기 때문에 가비지 컬렉터의 대상이 되지 않는다.
// array[0]을 이용하면 해당 객체를 얻는 것도 가능하다.
alert(JSON.stringify(array[0]));
```

`맵`에서 객체를 키로 사용한 경우 역시, `맵`이 메모리에 있는 한 객체도 메모리에 남는다. 가비지 컬렉터의 대상이 되지 않는다.



```javascript
let john = { name: "John" };

let map = new Map();
map.set(john, "...");

john = null; // 참조를 null로 덮어씀

// john을 나타내는 객체는 맵 안에 저장되어있다.
// map.keys()를 이용하면 해당 객체를 얻는 것도 가능하다.
for(let obj of map.keys()){
  alert(JSON.stringify(obj));
}

alert(map.size);
```

이런 관점에서 `위크맵(WeakMap)`은 일반 `맵`과 전혀 다른 양상을 보인다. 위크맵을 사용하면 키로 쓰인 객체가 가비지 컬렉션의 대상이 된다.

예시를 이용해 이에 대해 자세히 알아보도록 하자.



#### 위크맵

`맵`과 `위크맵`의 첫 번째 차이는 `위크맵`의 키가 반드시 객체여야 한다는 점입니다. 원시값은 위크맵의 키가 될 수 없다.

```javascript
let weakMap = new WeakMap();

let obj = {};

weakMap.set(obj, "ok"); //정상적으로 동작한다(객체 키).

// 문자열("test")은 키로 사용할 수 없다.
weakMap.set("test", "Whoops"); // Error: Invalid value used as weak map key
```

위크맵의 키로 사용된 객체를 참조하는 것이 아무것도 없다면 해당 객체는 메모리와 위크맵에서 자동으로 삭제된다.

```javascript
let john = { name: "John" };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // 참조를 덮어씀

// john을 나타내는 객체는 이제 메모리에서 지워진다!
```

`john`을 나타내는 객체는 오로지 `위크맵`의 키로만 사용되고 있으므로, 참조를 덮어쓰게 되면 이 객체는 위크맵과 메모리에서 자동으로 삭제된다.

`맵`과 `위크맵`의 두 번째 차이는 `위크맵`은 반복 작업과 `keys()`, `values()`, `entries()` 메서드를 지원하지 않는다는 점이다. 따라서 위크맵에선 키나 값 전체를 얻는 게 불가능하다.

`위크맵`이 지원하는 메서드는 단출하다.

- `weakMap.get(key)`
- `weakMap.set(key, value)`
- `weakMap.delete(key)`
- `weakMap.has(key)`

왜 이렇게 적은 메서드만 제공할까? 원인은 가비지 컬렉션의 동작 방식 때문이다. 위 예시의 `john`을 나타내는 객체처럼, 객체는 모든 참조를 잃게 되면 자동으로 가비지 컬렉션의 대상이 된다. 그런데 가비지 컬렉션의 *동작 시점*은 정확히 알 수 없다.

가비지 컬렉션이 일어나는 시점은 자바스크립트 엔진이 결정한다. 객체는 모든 참조를 잃었을 때, 그 즉시 메모리에서 삭제될 수도 있고, 다른 삭제 작업이 있을 때까지 대기하다가 함께 삭제될 수도 있다. 현재 `위크맵`에 요소가 몇 개 있는지 정확히 파악하는 것 자체가 불가능한 것이다. 가비지 컬렉터가 한 번에 메모리를 청소할 수도 있고, 부분 부분 메모리를 청소할 수도 있으므로 위크맵의 요소(키/값) 전체를 대상으로 무언가를 하는 메서드는 동작 자체가 불가능하다.

그럼 위크맵을 어떤 경우에 사용할 수 있을까?



#### 유스 케이스: 추가 데이터

`위크맵`은 *부차적인 데이터를 저장*할 곳이 필요할 때 그 진가를 발휘한다. 서드파티 라이브러리와 같은 외부 코드에 ‘속한’ 객체를 가지고 작업을 해야 한다고 가정해 보면, 이 객체에 데이터를 추가해줘야 하는데, 추가해 줄 데이터는 객체가 살아있는 동안에만 유효한 상황이다. 이럴 때 `위크맵`을 사용할 수 있다.

`위크맵`에 원하는 데이터를 저장하고, 이때 키는 객체를 사용하면 된다. 이렇게 하면 객체가 가비지 컬렉션의 대상이 될 때, 데이터도 함께 사라지게 된다.

```javascript
weakMap.set(john, "비밀문서");
// john이 사망하면, 비밀문서는 자동으로 파기된다.
```

좀 더 구체적인 예시를 들어보겠다.

아래에 사용자의 방문 횟수를 세어 주는 코드가 있다. 관련 정보는 맵에 저장하고 있는데 맵 요소의 키엔 특정 사용자를 나타내는 객체를, 값엔 해당 사용자의 방문 횟수를 저장하고 있다. 어떤 사용자의 정보를 저장할 필요가 없어지면(가비지 컬렉션의 대상이 되면) 해당 사용자의 방문 횟수도 저장할 필요가 없어질 것이다.

아래 함수는 `맵`을 사용해 사용자의 방문 횟수를 세준다.

```javascript
// 📁 visitsCount.js
let visitsCountMap = new Map(); // 맵에 사용자의 방문 횟수를 저장함

// 사용자가 방문하면 방문 횟수를 늘려준다.
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(user, count + 1);
}
```

아래는 John이라는 사용자가 방문했을 때, 어떻게 방문 횟수가 증가하는지를 보여준다.

```javascript
// 📁 main.js
let john = { name: "John" };

countUser(john); // John의 방문 횟수를 증가시킨다.

// John의 방문 횟수를 셀 필요가 없어지면 아래와 같이 john을 null로 덮어쓴다.
john = null;
```

이제 `john`을 나타내는 객체는 가비지 컬렉션의 대상이 되어야 하는데, `visitsCountMap`의 키로 사용되고 있어서 메모리에서 삭제되지 않는다.

특정 사용자를 나타내는 객체가 메모리에서 사라지면 해당 객체에 대한 정보(방문 횟수)도 우리가 손수 지워줘야 하는 상황이다. 이렇게 하지 않으면 `visitsCountMap`가 차지하는 메모리 공간이 한없이 커질 것이다. 애플리케이션 구조가 복잡할 땐, 이렇게 쓸모 없는 데이터를 수동으로 비워주는 게 꽤 골치 아프다.

이런 문제는 `위크맵`을 사용해 예방할 수 있다.

```javascript
// 📁 visitsCount.js
let visitsCountMap = new WeakMap(); // 위크맵에 사용자의 방문 횟수를 저장함

// 사용자가 방문하면 방문 횟수를 늘려준다.
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(user, count + 1);
}
```

`위크맵`을 사용해 사용자 방문 횟수를 저장하면 `visitsCountMap`을 수동으로 청소해줄 필요가 없다. `john`을 나타내는 객체가 도달 가능하지 않은 상태가 되면 자동으로 메모리에서 삭제되기 때문이다. `위크맵`의 키(`john`)에 대응하는 값(john의 방문 횟수)도 자동으로 가비지 컬렉션의 대상이 된다.



#### 유스 케이스: 캐싱

위크맵은 캐싱(caching)이 필요할 때 유용하다. 캐싱은 시간이 오래 걸리는 작업의 결과를 저장해서 연산 시간과 비용을 절약해주는 기법이다. 동일한 함수를 여러 번 호출해야 할 때, 최초 호출 시 반환된 값을 어딘가에 저장해 놓았다가 그다음엔 함수를 호출하는 대신 저장된 값을 사용하는 게 캐싱의 실례이다.

아래 예시는 함수 연산 결과를 `맵`에 저장하고 있다.

```javascript
// 📁 cache.js
let cache = new Map();

// 연산을 수행하고 그 결과를 맵에 저장한다.
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* 연산 수행 */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 함수 process()를 호출해보자.

// 📁 main.js
let obj = {/* ... 객체 ... */};

let result1 = process(obj); // 함수를 호출한다.

// 동일한 함수를 두 번째 호출할 땐,
let result2 = process(obj); // 연산을 수행할 필요 없이 맵에 저장된 결과를 가져오면 된다.

// 객체가 쓸모없어지면 아래와 같이 null로 덮어쓴다.
obj = null;

alert(cache.size); // 1 (엇! 그런데 객체가 여전히 cache에 남아있고 메모리가 낭비되고 있다.)
```

`process(obj)`를 여러 번 호출하면 최초 호출할 때만 연산이 수행되고, 그 이후엔 연산 결과를 `cache`에서 가져온다. 그런데 `맵`을 사용하고 있어서 객체가 필요 없어져도 `cache`를 수동으로 청소해 줘야 한다.

`맵`을 `위크맵`으로 교체하면 이런 문제를 예방할 수 있다. 객체가 메모리에서 삭제되면, 캐시에 저장된 결과(함수 연산 결과) 역시 메모리에서 자동으로 삭제되기 때문이다.

```javascript
// 📁 cache.js
let cache = new WeakMap();

// 연산을 수행하고 그 결과를 위크맵에 저장한다.
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* 연산 수행 */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 📁 main.js
let obj = {/* ... 객체 ... */};

let result1 = process(obj);
let result2 = process(obj);

// 객체가 쓸모없어지면 아래와 같이 null로 덮어쓴다.
obj = null;

// 이 예시에선 맵을 사용한 예시처럼 cache.size를 사용할 수 없다.
// 하지만 obj가 가비지 컬렉션의 대상이 되므로, 캐싱된 데이터 역시 메모리에서 삭제될 것이다.
// 삭제가 진행되면 cache엔 그 어떤 요소도 남아있지 않을것이다.
```



#### 위크셋

이제 `위크셋(WeakSet)`에 대해 알아보자.

- `위크셋`은 `셋`과 유사한데, 객체만 저장할 수 있다는 점이 다르다. 원시값은 저장할 수 없다.
- 셋 안의 객체는 도달 가능할 때만 메모리에서 유지된다.
- `셋`과 마찬가지로 `위크셋`이 지원하는 메서드는 단출하다. `add`, `has`, `delete`를 사용할 수 있고, `size`, `keys()`나 반복 작업 관련 메서드는 사용할 수 없다.

'위크’맵과 유사하게 '위크’셋도 부차적인 데이터를 저장할 때 사용할 수 있다. 다만, 위크셋엔 위크맵처럼 복잡한 데이터를 저장하지 않는다. 대신 "예"나 “아니오” 같은 간단한 답변을 얻는 용도로 사용된다. 물론 `위크셋`에 저장되는 값들은 객체이다.

예시와 함께 위크셋의 용도를 알아보자. 아래 코드에선 사용자의 사이트 방문 여부를 추적하는 용도로 `위크셋`을 사용하고 있다.

```javascript
let visitedSet = new WeakSet();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

visitedSet.add(john); // John이 사이트를 방문한다.
visitedSet.add(pete); // 이어서 Pete가 사이트를 방문한다.
visitedSet.add(john); // 이어서 John이 다시 사이트를 방문한다.

// visitedSet엔 두 명의 사용자가 저장될 것이다.

// John의 방문 여부를 확인해보자.
alert(visitedSet.has(john)); // true

// Mary의 방문 여부를 확인해보자.
alert(visitedSet.has(mary)); // false

john = null;

// visitedSet에서 john을 나타내는 객체가 자동으로 삭제된다.
```

`위크맵`과 `위크셋`의 가장 큰 단점은 반복 작업이 불가능하다는 점이다. 위크맵이나 위크셋에 저장된 자료를 한 번에 얻는 게 불가능하다. 이런 단점은 불편함을 초래하는 것 같아 보이지만, `위크맵`과 `위크셋`을 이용해 할 수 있는 주요 작업을 방해하진 않는다. `위크맵`과 `위크셋`은 객체와 함께 ‘추가’ 데이터를 저장하는 용도로 쓸 수 있다.





> #### 요약
>
> `위크맵`은 `맵`과 유사한 컬렉션이다. `위크맵`을 구성하는 요소의 키는 오직 객체만 가능하다. 키로 사용된 객체가 메모리에서 삭제되면 이에 대응하는 값 역시 삭제된다.
>
> `위크셋`은 `셋`과 유사한 컬렉션이다. 위크셋엔 객체만 저장할 수 있다. 위크셋에 저장된 객체가 도달 불가능한 상태가 되면 해당 객체는 메모리에서 삭제된다.
>
> 두 자료구조 모두 구성 요소 전체를 대상으로 하는 메서드를 지원하지 않는다. 구성 요소 하나를 대상으로 하는 메서드만 지원한다.
>
> 객체엔 ‘주요’ 자료를, `위크맵`과 `위크셋`엔 ‘부수적인’ 자료를 저장하는 형태로 위크맵과 위크셋을 활용할 수 있다. 객체가 메모리에서 삭제되면, (그리고 오로지 `위크맵`과 `위크셋`의 키만 해당 객체를 참조하고 있다면) 위크맵이나 위크셋에 저장된 연관 자료들 역시 메모리에서 자동으로 삭제된다.





## 5.9 Object.keys, values, entries

개별 자료 구조에서 한발 뒤로 물러나 순회(iteration)에 관해 이야기이다.

이전 챕터에서 우리는 순회에 필요한 메서드 `map.keys()`, `map.values()`, `map.entries()`에 대해 알아보았다.

이 메서드들은 포괄적인 용도로 만들어졌기 때문에 메서드를 적용할 자료구조는 일련의 합의를 준수해야 한다. 커스텀 자료구조를 대상으로 순회를 해야 한다면 이 메서드들을 쓰지 못하고 직접 메서드를 구현해야 한다.

`keys()`, `values()`, `entries()`를 사용할 수 있는 자료구조는 다음과 같다.

- `Map`
- `Set`
- `Array`

일반 객체에도 순회 관련 메서드가 있긴 한데, `keys()`, `values()`, `entries()`와는 문법에 차이가 있다.



#### Object.keys, values, entries

일반 객체엔 다음과 같은 메서드를 사용할 수 있다.

- [Object.keys(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) – 객체의 키만 담은 배열을 반환한다.
- [Object.values(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/values) – 객체의 값만 담은 배열을 반환한다.
- [Object.entries(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) – `[키, 값]` 쌍을 담은 배열을 반환한다.

`Map`, `Set`, `Array` 전용 메서드와 일반 객체용 메서드의 차이를 (맵을 기준으로) 비교하면 다음과 같다.

|           | 맵            | 객체                                  |
| :-------- | :------------ | :------------------------------------ |
| 호출 문법 | `map.keys()`  | `Object.keys(obj)`(`obj.keys()` 아님) |
| 반환 값   | iterable 객체 | ‘진짜’ 배열                           |

첫 번째 차이는 `obj.keys()`가 아닌 `Object.keys(obj)`를 호출한다는 점이다.

이렇게 문법이 다른 이유는 유연성 때문인데 자바스크립트에선 복잡한 자료구조 전체가 객체에 기반한다. 그러다 보니 객체 `data`에 자체적으로 `data.values()`라는 메서드를 구현해 사용하는 경우가 있을 수 있다. 이렇게 커스텀 메서드를 구현한 상태라도 `Object.values(data)`같은 형태로 메서드를 호출할 수 있으면 커스텀 메서드와 내장 메서드 둘 다를 사용할 수 있다.

두 번째 차이는 메서드 `Object.*`를 호출하면 iterable 객체가 아닌 객체의 한 종류인 배열을 반환한다는 점이다. ‘진짜’ 배열을 반환하는 이유는 하위 호환성 때문이다.



```javascript
let user = {
  name: "John",
  age: 30
};
```

- `Object.keys(user) = ["name", "age"]`

- `Object.values(user) = ["John", 30]`

- `Object.entries(user) = [ ["name","John"], ["age",30] ]`

  

아래 예시처럼 `Object.values`를 사용하면 프로퍼티 값을 대상으로 원하는 작업을 할 수 있다.

```javascript
let user = {
  name: "Violet",
  age: 30
};

// 값을 순회합니다.
for (let value of Object.values(user)) {
  alert(value); // Violet과 30이 연속적으로 출력됨
}
```

**Object.keys, values, entries는 심볼형 프로퍼티를 무시한다.**

`for..in` 반복문처럼, Object.keys, Object.values, Object.entries는 키가 심볼형인 프로퍼티를 무시한다.

대개는 심볼형 키를 연산 대상에 포함하지 않는 게 좋지만, 심볼형 키가 필요한 경우엔 심볼형 키만 배열 형태로 반환해주는 메서드인 [Object.getOwnPropertySymbols](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)를 사용하면 된다. `getOwnPropertySymbols` 이외에도 키 *전체*를 배열 형태로 반환하는 메서드인 [Reflect.ownKeys(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)를 사용해도 괜찮다.



#### 객체 변환하기

객체엔 `map`, `filter` 같은 배열 전용 메서드를 사용할 수 없다.

하지만 `Object.entries`와 `Object.fromEntries`를 순차적으로 적용하면 객체에도 배열 전용 메서드 사용할 수 있다. 적용 방법은 다음과 같다.

1. `Object.entries(obj)`를 사용해 객체의 키-값 쌍이 요소인 배열을 얻는다.
2. 1.에서 만든 배열에 `map` 등의 배열 전용 메서드를 적용한다.
3. 2.에서 반환된 배열에 `Object.fromEntries(array)`를 적용해 배열을 다시 객체로 되돌린다.

이 방법을 사용해 가격 정보가 저장된 객체 prices의 프로퍼티 값을 두 배로 늘려보도록 하자.

```javascript
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // 객체를 배열로 변환해서 배열 전용 메서드인 map을 적용하고 fromEntries를 사용해 배열을 다시 객체로 되돌린다.
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);

alert(doublePrices.meat); // 8
```

지금 당장은 어렵게 느껴지겠지만 한두 번 위 방법을 적용해 보면 객체에 배열 전용 메서드를 적용하는게 익숙해질 것이다.





## 5.10 구조 분해 할당

`객체`와 `배열`은 자바스크립트에서 가장 많이 쓰이는 자료 구조이다.

키를 가진 데이터 여러 개를 하나의 엔티티에 저장할 땐 객체를, 컬렉션에 데이터를 순서대로 저장할 땐 배열을 사용한다.

개발을 하다 보면 함수에 객체나 배열을 전달해야 하는 경우 또, 가끔은 객체나 배열에 저장된 데이터 전체가 아닌 일부만 필요한 경우가 생기기도 한다.

이럴 때 객체나 배열을 변수로 '분해’할 수 있게 해주는 특별한 문법인 *구조 분해 할당(destructuring assignment)* 을 사용할 수 있다. 이 외에도 함수의 매개변수가 많거나 매개변수 기본값이 필요한 경우 등에서 구조 분해(destructuring)는 그 진가를 발휘한다.





#### 배열 분해하기

배열이 어떻게 변수로 분해되는지 예제를 통해 살펴보자.

```javascript
// 이름과 성을 요소로 가진 배열
let arr = ["Bora", "Lee"]

// 구조 분해 할당을 이용해
// firstName엔 arr[0]을
// surname엔 arr[1]을 할당하였다.
let [firstName, surname] = arr;

alert(firstName); // Bora
alert(surname);  // Lee
```

이제 인덱스를 이용해 배열에 접근하지 않고도 변수로 이름과 성을 사용할 수 있게 되었다.

아래 예시처럼 `split` 같은 반환 값이 배열인 메서드를 함께 활용해도 좋다.

```javascript
let [firstName, surname] = "Bora Lee".split(' ');
```

**'분해(destructuring)'는 '파괴(destructive)'를 의미하지 않는다.**

구조 분해 할당이란 명칭은 어떤 것을 복사한 이후에 변수로 '분해(destructurize)'해준다는 의미 때문에 붙여졌다. 이 과정에서 분해 대상은 수정 또는 파괴되지 않는다.

배열의 요소를 직접 변수에 할당하는 것보다 코드 양이 줄어든다는 점만 다르다.

```javascript
// let [firstName, surname] = arr;
let firstName = arr[0];
let surname = arr[1];
```

**쉼표를 사용하여 요소 무시하기**

쉼표를 사용하면 필요하지 않은 배열 요소를 버릴 수 있다.

```javascript
// 두 번째 요소는 필요하지 않음
let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

alert( title ); // Consul
```

두 번째 요소는 생략되었지만, 세 번째 요소는 `title`이라는 변수에 할당된 것을 확인할 수 있다. 할당할 변수가 없기 때문에 네 번째 요소 역시 생략되었다.

**할당 연산자 우측엔 모든 이터러블이 올 수 있다.**

배열뿐만 아니라 모든 이터러블(iterable, 반복 가능한 객체)에 구조 분해 할당을 적용할 수 있다.

```javascript
let [a, b, c] = "abc"; // ["a", "b", "c"]
let [one, two, three] = new Set([1, 2, 3]);
```

**할당 연산자 좌측엔 뭐든지 올 수 있다.**

할당 연산자 좌측엔 ‘할당할 수 있는(assignables)’ 것이라면 어떤 것이든 올 수 있다.

아래와 같이 객체 프로퍼티도 가능하다.

```javascript
let user = {};
[user.name, user.surname] = "Bora Lee".split(' ');

alert(user.name); // Bora
```

**.entries()로 반복하기**

[Object.entries(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)는 이전 챕터에서 학습한 바 있다.

이 메서드와 구조 분해를 조합하면 객체의 키와 값을 순회해 변수로 분해 할당할 수 있다.

```javascript
let user = {
  name: "John",
  age: 30
};

// 객체의 키와 값 순회하기
for (let [key, value] of Object.entries(user)) {
  alert(`${key}:${value}`); // name:John, age:30이 차례대로 출력
}
```

맵에도 물론 이 메서드를 활용할 수 있다.

```javascript
let user = new Map();
user.set("name", "John");
user.set("age", "30");

for (let [key, value] of user) {
  alert(`${key}:${value}`); // name:John, then age:30
}
```

**변수 교환 트릭**

두 변수에 저장된 값을 교환할 때 구조 분해 할당을 사용할 수 있다.

```javascript
let guest = "Jane";
let admin = "Pete";

// 변수 guest엔 Pete, 변수 admin엔 Jane이 저장되도록 값을 교환함
[guest, admin] = [admin, guest];

alert(`${guest} ${admin}`); // Pete Jane(값 교환이 성공적으로 이뤄졌다!)
```

예시에선 임시 배열을 만들어 두 변수를 담고, 요소 순서를 교체해 배열을 분해하는 방식을 사용했다.

이 방식을 사용하면 두 개뿐만 아니라 그 이상의 변수에 담긴 값도 교환할 수 있다.





#### '…'로 나머지 요소 가져오기

배열 앞쪽에 위치한 값 몇 개만 필요하고 그 이후 이어지는 나머지 값들은 한데 모아서 저장하고 싶을 때가 있다. 이럴 때는 점 세 개 `...`를 붙인 매개변수 하나를 추가하면 ‘나머지(rest)’ 요소를 가져올 수 있다.

```javascript
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

alert(name1); // Julius
alert(name2); // Caesar

// `rest`는 배열이다.
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
```

`rest`는 나머지 배열 요소들이 저장된 새로운 배열이 된다. `rest` 대신에 다른 이름을 사용해도 되는데, 변수 앞의 점 세 개(`...`)와 변수가 가장 마지막에 위치해야 한다는 점은 지켜주시기 바란다.

### 

#### 기본값

할당하고자 하는 변수의 개수가 분해하고자 하는 배열의 길이보다 크더라도 에러가 발생하지 않는다. 할당할 값이 없으면 undefined로 취급되기 때문이다.

```javascript
let [firstName, surname] = [];

alert(firstName); // undefined
alert(surname); // undefined
```

`=`을 이용하면 할당할 값이 없을 때 기본으로 할당해 줄 값인 '기본값(default value)'을 설정할 수 있다.

```javascript
// 기본값
let [name = "Guest", surname = "Anonymous"] = ["Julius"];

alert(name);    // Julius (배열에서 받아온 값)
alert(surname); // Anonymous (기본값)
```

복잡한 표현식이나 함수 호출도 기본값이 될 수 있다. 이렇게 기본식으로 표현식이나 함수를 설정하면 할당할 값이 없을 때 표현식이 평가되거나 함수가 호출된다.

기본값으로 두 개의 `prompt` 함수를 할당한 예시를 살펴보자. 값이 제공되지 않았을 때만 함수가 호출되므로, `prompt`는 한 번만 호출된다.

```javascript
// name의 prompt만 실행됨
let [surname = prompt('성을 입력하세요.'), name = prompt('이름을 입력하세요.')] = ["김"];

alert(surname); // 김 (배열에서 받아온 값)
alert(name);    // prompt에서 받아온 값
```





#### 객체 분해하기

구조 분해 할당으로 객체도 분해할 수 있다.

기본 문법은 다음과 같다.

```javascript
let {var1, var2} = {var1:…, var2:…}
```

할당 연산자 우측엔 분해하고자 하는 객체를, 좌측엔 상응하는 객체 프로퍼티의 '패턴’을 넣는다. 분해하려는 객체 프로퍼티의 키 목록을 패턴으로 사용하는 예시를 살펴보자.



```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

let {title, width, height} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
```

프로퍼티 `options.title`과 `options.width`, `options.height`에 저장된 값이 상응하는 변수에 할당된 것을 확인할 수 있다. 참고로 순서는 중요하지 않다. 아래와 같이 작성해도 위 예시와 동일하게 동작한다.

```javascript
// let {...} 안의 순서가 바뀌어도 동일하게 동작함
let {height, width, title} = { title: "Menu", height: 200, width: 100 }
```

할당 연산자 좌측엔 좀 더 복잡한 패턴이 올 수도 있다. 분해하려는 객체의 프로퍼티와 변수의 연결을 원하는 대로 조정할 수도 있다.

객체 프로퍼티를 프로퍼티 키와 다른 이름을 가진 변수에 저장해보자. `options.width`를 `w`라는 변수에 저장하는 식으로 말이다. 좌측 패턴에 콜론(:)을 사용하면 원하는 목표를 달성할 수 있다.

```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

// { 객체 프로퍼티: 목표 변수 }
let {width: w, height: h, title} = options;

// width -> w
// height -> h
// title -> title

alert(title);  // Menu
alert(w);      // 100
alert(h);      // 200
```

콜론은 '분해하려는 객체의 프로퍼티: 목표 변수’와 같은 형태로 사용한다. 위 예시에선 프로퍼티 `width`를 변수 `w`에, 프로퍼티 `height`를 변수 `h`에 저장했다. 프로퍼티 `title`은 동일한 이름을 가진 변수 `title`에 저장된다.

프로퍼티가 없는 경우를 대비하여 `=`을 사용해 기본값을 설정하는 것도 아래와 같이 가능하다. 

```javascript
let options = {
  title: "Menu"
};

let {width = 100, height = 200, title} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
```

배열 혹은 함수의 매개변수에서 했던 것처럼 객체에도 표현식이나 함수 호출을 기본값으로 할당할 수 있다. 물론 표현식이나 함수는 값이 제공되지 않았을 때 평가 혹은 실행된다.

아래 예시를 실행하면 width 값만 물어보고 title 값은 물어보지 않는다.

```javascript
let options = {
  title: "Menu"
};

let {width = prompt("width?"), title = prompt("title?")} = options;

alert(title);  // Menu
alert(width);  // prompt 창에 입력한 값
```

콜론과 할당 연산자를 동시에 사용할 수도 있다.

```javascript
let options = {
  title: "Menu"
};

let {width: w = 100, height: h = 200, title} = options;

alert(title);  // Menu
alert(w);      // 100
alert(h);      // 200
```

프로퍼티가 많은 복잡한 객체에서 원하는 정보만 뽑아오는 것도 가능하다.

```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};

// title만 변수로 뽑아내기
let { title } = options;

alert(title); // Menu
```





#### 나머지 패턴 ‘…’

분해하려는 객체의 프로퍼티 개수가 할당하려는 변수의 개수보다 많다면 어떨까? '나머지’를 어딘가에 할당하면 되지 않겠냐는 생각이 든다.

나머지 패턴(rest pattern)을 사용하면 배열에서 했던 것처럼 나머지 프로퍼티를 어딘가에 할당하는 게 가능하다. 참고로 모던 브라우저는 나머지 패턴을 지원하지만, 물론 바벨(Babel)을 이용하면 되지만 IE를 비롯한 몇몇 구식 브라우저는 나머지 패턴을 지원하지 않으므로 주의해서 사용해야 한다. 



```javascript
let options = {
  title: "Menu",
  height: 200,
  width: 100
};

// title = 이름이 title인 프로퍼티
// rest = 나머지 프로퍼티들
let {title, ...rest} = options;

// title엔 "Menu", rest엔 {height: 200, width: 100}이 할당된다.
alert(rest.height);  // 200
alert(rest.width);   // 100
```

**`let` 없이 사용하기**

지금까진 할당 연산 `let {…} = {…}` 안에서 변수들을 선언하였다. `let`으로 새로운 변수를 선언하지 않고 기존에 있던 변수에 분해한 값을 할당할 수도 있는데, 이때는 주의할 점이 있다.

잘못된 코드:

```javascript
let title, width, height;

// SyntaxError: Unexpected token '=' 이라는 에러가 아랫줄에서 발생한다.
{title, width, height} = {title: "Menu", width: 200, height: 100};
```

자바스크립트는 표현식 안에 있지 않으면서 주요 코드 흐름 상에 있는 `{...}`를 코드 블록으로 인식한다. 코드 블록의 본래 용도는 아래와 같이 문(statement)을 묶는 것이다.

```javascript
{
  // 코드 블록
  let message = "Hello";
  // ...
  alert( message );
}
```

위쪽 예시에선 구조 분해 할당을 위해 사용한 `{...}`를 자바스크립트가 코드 블록으로 인식해서 에러가 발생하였다.

에러를 해결하려면 할당문을 괄호`(...)`로 감싸 자바스크립트가 `{...}`를 코드 블록이 아닌 표현식으로 해석면 된다.

```javascript
let title, width, height;

// 에러가 발생하지 않는다.
({title, width, height} = {title: "Menu", width: 200, height: 100});

alert( title ); // Menu
```





#### 중첩 구조 분해

객체나 배열이 다른 객체나 배열을 포함하는 경우, 좀 더 복잡한 패턴을 사용하면 중첩 배열이나 객체의 정보를 추출할 수 있다. 이를 중첩 구조 분해(nested destructuring)라고 부른다.

아래 예시에서 객체 `options`의 `size` 프로퍼티 값은 또 다른 객체이다. `items` 프로퍼티는 배열을 값으로 가지고 있다. 대입 연산자 좌측의 패턴은 정보를 추출하려는 객체 `options`와 같은 구조를 갖추고 있다.

```javascript
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true
};

// 코드를 여러 줄에 걸쳐 작성해 의도하는 바를 명확히 드러냄
let {
  size: { // size는 여기,
    width,
    height
  },
  items: [item1, item2], // items는 여기에 할당함
  title = "Menu" // 분해하려는 객체에 title 프로퍼티가 없으므로 기본값을 사용함
} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
alert(item1);  // Cake
alert(item2);  // Donut
```

`extra`(할당 연산자 좌측의 패턴에는 없음)를 제외한 `options` 객체의 모든 프로퍼티가 상응하는 변수에 할당되었다.

변수 `width`, `height`, `item1`, `item2`엔 원하는 값이, `title`엔 기본값이 저장되었다.

그런데 위 예시에서 `size`와 `items` 전용 변수는 없다는 점에 유의하자. 전용 변수 대신 우리는 `size`와 `items` 안의 정보를 변수에 할당하였다.



#### 똑똑한 함수 매개변수

함수에 매개변수가 많은데 이중 상당수는 선택적으로 쓰이는 경우가 종종 있다. 사용자 인터페이스와 연관된 함수에서 이런 상황을 자주 볼 수 있는데 메뉴 생성에 관여하는 함수가 있다고 해 보자. 메뉴엔 너비, 높이, 제목, 항목 리스트 등이 필요하기 때문에 이 정보는 매개변수로 받는다.

먼저 리팩토링 전의 메뉴 생성 함수를 살펴보겠다.

```javascript
function showMenu(title = "Untitled", width = 200, height = 100, items = []) {
  // ...
}
```

문서화가 잘 되어있다면 IDE가 순서를 틀리지 않게 도움을 주긴 하겠지만 이렇게 함수를 작성하면 넘겨주는 인수의 순서가 틀려 문제가 발생할 수 있다.  이 외에도 대부분의 매개변수에 기본값이 설정되어 있어 굳이 인수를 넘겨주지 않아도 되는 경우에 문제가 발생한다.

아래 코드를 살펴보면 어떤 생각이 들까?

```javascript
// 기본값을 사용해도 괜찮은 경우 아래와 같이 undefined를 여러 개 넘겨줘야 한다.
showMenu("My Menu", undefined, undefined, ["Item1", "Item2"])
```

꽤 지저분해 보이네요. 매개변수가 많아질수록 가독성은 더 떨어질 것이다.

구조 분해는 이럴 때 엄청 유용한데, 매개변수 모두를 객체에 모아 함수에 전달해, 함수가 전달받은 객체를 분해하여 변수에 할당하고 원하는 작업을 수행할 수 있도록 함수를 리팩토링해 보자..

```javascript
// 함수에 전달할 객체
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

// 똑똑한 함수는 전달받은 객체를 분해해 변수에 즉시 할당함
function showMenu({title = "Untitled", width = 200, height = 100, items = []}) {
  // title, items – 객체 options에서 가져옴
  // width, height – 기본값
  alert( `${title} ${width} ${height}` ); // My Menu 200 100
  alert( items ); // Item1, Item2
}

showMenu(options);
```

중첩 객체와 콜론을 조합하면 좀 더 복잡한 구조 분해도 가능하다.

```javascript
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

function showMenu({
  title = "Untitled",
  width: w = 100,  // width는 w에,
  height: h = 200, // height는 h에,
  items: [item1, item2] // items의 첫 번째 요소는 item1에, 두 번째 요소는 item2에 할당함
}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```

이렇게 똑똑한 함수 매개변수 문법은 구조 분해 할당 문법과 동일하다.

```javascript
function({
  incomingProperty: varName = defaultValue
  ...
})
```

매개변수로 전달된 객체의 프로퍼티 `incomingProperty`는 `varName`에 할당되겠죠. 만약 값이 없다면 `defaultValue`가 기본값으로 사용될 것이다.

참고로 이렇게 함수 매개변수를 구조 분해할 땐, 반드시 인수가 전달된다고 가정되고 사용된다는 점에 유의하시기 바란다. 모든 인수에 기본값을 할당해 주려면 빈 객체를 명시적으로 전달해야 한다.

```javascript
showMenu({}); // 모든 인수에 기본값이 할당된다.

showMenu(); // 에러가 발생할 수 있다.
```

이 문제를 예방하려면 빈 객체 `{}`를 인수 전체의 기본값으로 만들면 된다.

```javascript
function showMenu({ title = "Menu", width = 100, height = 200 } = {}) {
  alert( `${title} ${width} ${height}` );
}

showMenu(); // Menu 100 200
```

이렇게 인수 객체의 기본값을 빈 객체 `{}`로 설정하면 어떤 경우든 분해할 것이 생겨서 함수에 인수를 하나도 전달하지 않아도 에러가 발생하지 않는다.



> #### 요약
>
> - 구조 분해 할당을 사용하면 객체나 배열을 변수로 연결할 수 있다.
>
> - 객체 분해하기:
>
>   ```javascript
>   let {prop : varName = default, ...rest} = object
>   ```
>
>   object의 프로퍼티 `prop`의 값은 변수 `varName`에 할당되는데, object에 prop이 없으면 `default`가 `varName`에 할당된다.
>
>   연결할 변수가 없는 나머지 프로퍼티들은 객체 `rest`에 복사된다.
>
> - 배열 분해하기:
>
>   ```javascript
>   let [item1 = default, item2, ...rest] = array
>   ```
>
>   array의 첫 번째 요소는 `item1`에, 두 번째 요소는 변수 `item2`에 할당되고, 이어지는 나머지 요소들은 배열 `rest` 저장된다.
>
> - 할당 연산자 좌측의 패턴과 우측의 구조가 같으면 중첩 배열이나 객체가 있는 복잡한 구조에서도 원하는 데이터를 뽑아낼 수 있다.







## 5.11 Date 객체와 날짜

날짜를 저장할 수 있고, 날짜와 관련된 메서드도 제공해주는 내장 객체 [Date](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date)에 대해 알아보자. Date 객체를 활용하면 생성 및 수정 시간을 저장하거나 시간을 측정할 수 있고, 현재 날짜를 출력하는 용도 등으로도 활용할 수 있다.





#### 객체 생성하기

`new Date()`를 호출하면 새로운 `Date` 객체가 만들어지는데, `new Date()`는 다음과 같은 형태로 호출할 수 있다.

- `new Date()`

  인수 없이 호출하면 현재 날짜와 시간이 저장된 `Date` 객체가 반환된다.`let now = new Date(); alert( now ); // 현재 날짜 및 시간이 출력됨`

  

- `new Date(milliseconds)`

  UTC 기준(UTC+0) 1970년 1월 1일 0시 0분 0초에서 `milliseconds` 밀리초(1/1000 초) 후의 시점이 저장된 `Date` 객체가 반환된다.

  ```javascript
  // 1970년 1월 1일 0시 0분 0초(UTC+0)를 나타내는 객체
  let Jan01_1970 = new Date(0);
  alert( Jan01_1970 );
  
  // 1970년 1월 1일의 24시간 후는 1970년 1월 2일(UTC+0)임
  let Jan02_1970 = new Date(24 * 3600 * 1000);
  alert( Jan02_1970 );
  ```

  `1970년의 첫날을 기준으로 흘러간 밀리초를 나타내는 정수는 *타임스탬프(timestamp)* 라고 부른다. 타임스탬프를 사용하면 날짜를 숫자 형태로 간편하게 나타낼 수 있다. `new Date(timestamp)`를 사용해 타임스탬프를 사용해 특정 날짜가 저장된 `Date` 객체를 손쉽게 만들 수 있고`date.getTime()` 메서드를 사용해 `Date` 객체에서 타임스탬프를 추출하는 것도 가능하다.

  1970년 1월 1일 이전 날짜에 해당하는 타임스탬프 값은 음수이다. 예시를 살펴보면,

  ```javascript
  // 31 Dec 1969
  let Dec31_1969 = new Date(-24 * 3600 * 1000);
  alert( Dec31_1969 );
  ```

  

- `new Date(datestring)`

  인수가 하나인데, 문자열이라면 해당 문자열은 자동으로 구문 분석(parsed)된다. 구문 분석에 적용되는 알고리즘은 `Date.parse`에서 사용하는 알고리즘과 동일하다.

  ```javascript
  let date = new Date("2017-01-26");
  alert(date);
  // 인수로 시간은 지정하지 않았기 때문에 GMT 자정이라고 가정하고
  // 코드가 실행되는 시간대(timezone)에 따라 출력 문자열이 바뀝니다.
  // 따라서 얼럿 창엔
  // Thu Jan 26 2017 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
  // 혹은
  // Wed Jan 25 2017 16:00:00 GMT-0800 (Pacific Standard Time)등이 출력됩니다.
  ```

  

- `new Date(year, month, date, hours, minutes, seconds, ms)`

  주어진 인수를 조합해 만들 수 있는 날짜가 저장된 객체가 반환된다(지역 시간대 기준). 첫 번째와 두 번째 인수만 필수값이다.

  

  *  `year`는 반드시 네 자리 숫자여야 한다. `2013`은 괜찮고 `98`은 괜찮지 않다.

  * `month`는 `0`(1월)부터 `11`(12월) 사이의 숫자여야 한다.

  * `date`는 일을 나타내는데, 값이 없는 경우엔 1일로 처리된다.

  * `hours/minutes/seconds/ms`에 값이 없는 경우엔 `0`으로 처리된다. 

  

  ```javascript
  new Date(2011, 0, 1, 0, 0, 0, 0); // 2011년 1월 1일, 00시 00분 00초
  new Date(2011, 0, 1); // hours를 비롯한 인수는 기본값이 0이므로 위와 동일
  ```

  최소 정밀도는 1밀리초(1/1000초)이다.

  ```javascript
  new Date(2011, 0, 1, 0, 0, 0, 0); // 2011년 1월 1일, 00시 00분 00초
  new Date(2011, 0, 1); // hours를 비롯한 인수는 기본값이 0이므로 위와 동일
  ```

  

  #### 날짜 구성요소 얻기

  `Date` 객체의 메서드를 사용하면 연, 월, 일 등의 값을 얻을 수 있다.

- [getFullYear()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getFullYear)

  연도(네 자릿수)를 반환한다.

- [getMonth()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth)

  월을 반환한다(**0 이상 11 이하**).

- [getDate()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)

  일을 반환한다(1 이상 31 이하)

- [getHours()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getHours), [getMinutes()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getMinutes), [getSeconds()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getSeconds), [getMilliseconds()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getMilliseconds)

  시, 분, 초, 밀리초를 반환한다.

**`getYear()`말고 `getFullYear()`를 사용하자.**

여러 자바스크립트 엔진이 더는 사용되지 않는(deprecated) 비표준 메서드 `getYear()`을 구현하고 있다.  **이 메서드는 두 자릿수 연도를 반환하는 경우가 있기 때문에 절대 사용해선 안 된다.** 연도 정보를 얻고 싶다면 `getFullYear()`를 사용하자.

이 외에도 요일을 반환해주는 메서드도 있다.

- [getDay()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)

  일요일을 나타내는 `0`부터 토요일을 나타내는 `6`까지의 숫자 중 하나를 반환한다. 몇몇 나라에서 요일의 첫날이 일요일이 아니긴 하지만, getDay에선 항상 `0`이 일요일을 나타냅니다. 이를 변경할 방법은 아쉽게도 없다.

**위에서 소개해드린 메서드 모두는 현지 시간 기준 날짜 구성요소를 반환한다.**

위 메서드 이름에 있는 ‘get’ 다음에 'UTC’를 붙여주면 표준시(UTC+0) 기준의 날짜 구성 요소를 반환해주는 메서드 [getUTCFullYear()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCFullYear), [getUTCMonth()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCMonth), [getUTCDay()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCDay)를 만들 수 있다.

현지 시간대가 UTC 시간대와 다르다면 아래 예시를 실행했을 때 얼럿창엔 다른 값이 출력된다.

```javascript
// 현재 일시
let date = new Date();

// 현지 시간 기준 시
alert( date.getHours() );

// 표준시간대(UTC+0, 일광 절약 시간제를 적용하지 않은 런던 시간) 기준 시
alert( date.getUTCHours() );
```

아래 두 메서드는 위에서 소개한 메서드와 달리 표준시(UTC+0) 기준의 날짜 구성 요소를 반환해주는 메서드가 없다.

- [getTime()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)

  주어진 일시와 1970년 1월 1일 00시 00분 00초 사이의 간격(밀리초 단위)인 타임스탬프를 반환한다.

- [getTimezoneOffset()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset)

  현지 시간과 표준 시간의 차이(분)를 반환한다.`// UTC-1 시간대에서 이 예시를 실행하면 60이 출력된다. // UTC+3 시간대에서 이 예시를 실행하면 -180이 출력된다. alert( new Date().getTimezoneOffset() );`



#### 날짜 구성요소 설정하기

아래 메서드를 사용하면 날짜 구성요소를 설정할 수 있다.

- [`setFullYear(year, [month\], [date])`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setFullYear)
- [`setMonth(month, [date\])`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setMonth)
- [`setDate(date)`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setDate)
- [`setHours(hour, [min\], [sec], [ms])`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setHours)
- [`setMinutes(min, [sec\], [ms])`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setMinutes)
- [`setSeconds(sec, [ms\])`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setSeconds)
- [`setMilliseconds(ms)`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setMilliseconds)
- [`setTime(milliseconds)`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/setTime) (1970년 1월 1일 00:00:00 UTC부터 밀리초 이후를 나타내는 날짜를 설정)

`setTime()`을 제외한 모든 메서드는 `setUTCHours()`같이 표준시에 따라 날짜 구성 요소를 설정해주는 메서드가 있다.

`setHours`와 같은 메서드는 여러 개의 날짜 구성요소를 동시에 설정할 수 있는데, 메서드의 인수에 없는 구성요소는 변경되지 않다.

```javascript
let today = new Date();

today.setHours(0);
alert(today); // 날짜는 변경되지 않고 시만 0으로 변경된다.

today.setHours(0, 0, 0, 0);
alert(today); // 날짜는 변경되지 않고 시, 분, 초가 모두 변경된다(00시 00분 00초).
```



#### 자동 고침

`Date` 객체엔 *자동 고침(autocorrection)* 이라는 유용한 기능이 있다. 범위를 벗어나는 값을 설정하려고 하면 자동 고침 기능이 활성화되면서 값이 자동으로 수정된다.



```javascript
let date = new Date(2013, 0, 32); // 2013년 1월 32일은 없다.
alert(date); // 2013년 2월 1일이 출력된다.
```

입력받은 날짜 구성 요소가 범위를 벗어나면 초과분은 자동으로 다른 날짜 구성요소에 배분된다.

'2016년 2월 28일’의 이틀 뒤 날짜를 구하고 싶다고 가정해면, 답은 3월 2일 혹은 3월 1일(윤년)이 될 텐데, 2016년이 윤년인지 아닌지 생각할 필요 없이 단순히 이틀을 더해주기만 하면 답을 구할 수 있다. 나머지 작업은 `Date` 객체가 알아서 처리해 주기 때문이다.

```javascript
let date = new Date(2016, 1, 28);
date.setDate(date.getDate() + 2);

alert( date ); // 2016년 3월 1일
```

자동 고침은 일정 시간이 지난 후의 날짜를 구하는데도 종종 사용된다. '지금부터 70초 후’의 날짜를 구해보자.

```javascript
let date = new Date();
date.setSeconds(date.getSeconds() + 70);

alert( date ); // 70초 후의 날짜가 출력된다.
```

0이나 음수를 날짜 구성요소에 설정하는 것도 가능하다. 

```javascript
let date = new Date(2016, 0, 2); // 2016년 1월 2일

date.setDate(1); // 1일로 변경한다.
alert( date ); // 01 Jan 2016

date.setDate(0); // 일의 최솟값은 1이므로 0을 입력하면 전 달의 마지막 날을 설정한 것과 같은 효과를 본다.
alert( date ); // 31 Dec 2015
```



#### Date 객체를 숫자로 변경해 시간차 측정하기

`Date` 객체를 숫자형으로 변경하면 타임스탬프(`date.getTime()`을 호출 시 반환되는 값)가 된다.

```javascript
let date = new Date();
alert(+date); // 타임스탬프(date.getTime()를 호출한 것과 동일함)
```

이를 응용하면 날짜에 마이너스 연산자를 적용해 밀리초 기준 시차를 구할 수 있다.

시차를 측정해 나만의 스톱워치를 만들어보자.

```javascript
let start = new Date(); // 측정 시작

// 원하는 작업을 수행
for (let i = 0; i < 100000; i++) {
  let doSomething = i * i * i;
}

let end = new Date(); // 측정 종료

alert( `반복문을 모두 도는데 ${end - start} 밀리초가 걸렸습니다.` );
```



#### Date.now()

`Date` 객체를 만들지 않고도 시차를 측정할 방법이 있다.

현재 타임스탬프를 반환하는 메서드 `Date.now()`를 응용하면 된다.

`Date.now()`는 `new Date().getTime()`과 의미론적으로 동일하지만 중간에 `Date` 객체를 만들지 않는다는 점이 다르다. 따라서 `new Date().getTime()`를 사용하는 것보다 빠르고 가비지 컬렉터의 일을 덜어준다는 장점이 있다.

자바스크립트를 사용해 게임을 구현한다던가 전문분야의 애플리케이션을 구현할 때와 같이 성능이 중요한 경우에 `Date.now()`가 자주 활용된다. 물론 편의를 위해 활용되기도 한다.

위 예시를 `Date.now()`를 사용해 변경하면 성능이 더 좋다.

```javascript
let start = Date.now(); // 1970년 1월 1일부터 현재까지의 밀리초

// 원하는 작업을 수행
for (let i = 0; i < 100000; i++) {
  let doSomething = i * i * i;
}

let end = Date.now(); // done

alert( `반복문을 모두 도는데 ${end - start} 밀리초가 걸렸습니다.` ); // Date 객체가 아닌 숫자끼리 차감함
```





#### 벤치마크 테스트

'벤치마크 테스트’는 비교 대상을 두고 성능을 비교하여 시험하고 평가할 때 쓰인다.

CPU를 많이 잡아먹는 함수의 신뢰할만한 벤치마크(평가 기준)를 구하려면 상당한 주의가 필요하다.

두 날짜의 차이를 계산해주는 함수 두 개가 있는데, 어느 함수의 성능이 더 좋은지 알아내야 한다고 가정해보자.

```javascript
// 두 함수 중 date1과 date2의 차이를 어떤 함수가 더 빨리 반환할까?
function diffSubtract(date1, date2) {
  return date2 - date1;
}

// 반환 값은 밀리초이다.
function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime();
}
```

두 함수는 완전히 동일한 작업을 수행하지만, 한 함수는 날짜를 밀리초 단위로 얻기 위해 `date.getTime()`를 사용하고 있고, 다른 함수는 마이너스 연산자 적용 시 객체가 숫자형으로 변화한다는 특징을 사용하고 있다. 두 함수가 반환하는 값은 항상 동일하다.

속도는 어떨까?

연속해서 함수를 아주 많이 호출한 후, 실제 연산이 종료되는 데 걸리는 시간을 비교하면 두 함수의 성능을 비교할 수 있을 것이다. `diffSubtract`와 `diffGetTime`는 아주 간단한 함수이기 때문에 유의미한 시차를 구하려면 각 함수를 최소한 십만 번 호출해야 한다.



```javascript
function diffSubtract(date1, date2) {
  return date2 - date1;
}

function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime();
}

function bench(f) {
  let date1 = new Date(0);
  let date2 = new Date();

  let start = Date.now();
  for (let i = 0; i < 100000; i++) f(date1, date2);
  return Date.now() - start;
}

alert( 'diffSubtract를 십만번 호출하는데 걸린 시간: ' + bench(diffSubtract) + 'ms' );
alert( 'diffGetTime을 십만번 호출하는데 걸린 시간: ' + bench(diffGetTime) + 'ms' );
```

형 변환이 없어서 엔진 최적화에 드는 자원이 줄어들므로 `getTime()`을 이용한 방법이 훨씬 빠르다.

자, 벤치마크로 쓸 뭔가를 구하긴 했습니다. 그런데 이 벤치마크는 그다지 좋은 벤치마크가 아니다.

`bench(diffSubtract)`를 실행하고 있을 때 CPU가 어떤 작업을 병렬적으로 처리하고 있고, 여기에 CPU의 자원이 투입되고 있었다고 가정해 보자. 그리고 `bench(diffGetTime)`을 실행할 땐, 이 작업이 끝난 상태라고 가정해 보자.

멀티 프로세스를 지원하는 운영체제에서 이런 시나리오는 흔히 발생한다.

첫 번째 `benchmark`가 실행될 땐 사용할 수 있는 CPU 자원이 적었기 때문에 이 벤치마크는 좋지 않다.

**좀 더 신뢰할만한 벤치마크 테스트를 만들려면 `benchmark`를 번갈아 가면서 여러 번 돌려야 한다.**

```javascript
function diffSubtract(date1, date2) {
  return date2 - date1;
}

function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime();
}

function bench(f) {
  let date1 = new Date(0);
  let date2 = new Date();

  let start = Date.now();
  for (let i = 0; i < 100000; i++) f(date1, date2);
  return Date.now() - start;
}

let time1 = 0;
let time2 = 0;

// 함수 bench를 각 함수(diffSubtract, diffGetTime)별로 10번씩 돌린다.
for (let i = 0; i < 10; i++) {
  time1 += bench(diffSubtract);
  time2 += bench(diffGetTime);
}

alert( 'diffSubtract에 소모된 시간: ' + time1 );
alert( 'diffGetTime에 소모된 시간: ' + time2 );
```

모던 자바스크립트 엔진은 아주 많이 실행된 코드인 'hot code’를 대상으로 최적화를 수행한다(실행 횟수가 적은 코드는 최적화할 필요가 없다). 위 예시에서 bench를 첫 번째 실행했을 때는 최적화가 잘 적용되지 않기 때문에 아래 코드처럼 메인 반복문을 실행하기 전에 예열용(heat-up)으로 bench를 실행할 수 있다.

```javascript
// 메인 반복문 실행 전, "예열용"으로 추가한 코드
bench(diffSubtract);
bench(diffGetTime);

// 벤치마크 테스트 시작
for (let i = 0; i < 10; i++) {
  time1 += bench(diffSubtract);
  time2 += bench(diffGetTime);
}
```

**세밀한 벤치마킹을 할 때는 주의하자**

모던 자바스크립트 엔진은 최적화를 많이 한다. 이로 인해 '만들어진 테스트’가 '실제 사례’와는 결과가 다를 수 있다. 특히 연산자, 내장 함수와 같이 아주 작은 것일수록 더 결과가 다를 수 있다. 그러니 진지하게 성능을 이해하고 싶다면 자바스크립트 엔진이 어떻게 동작하는지 공부하면 아마 세밀한 벤치마킹을 할 필요가 없을 것이다.

[http://mrale.ph](http://mrale.ph/)에서 V8 엔진을 설명한 좋은 글들을 보실 수 있다.



#### Date.parse와 문자열

메서드 [Date.parse(str)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)를 사용하면 문자열에서 날짜를 읽어올 수 있다. 단, 문자열의 형식은 `YYYY-MM-DDTHH:mm:ss.sssZ`처럼 생겨야 한다.

- `YYYY-MM-DD` – 날짜(연-월-일)
- `"T"` – 구분 기호로 쓰임
- `HH:mm:ss.sss` – 시:분:초.밀리초
- `'Z'`(옵션) – `+-hh:mm` 형식의 시간대를 나타냄. `Z` 한 글자인 경우엔 UTC+0을 나타냄

`YYYY-MM-DD`, `YYYY-MM`, `YYYY`같이 더 짧은 문자열 형식도 가능합니다.

위 조건을 만족하는 문자열을 대상으로 `Date.parse(str)`를 호출하면 문자열과 대응하는 날짜의 타임스탬프가 반환된다. 문자열의 형식이 조건에 맞지 않은 경우엔 `NaN`이 반환된다.



```javascript
let ms = Date.parse('2012-01-26T13:51:50.417-07:00');

alert(ms); // 1327611110417  (타임스탬프)
```

`Date.parse(str)`를 이용하면 타임스탬프만으로도 새로운 `Date` 객체를 바로 만들 수 있다.

```javascript
let date = new Date( Date.parse('2012-01-26T13:51:50.417-07:00') );

alert(date);
```



> #### 요약
>
> - 자바스크립트에선 [Date](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date) 객체를 사용해 날짜와 시간을 나타낸다. `Date` 객체엔 ‘날짜만’ 혹은 ‘시간만’ 저장하는 것은 불가능하고, 항상 날짜와 시간이 함께 저장된다.
> - 월은 0부터 시작한다(0은 1월을 나타낸다).
> - 요일은 `getDay()`를 사용하면 얻을 수 있는데, 요일 역시 0부터 시작한다(0은 일요일을 나타낸다).
> - 범위를 넘어가는 구성요소를 설정하려 할 때 `Date` 자동 고침이 활성화된다. 이를 이용하면 월/일/시간을 쉽게 날짜에 추가하거나 뺄 수 있다.
> - 날짜끼리 빼는 것도 가능한데, 이때 두 날짜의 밀리초 차이가 반환된다. 이게 가능한 이유는 `Date` 가 숫자형으로 바뀔 때 타임스탬프가 반환되기 때문이다.
> - `Date.now()`를 사용하면 현재 시각의 타임스탬프를 빠르게 구할 수 있다.
>
> 자바스크립트의 타임스탬프는 초가 아닌 밀리초 기준이라는 점을 항상 유의하자.
>
> 간혹 밀리초보다 더 정확한 시간 측정이 필요할 때가 있다. 자바스크립트는 마이크로초(1/1,000,000초)를 지원하진 않지만 대다수의 호스트 환경은 마이크로초를 지원한다. 브라우저 환경의 메서드 [performance.now()](https://developer.mozilla.org/ko/docs/Web/API/Performance/now)는 페이지 로딩에 걸리는 밀리초를 반환해주는데, 반환되는 숫자는 소수점 아래 세 자리까지 지원한다.
>
> ```javascript
> alert(`페이지 로딩이 ${performance.now()}밀리초 전에 시작되었습니다.`);
> // 얼럿 창에 "페이지 로딩이 34731.26000000001밀리초 전에 시작되었습니다."와 유사한 메시지가 뜰 텐데
> // 여기서 '.26'은 마이크로초(260마이크로초)를 나타낸다.
> // 소수점 아래 숫자 세 개 이후의 숫자는 정밀도 에러때문에 보이는 숫자이므로 소수점 아래 숫자 세 개만 유효하다.
> ```
>
> Node.js에선 `microtime` 모듈 등을 사용해 마이크로초를 사용할 수 있다. 자바스크립트가 구동되는 대다수의 호스트 환경과 기기에서 마이크로초를 지원하고 있는데 `Date` 객체만 마이크로초를 지원하지 않는다.







## 5.12 JSON과 메서드

복잡한 객체를 다루고 있다고 가정해 보면, 네트워크를 통해 객체를 어딘가에 보내거나 로깅 목적으로 객체를 출력해야 한다면 객체를 문자열로 전환해야 할 것이다.

이때 전환된 문자열엔 원하는 정보가 있는 객체 프로퍼티 모두가 포함되어야만 한다. 아래와 같은 메서드를 구현해 객체를 문자열로 전환해보자.

```javascript
let user = {
  name: "John",
  age: 30,

  toString() {
    return `{name: "${this.name}", age: ${this.age}}`;
  }
};

alert(user); // {name: "John", age: 30}
```



그런데 개발 과정에서 프로퍼티가 추가되거나 삭제, 수정될 수 있다. 이렇게 되면 위에서 구현한 `toString`을 매번 수정해야 하는데 이는 아주 고통스러운 작업이 될 것이다. 프로퍼티에 반복문을 돌리는 방법을 대안으로 사용할 수 있는데, 중첩 객체 등으로 인해 객체가 복잡한 경우 이를 문자열로 변경하는 건 까다로운 작업이라 이마저도 쉽지 않을 것이다.

다행히 자바스크립트엔 이런 문제를 해결해주는 방법이 있다. 관련 기능이 이미 구현되어 있어서 우리가 직접 코드를 짤 필요가 없다.



#### JSON.stringify

[JSON](http://en.wikipedia.org/wiki/JSON) (JavaScript Object Notation)은 값이나 객체를 나타내주는 범용 포맷으로, [RFC 4627](http://tools.ietf.org/html/rfc4627) 표준에 정의되어 있다. JSON은 본래 자바스크립트에서 사용할 목적으로 만들어진 포맷이다. 그런데 라이브러리를 사용하면 자바스크립트가 아닌 언어에서도 JSON을 충분히 다룰 수 있어서, JSON을 데이터 교환 목적으로 사용하는 경우가 많다. 특히 클라이언트 측 언어가 자바스크립트일 때 말이죠. 서버 측 언어는 무엇이든 상관없다.

자바스크립트가 제공하는 JSON 관련 메서드는 아래와 같다.

- `JSON.stringify` – 객체를 JSON으로 바꿔준다.

- `JSON.parse` – JSON을 객체로 바꿔준다.

  

객체 `student`에 `JSON.stringify`를 적용해보자.

```javascript
let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

let json = JSON.stringify(student);

alert(typeof json); // 문자열이다!

alert(json);
/* JSON으로 인코딩된 객체:
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/
```

`JSON.stringify(student)`를 호출하자 `student`가 문자열로 바뀌었다.

이렇게 변경된 문자열은 *JSON으로 인코딩된(JSON-encoded)*, *직렬화 처리된(serialized)*, *문자열로 변환된(stringified)*, *결집된(marshalled)* 객체라고 부른다. 객체는 이렇게 문자열로 변환된 후에야 비로소 네트워크를 통해 전송하거나 저장소에 저장할 수 있다.

JSON으로 인코딩된 객체는 일반 객체와 다른 특징을 보인다.

- 문자열은 큰따옴표로 감싸야 합니다. JSON에선 작은따옴표나 백틱을 사용할 수 없다(`'John'`이 `"John"`으로 변경된 것을 통해 이를 확인할 수 있다).
- 객체 프로퍼티 이름은 큰따옴표로 감싸야 한다(`age:30`이 `"age":30`으로 변한 것을 통해 이를 확인할 수 있다).

`JSON.stringify`는 객체뿐만 아니라 원시값에도 적용할 수 있다.

적용할 수 있는 자료형은 아래와 같다.

- 객체 `{ ... }`
- 배열 `[ ... ]`
- 원시형:
  - 문자형
  - 숫자형
  - 불린형 값 `true`와 `false`
  - `null`



```javascript
// 숫자를 JSON으로 인코딩하면 숫자이다.
alert( JSON.stringify(1) ) // 1

// 문자열을 JSON으로 인코딩하면 문자열이다(다만, 큰따옴표가 추가된다).
alert( JSON.stringify('test') ) // "test"

alert( JSON.stringify(true) ); // true

alert( JSON.stringify([1, 2, 3]) ); // [1,2,3]
```

JSON은 데이터 교환을 목적으로 만들어진 언어에 종속되지 않는 포맷이다. 따라서 자바스크립트 특유의 객체 프로퍼티는 `JSON.stringify`가 처리할 수 없다.

`JSON.stringify` 호출 시 무시되는 프로퍼티는 아래와 같다.

- 함수 프로퍼티 (메서드)
- 심볼형 프로퍼티 (키가 심볼인 프로퍼티)
- 값이 `undefined`인 프로퍼티

```javascript
let user = {
  sayHi() { // 무시
    alert("Hello");
  },
  [Symbol("id")]: 123, // 무시
  something: undefined // 무시
};

alert( JSON.stringify(user) ); // {} (빈 객체가 출력됨)
```

대개 이 프로퍼티들은 무시 되어도 괜찮다. `JSON.stringify`의 장점 중 하나는 중첩 객체도 알아서 문자열로 바꿔준다는 점이다.



```javascript
let meetup = {
  title: "Conference",
  room: {
    number: 23,
    participants: ["john", "ann"]
  }
};

alert( JSON.stringify(meetup) );
/* 객체 전체가 문자열로 변환되었다.
{
  "title":"Conference",
  "room":{"number":23,"participants":["john","ann"]},
}
*/
```

`JSON.stringify`를 사용할 때 주의하셔야 할 점이 하나 있다. 순환 참조가 있으면 원하는 대로 객체를 문자열로 바꾸는 게 불가능하다.



```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: ["john", "ann"]
};

meetup.place = room;       // meetup은 room을 참조한다.
room.occupiedBy = meetup; // room은 meetup을 참조한다.

JSON.stringify(meetup); // Error: Converting circular structure to JSON
```

`room.occupiedBy`는 `meetup`을, `meetup.place`는 `room`을 참조하기 때문에 JSON으로의 변환이 실패했다.





#### replacer로 원하는 프로퍼티만 직렬화하기

`JSON.stringify`의 전체 문법은 아래와 같다.

```javascript
let json = JSON.stringify(value[, replacer, space])
```

- value

  인코딩 하려는 값

- replacer

  JSON으로 인코딩 하길 원하는 프로퍼티가 담긴 배열. 또는 매핑 함수 `function(key, value)`

- space

  서식 변경 목적으로 사용할 공백 문자 수

대다수의 경우 `JSON.stringify`엔 인수를 하나만 넘겨서 사용한다. 그런데 순환 참조를 다뤄야 하는 경우같이 전환 프로세스를 정교하게 조정하려면 두 번째 인수를 사용해야 한다.

JSON으로 변환하길 원하는 프로퍼티가 담긴 배열을 두 번째 인수로 넘겨주면 이 프로퍼티들만 인코딩할 수 있다.



```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup은 room을 참조한다.
};

room.occupiedBy = meetup; // room references meetup

alert( JSON.stringify(meetup, ['title', 'participants']) );
// {"title":"Conference","participants":[{},{}]}
```

배열에 넣어준 프로퍼티가 잘 출력된 것을 확인할 수 있다. 그런데 배열에 `name`을 넣지 않아서 출력된 문자열의 `participants`가 텅 비어버렸는데 이건 규칙이 너무 까다로워서 발생한 문제이다.

순환 참조를 발생시키는 프로퍼티 `room.occupiedBy`만 제외하고 모든 프로퍼티를 배열에 넣어보자.

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup references room
};

room.occupiedBy = meetup; // room references meetup

alert( JSON.stringify(meetup, ['title', 'participants', 'place', 'name', 'number']) );
/*
{
  "title":"Conference",
  "participants":[{"name":"John"},{"name":"Alice"}],
  "place":{"number":23}
}
*/
```

`occupiedBy`를 제외한 모든 프로퍼티가 직렬화되었다. 그런데 배열이 좀 길다는 느낌이 든다.

`replacer` 자리에 배열 `대신` 함수를 전달해 이 문제를 해결해 보자(매개변수 replacer는 '대신하다’라는 뜻을 가진 영단어 replace에서 그 이름이  유래되었다고 한다).

`replacer`에 전달되는 함수(`replacer` 함수)는 프로퍼티 `(키, 값)` 쌍 전체를 대상으로 호출되는데, 반드시 기존 프로퍼티 값을 대신하여 사용할 값을 반환해야 한다. 특정 프로퍼티를 직렬화에서 누락시키려면 반환 값을 `undefined`로 만들면 된다.

아래 예시는 `occupiedBy`를 제외한 모든 프로퍼티의 값을 변경 없이 “그대로” 직렬화하고 있다. `occupiedBy`는 `undefined`를 반환하게 해 직렬화에 포함하지 않은 것도 확인해 보자.

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  participants: [{name: "John"}, {name: "Alice"}],
  place: room // meetup은 room을 참조한다
};

room.occupiedBy = meetup; // room은 meetup을 참조한다

alert( JSON.stringify(meetup, function replacer(key, value) {
  alert(`${key}: ${value}`);
  return (key == 'occupiedBy') ? undefined : value;
}));

/* replacer 함수에서 처리하는 키:값 쌍 목록
:             [object Object]
title:        Conference
participants: [object Object],[object Object]
0:            [object Object]
name:         John
1:            [object Object]
name:         Alice
place:        [object Object]
number:       23
*/
```

`replacer` 함수가 중첩 객체와 배열의 요소까지 포함한 모든 키-값 쌍을 처리하고 있다는 점에 주목해주시기 바란다. `replacer` 함수는 재귀적으로 키-값 쌍을 처리하는데, 함수 내에서 `this`는 현재 처리하고 있는 프로퍼티가 위치한 객체를 가리킨다.

첫 얼럿창에 예상치 못한 문자열(`":[object Object]"`)이 뜨는걸 볼 수 있는데, 이는 함수가 최초로 호출될 때 `{"": meetup}` 형태의 "래퍼 객체"가 만들어지기 때문이다. `replacer`함수가 가장 처음으로 처리해야하는 `(key, value)` 쌍에서 키는 빈 문자열, 값은 변환하고자 하는 객체(meetup) 전체가 되는 것이다.

이렇게 `replacer` 함수를 사용하면 중첩 객체 등을 포함한 객체 전체에서 원하는 프로퍼티만 선택해 직렬화 할 수 있다.



#### space로 가독성 높이기

`JSON.stringify(value, replacer, space)`의 세 번째 인수 `space`는 가독성을 높이기 위해 중간에 삽입해 줄 공백 문자 수를 나타낸다.

지금까진 `space` 없이 메서드를 호출했기 때문에 인코딩된 JSON에 들여쓰기나 여분의 공백문자가 하나도 없었다. `space`는 가독성을 높이기 위한 용도로 만들어졌기 때문에 단순 전달 목적이라면 `space` 없이 직렬화하는 편이다.

아래 예시처럼 `space`에 `2`를 넘겨주면 자바스크립트는 중첩 객체를 별도의 줄에 출력해주고 공백 문자 두 개를 써 들여쓰기해 준다.

```javascript
let user = {
  name: "John",
  age: 25,
  roles: {
    isAdmin: false,
    isEditor: true
  }
};

alert(JSON.stringify(user, null, 2));
/* 공백 문자 두 개를 사용하여 들여쓰기함:
{
  "name": "John",
  "age": 25,
  "roles": {
    "isAdmin": false,
    "isEditor": true
  }
}
*/

/* JSON.stringify(user, null, 4)라면 아래와 같이 좀 더 들여써진다.
{
    "name": "John",
    "age": 25,
    "roles": {
        "isAdmin": false,
        "isEditor": true
    }
}
*/
```

이처럼 매개변수 `space`는 로깅이나 가독성을 높이는 목적으로 사용된다.





#### 커스텀 “toJSON”

`toString`을 사용해 객체를 문자형으로 변환시키는 것처럼, 객체에 `toJSON`이라는 메서드가 구현되어 있으면 객체를 JSON으로 바꿀 수 있을 것이다. `JSON.stringify`는 이런 경우를 감지하고 `toJSON`을 자동으로 호출해준다.

```javascript
let room = {
  number: 23
};

let meetup = {
  title: "Conference",
  date: new Date(Date.UTC(2017, 0, 1)),
  room
};

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "date":"2017-01-01T00:00:00.000Z",  // (1)
    "room": {"number":23}               // (2)
  }
*/
```

Date 객체의 내장 메서드 `toJSON`이 호출되면서 `date`의 값이 문자열로 변환된 걸 확인할 수 있다(`(1)`).

이번엔 `room`에 직접 커스텀 메서드 `toJSON`을 추가해 보고 `(2)`로 표시한 줄이 어떻게 변경되는지도 확인해보자.

```javascript
let room = {
  number: 23,
  toJSON() {
    return this.number;
  }
};

let meetup = {
  title: "Conference",
  room
};

alert( JSON.stringify(room) ); // 23

alert( JSON.stringify(meetup) );
/*
  {
    "title":"Conference",
    "room": 23
  }
*/
```

위와 같이 `toJSON`은 `JSON.stringify(room)`를 직접 호출할 때도 사용할 수 있고, `room`과 같은 중첩객체에도 구현하여 사용할 수 있다.





#### JSON.parse

[JSON.parse](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)를 사용하면 JSON으로 인코딩된 객체를 다시 객체로 디코딩 할 수 있다.



```javascript
let value = JSON.parse(str, [reviver]);
```

- str

  JSON 형식의 문자열

- reviver

  모든 `(key, value)` 쌍을 대상으로 호출되는 function(key,value) 형태의 함수로 값을 변경시킬 수 있다.

```javascript
// 문자열로 변환된 배열
let numbers = "[0, 1, 2, 3]";

numbers = JSON.parse(numbers);

alert( numbers[1] ); // 1
```



`JSON.parse`는 아래와 같이 중첩 객체에도 사용할 수 있다.

```javascript
let userData = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

let user = JSON.parse(userData);

alert( user.friends[1] ); // 1
```



중첩 객체나 중쳡 배열이 있다면 JSON도 복잡해지기 마련인데, 그렇더라도 결국엔 JSON 포맷 지켜야 한다.

아래에서 디버깅 등의 목적으로 직접 JSON을 만들 때 흔히 저지르는 실수 몇 개를 간추려보았다. 참고하시어 이와 같은 실수를 저지르지 말자.

```javascript
let json = `{
  name: "John",                     // 실수 1: 프로퍼티 이름을 큰따옴표로 감싸지 않았다.
  "surname": 'Smith',               // 실수 2: 프로퍼티 값은 큰따옴표로 감싸야 하는데, 작은따옴표로 감쌌다.
  'isAdmin': false                  // 실수 3: 프로퍼티 키는 큰따옴표로 감싸야 하는데, 작은따옴표로 감쌌다.
  "birthday": new Date(2000, 2, 3), // 실수 4: "new"를 사용할 수 없습니다. 순수한 값(bare value)만 사용할 수 있다.
  "friends": [0,1,2,3]              // 이 프로퍼티는 괜찮다.
}`;
```

JSON은 주석을 지원하지 않는다는 점도 기억해 놓으시기 바란다. 주석을 추가하면 유효하지 않은 형식이 된다.

키를 큰따옴표로 감싸지 않아도 되고 주석도 지원해주는 [JSON5](http://json5.org/)라는 포맷도 있는데, 이 포맷은 자바스크립트 명세서에서 정의하지 않은 독자적인 라이브러리이다.

JSON 포맷이 까다로운 규칙을 가지게 된 이유는 개발자의 귀차니즘 때문이 아니고, 쉽고 빠르며 신뢰할 수 있을 만한 파싱 알고리즘을 구현하기 위해서이다.



#### reviver 사용하기

서버로부터 문자열로 변환된 `meetup` 객체를 전송받았다고 가정하자.

전송받은 문자열은 아마 아래와 같이생겼을것이다.

```javascript
// title: (meetup 제목), date: (meetup 일시)
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';
```

이제 이 문자열을 *역 직렬화(deserialize)* 해서 자바스크립트 객체를 만들어보자.



`JSON.parse`를 호출해보자.

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str);

alert( meetup.date.getDate() ); // 에러!
```

에러가 발생된다!`meetup.date`의 값은 `Date` 객체가 아니고 문자열이기 때문에 발생한 에러이다. 그렇다면 문자열을 `Date`로 전환해줘야 한다는 걸 어떻게 `JSON.parse`에게 알릴 수 있을까?

이럴 때 `JSON.parse`의 두 번째 인수 `reviver`를 사용하면 된다. 모든 값은 “그대로”, 하지만 `date`만큼은 `Date` 객체를 반환하도록 함수를 구현해 보자.

```javascript
let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

let meetup = JSON.parse(str, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( meetup.date.getDate() ); // 제대로 동작한다!
```



참고로 이 방식은 중첩 객체에도 적용할 수 있다.

```javascript
let schedule = `{
  "meetups": [
    {"title":"Conference","date":"2017-11-30T12:00:00.000Z"},
    {"title":"Birthday","date":"2017-04-18T12:00:00.000Z"}
  ]
}`;

schedule = JSON.parse(schedule, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( schedule.meetups[1].date.getDate() ); // 정상 동작한다!
```





>#### 요약
>
>- JSON은 독자적인 표준을 가진 데이터 형식으로, 대부분의 언어엔 JSON을 쉽게 다룰 수 있게 해주는 라이브러리가 있다.
>- JSON은 일반 객체, 배열, 문자열, 숫자, 불린값, `null`을 지원한다.
>- [JSON.stringify](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)를 사용하면 원하는 값을 JSON으로 직렬화 할 수 있고, [JSON.parse](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)를 사용하면 JSON을 본래 값으로 역 직렬화 할 수 있다.
>- 위 두 메서드에 함수를 인수로 넘겨주면 원하는 값만 읽거나 쓰는 게 가능한다.
>- `JSON.stringify`는 객체에 `toJSON` 메서드가 있으면 이를 자동으로 호출해준다.