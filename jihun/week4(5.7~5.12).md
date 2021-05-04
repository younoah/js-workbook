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





## 구조 분해 할당

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



