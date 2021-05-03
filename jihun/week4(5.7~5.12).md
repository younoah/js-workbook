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
- 

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

`map[key] = 2`로 값을 설정하는 것 같이 `map[key]`를 사용할 수 있긴 하지만, 이 방법은 `map`을 일반 객체처럼 취급하게 된다. 따라서 여러 제약이 생기게 된다.

`map`을 사용할 땐 `map`전용 메서드 `set`, `get` 등을 사용해야만 한다.

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

`셋(Set)`은 중복을 허용하지 않는 값을 모아놓은 특별한 컬렉션이다. 셋에 키가 없는 값이 저장된다.

주요 메서드는 다음과 같다.

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

`셋` 대신 배열을 사용하여 방문자 정보를 저장한 후, 중복 값 여부는 배열 메서드인 [arr.find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)를 이용해 확인할 수도 있다. 하지만 `arr.find`는 배열 내 요소 전체를 뒤져 중복 값을 찾기 때문에, 셋보다 성능 면에서 떨어진다. 반면, `셋`은 값의 유일무이함을 확인하는데 최적화되어있다.



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

흥미로운 점이 눈에 띈다. `forEach`에 쓰인 콜백 함수는 세 개의 인수를 받는데, 첫 번째는 `값`, 두 번째도 *같은 값*인 `valueAgain`을 받고 있다. 세 번째는 목표하는 객체(셋)이고요. 동일한 값이 인수에 두 번 등장하고 있다.

이렇게 구현된 이유는 `맵`과의 호환성 때문이다. `맵`의 `forEach`에 쓰인 콜백이 세 개의 인수를 받을 때를 위해서다. 이상해 보이겠지만 이렇게 구현해 놓았기 때문에 `맵`을 `셋`으로 혹은 `셋`을 `맵`으로 교체하기가 쉽다.

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
> 일반적인 `객체`와의 차이점:
>
> - 키의 타입에 제약이 없다. 객체도 키가 될 수 있다.
> - `size` 프로퍼티 등의 유용한 메서드나 프로퍼티가 있다.
>
> `셋`은 중복이 없는 값을 저장할 때 쓰이는 컬렉션이다.
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

