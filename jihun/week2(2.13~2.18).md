#### 2.13 while과 for 반복문

*loop(반복문)* 을 사용하면 동일한 코드를 여러 번 반복할 수 있다.

## [‘while’ 반복문](https://ko.javascript.info/while-for#ref-1966)

```javascript
while (condition) {
  // 코드
  // '반복문 본문(body)'이라 불림
}
```

`condition`(조건)이 truthy 이면 반복문 본문의 `코드`가 실행된다.

아래 반복문은 조건 `i < 3`을 만족할 동안 `i`를 출력해준다.

```javascript
let i = 0;
while (i < 3) { // 0, 1, 2가 출력된다.
  alert( i );
  i++;
}
```

반복문 본문이 한 번 실행되는 것을 *iteration(반복)*이라고 부른다. 위 예시에선 반복문이 세 번의 이터레이션을 만든다.

`i++`가 없었다면 이론적으로 반복문이 영원히 반복되었을 것이다. 그런데 브라우저는 이런 무한 반복을 멈추게 해주는 실질적인 수단을 제공한다. 서버 사이드 자바스크립트도 이런 수단을 제공해 주므로 무한으로 반복되는 프로세스를 죽일 수 있다.

반복문 조건엔 비교뿐만 아니라 모든 종류의 표현식, 변수가 올 수 있다. 조건은 `while`에 의해 평가되고, 평가 후엔 불린값으로 변경된다.

아래 예시에선 `while (i != 0)`을 짧게 줄여 `while (i)`로 만들어보았다.

```javascript
let i = 3;
while (i) { // i가 0이 되면 조건이 falsy가 되므로 반복문이 멈춥니다.
  alert( i );
  i--;
}
```

**본문이 한 줄이면 대괄호를 쓰지 않아도 된다.**

반복문 본문이 한 줄짜리 문이라면 대괄호 `{…}`를 생략할 수 있다.

```javascript
let i = 3;
while (i) alert(i--);
```

## [‘do…while’ 반복문](https://ko.javascript.info/while-for#ref-1967)

`do..while` 문법을 사용하면 `condition`을 반복문 본문 *아래*로 옮길 수 있다.

```javascript
do {
  // 반복문 본문
} while (condition);
```

이때 본문이 먼저 실행되고, 조건을 확인한 후 조건이 truthy인 동안엔 본문이 계속 실행된다.

예시:

```javascript
let i = 0;
do {
  alert( i );
  i++;
} while (i < 3);
```

`do..while` 문법은 조건이 truthy 인지 아닌지에 상관없이, 본문을 **최소한 한번**이라도 실행하고 싶을 때만 사용해야한다. 대다수의 일반적인 상황에선 `do..while`보다 `while(…) {…}`이 적합하다.

## [‘for’ 반복문](https://ko.javascript.info/while-for#ref-1968)

`for` 반복문은 `while` 반복문보다는 복잡하지만 가장 많이 쓰이는 반복문이다.

```javascript
for (begin; condition; step) {
  // ... 반복문 본문 ...
}
```

`for`문을 구성하는 각 요소가 무엇을 의미하는지 알아보자. 아래 반복문을 실행하면 `i`가 `0`부터 `3`이 될 때까지(단, `3`은 포함하지 않음) `alert(i)`가 호출된다.

```javascript
for (let i = 0; i < 3; i++) { // 0, 1, 2가 출력된다.
  alert(i);
}
```

이제 `for`문의 구성 요소를 하나씩 살펴보자.

| 구성 요소 |            |                                                             |
| :-------- | :--------- | :---------------------------------------------------------- |
| begin     | `i = 0`    | 반복문에 진입할 때 단 한 번 실행된다.                       |
| condition | `i < 3`    | 반복마다 해당 조건이 확인됩니다. false이면 반복문을 멈춘다. |
| body      | `alert(i)` | condition이 truthy일 동안 계속해서 실행된다.                |
| step      | `i++`      | 각 반복의 body가 실행된 이후에 실행된다.                    |

일반적인 반복문 알고리즘은 다음과 같다.

```none
begin을 실행함
→ (condition이 truthy이면 → body를 실행한 후, step을 실행함)
→ (condition이 truthy이면 → body를 실행한 후, step을 실행함)
→ (condition이 truthy이면 → body를 실행한 후, step을 실행함)
→ ...
```

`begin`이 한 차례 실행된 이후에, `condition` 확인과 `body`, `step`이 계속해서 반복 실행된다.

```javascript
// for (let i = 0; i < 3; i++) alert(i)

// begin을 실행함
let i = 0
// condition이 truthy이면 → body를 실행한 후, step을 실행함
if (i < 3) { alert(i); i++ }
// condition이 truthy이면 → body를 실행한 후, step을 실행함
if (i < 3) { alert(i); i++ }
// condition이 truthy이면 → body를 실행한 후, step을 실행함
if (i < 3) { alert(i); i++ }
// i == 3이므로 반복문 종료
```

**인라인 변수 선언**

지금까진 ‘카운터’ 변수 `i`를 반복문 안에서 선언하였다. 이런 방식을 ‘인라인’ 변수 선언이라고 부른다. 이렇게 선언한 변수는 반복문 안에서만 접근할 수 있다.

```javascript
for (let i = 0; i < 3; i++) {
  alert(i); // 0, 1, 2
}
alert(i); // Error: i is not defined
```

인라인 변수 선언 대신, 정의되어있는 변수를 사용할 수도 있다.

```javascript
let i = 0;

for (i = 0; i < 3; i++) { // 기존에 정의된 변수 사용
  alert(i); // 0, 1, 2
}

alert(i); // 3, 반복문 밖에서 선언한 변수이므로 사용할 수 있음
```

### [구성 요소 생략하기](https://ko.javascript.info/while-for#ref-1969)

`for`문의 구성 요소를 생략하는 것도 가능하고, 또한 반복문이 시작될 때 아무것도 할 필요가 없으면 `begin`을 생략하는 것이 가능하다.

```javascript
let i = 0; // i를 선언하고 값도 할당하였다.

for (; i < 3; i++) { // 'begin'이 필요하지 않기 때문에 생략하였다.
  alert( i ); // 0, 1, 2
}
```

`step` 역시 생략할 수 있다.

```javascript
let i = 0;

for (; i < 3;) {
  alert( i++ );
}
```

위와 같이 `for`문을 구성하면 `while (i < 3)`과 동일해진다.

모든 구성 요소를 생략할 수도 있는데, 이렇게 되면 무한 반복문이 만들어진다.

```javascript
for (;;) {
  // 끊임 없이 본문이 실행된다.
}
```

`for`문의 구성요소를 생략할 때 주의할 점은 두 개의 `;` 세미콜론을 꼭 넣어주어야 한다는 점인데, 하나라도 없으면 문법 에러가 발생한다.

## [반복문 빠져나오기](https://ko.javascript.info/while-for#ref-1970)

대개는 반복문의 조건이 falsy가 되면 반복문이 종료된다.

그런데 특별한 지시자인 `break`를 사용하면 언제든 원하는 때에 반복문을 빠져나올 수 있다.

아래 예시의 반복문은 사용자에게 일련의 숫자를 입력하도록 안내하고, 사용자가 아무런 값도 입력하지 않으면 반복문을 '종료’한다.

```javascript
let sum = 0;

while (true) {

  let value = +prompt("숫자를 입력하세요.", '');

  if (!value) break; // (*)

  sum += value;

}
alert( '합계: ' + sum );
```

`(*)`로 표시한 줄에 있는 `break`는 사용자가 아무것도 입력하지 않거나 `Cancel`버튼을 눌렀을 때 활성화된다. 이때 반복문이 즉시 중단되고 제어 흐름이 반복문 아래 첫 번째 줄로 이동한다. 여기선 `alert`가 그 첫 번째 줄이 되겠다.

반복문의 시작 지점이나 끝 지점에서 조건을 확인하는 것이 아니라 본문 가운데 혹은 본문 여러 곳에서 조건을 확인해야 하는 경우, '무한 반복문 + `break`’ 조합을 사용하면 좋다.

## [다음 반복으로 넘어가기](https://ko.javascript.info/while-for#continue)

`continue` 지시자는 `break`의 '가벼운 버전’이다. `continue`는 전체 반복문을 멈추지 않는다. 대신에 현재 실행 중인 이터레이션을 멈추고 반복문이 다음 이터레이션을 강제로 실행시키도록 한다(조건을 통과할 때).

`continue`는 현재 반복을 종료시키고 다음 반복으로 넘어가고 싶을 때 사용할 수 있다.

아래 반복문은 `continue`를 사용해 홀수만 출력한다.

```javascript
for (let i = 0; i < 10; i++) {

  // 조건이 참이라면 남아있는 본문은 실행되지 않는다.
  if (i % 2 == 0) continue;

  alert(i); // 1, 3, 5, 7, 9가 차례대로 출력 됨
}
```

`i`가 짝수이면 `continue`가 본문 실행을 중단시키고 다음 이터레이션이 실행되게 한다(`i`가 하나 증가하고, 다음 반복이 실행 됨). 따라서 `alert` 함수는 인수가 홀수일 때만 호출된다.

**`continue`는 중첩을 줄이는 데 도움을 준다.**

홀수를 출력해주는 예시는 아래처럼 생길 수도 있다.

```javascript
for (let i = 0; i < 10; i++) {

  if (i % 2) {
    alert( i );
  }
}
```

기술적인 관점에서 봤을 때, 이 예시는 위쪽에 있는 예시와 동일하다. `continue`를 사용하는 대신 코드를 `if` 블록으로 감싼 점만 다르다.

그런데 이렇게 코드를 작성하면 부작용으로 중첩 레벨(대괄호 안의 `alert` 호출)이 하나 더 늘어난다. `if` 안의 코드가 길어진다면 전체 가독성이 떨어질 수 있다.

삼항연산자 **‘?’ 오른쪽엔 `break`나 `continue`가 올 수 없다.**

표현식이 아닌 문법 구조(syntax construct)는 삼항 연산자 `?`에 사용할 수 없다는 점을 항상 유의해야한다. 특히 `break`나 `continue` 같은 지시자는 삼항 연산자에 사용하면 안된다.

```javascript
if (i > 5) {
  alert(i);
} else {
  continue;
}
```

위의 코드를 삼항연산자를 이용하여 아래와 같이 변경하면 오류가 발생 된다.

```javascript
(i > 5) ? alert(i) : continue; // 여기에 continue를 사용하면 안 된다.
```

이런 코드는 문법 에러를 발생시킨다.

**이는 물음표 연산자 `?`를 `if`문 대용으로 쓰지 말아야 하는 이유 중 하나다.**

## [break/continue와 레이블](https://ko.javascript.info/while-for#ref-1971)

여러 개의 중첩 반복문을 한 번에 빠져나와야 하는 경우가 종종 생기곤 한다.

`i`와 `j`를 반복하면서 프롬프트 창에 `(0,0)`부터 `(2,2)`까지를 구성하는 좌표 `(i, j)`를 입력하게 해주는 예시는 아래와 같다.

```javascript
for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = prompt(`(${i},${j})의 값`, '');

    // 여기서 멈춰서 아래쪽의 `완료!`가 출력되게 하려면 어떻게 해야 할까?
  }
}

alert('완료!');
```

사용자가 `Cancel` 버튼을 눌렀을 때 반복문을 중단시킬 방법이 필요하다.

`input` 아래에 평범한 `break` 지시자를 사용하면 안쪽에 있는 반복문만 빠져나올 수 있다. 이것만으론 충분하지 않다(중첩 반복문을 포함한 반복문 두 개 모두를 빠져나와야 하기 때문이다). 이럴 때 레이블을 사용할 수 있다.

*레이블(label)* 은 반복문 앞에 콜론과 함께 쓰이는 식별자다.

```javascript
labelName: for (...) {
  ...
}
```

반복문 안에서 `break <labelName>`문을 사용하면 레이블에 해당하는 반복문을 빠져나올 수 있다.

```javascript
outer: for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = prompt(`(${i},${j})의 값`, '');

    // 사용자가 아무것도 입력하지 않거나 Cancel 버튼을 누르면 두 반복문 모두를 빠져나온다.
    if (!input) break outer; // (*)

    // 입력받은 값을 가지고 무언가를 함
  }
}
alert('완료!');
```

위 예시에서 `break outer`는 `outer`라는 레이블이 붙은 반복문을 찾고, 해당 반복문을 빠져나오게 해준다.

따라서 제어 흐름이 `(*)`에서 `alert('완료!')`로 바로 바뀐다.

레이블을 별도의 줄에 써주는 것도 가능하다.

```javascript
outer:
for (let i = 0; i < 3; i++) { ... }
```

`continue` 지시자를 레이블과 함께 사용하는 것도 가능하다. 두 가지를 같이 사용하면 레이블이 붙은 반복문의 다음 이터레이션이 실행된다.

**레이블은 마음대로 '점프’할 수 있게 해주지 않는다.**

레이블을 사용한다고 해서 원하는 곳으로 마음대로 점프할 수 있는 것은 아니다.

아래 예시처럼 레이블을 사용하는 것은 불가능하다.

```javascript
break label; // 아래 for 문으로 점프할 수 없다.

label: for (...)
```

`break`와 `continue`는 반복문 안에서만 사용할 수 있고, 레이블은 반드시 `break`이나 `continue` 지시자 위에 있어야 한다.

## [요약](https://ko.javascript.info/while-for#ref-1972)

- `while` – 각 반복이 시작하기 전에 조건을 확인합니다.
- `do..while` – 각 반복이 끝난 후에 조건을 확인합니다.
- `for (;;)` – 각 반복이 시작하기 전에 조건을 확인합니다. 추가 세팅을 할 수 있습니다.

‘무한’ 반복문은 보통 `while(true)`를 써서 만든다. 무한 반복문은 여타 반복문과 마찬가지로 `break` 지시자를 사용해 멈출 수 있다.

현재 실행 중인 반복에서 더는 무언가를 하지 않고 다음 반복으로 넘어가고 싶다면 `continue` 지시자를 사용할 수 있다.

반복문 앞에 레이블을 붙이고, `break/continue`에 이 레이블을 함께 사용할 수 있다. 레이블은 중첩 반복문을 빠져나와 바깥의 반복문으로 갈 수 있게 해주는 유일한 방법이다.





#### 2.14 switch문

복수의 `if` 조건문은 `switch`문으로 바꿀 수 있는데, `switch`문을 사용한 비교법은 특정 변수를 다양한 상황에서 비교할 수 있게 해준다. 코드 자체가 비교 상황을 잘 설명한다는 장점도 있다.

## [문법](https://ko.javascript.info/switch#ref-1837)

`switch`문은 하나 이상의 `case`문으로 구성되며, 대개 `default`문도 있지만, 이는 다른 프로그래밍 언어와는 다르게 필수는 아니다.

```javascript
switch(x) {
  case 'value1':  // if (x === 'value1')
    ...
    [break]

  case 'value2':  // if (x === 'value2')
    ...
    [break]

  default:
    ...
    [break]
}
```

- 변수 `x`의 값과 첫 번째 `case`문의 값 `'value1'`를 일치 비교한 후, 두 번째 `case`문의 값 `'value2'`와 비교한다. 이런 과정은 계속 이어진다.
- `case`문에서 변수 `x`의 값과 일치하는 값을 찾으면 해당 `case` 문의 아래의 코드가 실행된다. 이때, `break`문을 만나거나 `switch` 문이 끝나면 코드의 실행은 멈춘다.
- 값과 일치하는 `case`문이 없다면, `default`문 아래의 코드가 실행된다.(`default` 문이 있는 경우)

```javascript
let a = 2 + 2;

switch (a) {
  case 3:
    alert( '비교하려는 값보다 작습니다.' );
    break;
  case 4:
    alert( '비교하려는 값과 일치합니다.' );
    break;
  case 5:
    alert( '비교하려는 값보다 큽니다.' );
    break;
  default:
    alert( "어떤 값인지 파악이 되지 않습니다." );
}
```

`switch`문은 a의 값인 4와 첫 번째 `case`문의 값인 3을 비교한다. 두 값은 같지 않기 때문에 다음 `case`문으로 넘어간다.

a와 그다음 `case`문의 값인 4는 일치한다. 따라서 `break`문을 만날 때까지 `case 4` 아래의 코드가 실행된다.

**`case`문 안에 `break`문이 없으면 조건에 부합하는지 여부를 따지지 않고 이어지는 `case`문을 실행한다.**

`break`문이 없는 경우 아래의 코드의 결과를 살펴보자.

```javascript
let a = 2 + 2;

switch (a) {
  case 3:
    alert( '비교하려는 값보다 작습니다.' );
  case 4:
    alert( '비교하려는 값과 일치합니다.' );
  case 5:
    alert( '비교하려는 값보다 큽니다.' );
  default:
    alert( "어떤 값인지 파악이 되지 않습니다." );
}
```

위 예시를 실행하면 아래 3개의 `alert`문이 실행된다.

```javascript
alert( '비교하려는 값과 일치합니다.' );
alert( '비교하려는 값보다 큽니다.' );
alert( "어떤 값인지 파악이 되지 않습니다." );
```

**`switch/case`문의 인수엔 어떤 표현식이든 올 수 있다.**

`switch`문과 `case`문은 모든 형태의 표현식을 인수로 받는다.

```javascript
let a = "1";
let b = 0;

switch (+a) {
  case b + 1:
    alert("표현식 +a는 1, 표현식 b+1는 1이므로 이 코드가 실행됩니다.");
    break;

  default:
    alert("이 코드는 실행되지 않습니다.");
}
```

표현식 +a를 평가하면 1이 된다. 이 값은 첫 번째 `case`문의 표현식 `b + 1`을 평가한 값(1)과 일치한다. 따라서 첫 번째 `case`문 아래의 코드가 실행된다.

## [여러 개의 "case"문 묶기](https://ko.javascript.info/switch#ref-1839)

코드가 같은 `case`문은 한데 묶을 수 있다.

`case 3`과 `case 5`에서 실행하려는 코드가 같은 경우에 대한 예시다.

```javascript
let a = 3;

switch (a) {
  case 4:
    alert('계산이 맞습니다!');
    break;

  case 3: // (*) 두 case문을 묶음
  case 5:
    alert('계산이 틀립니다!');
    alert("수학 수업을 다시 들어보는걸 권유 드립니다.");
    break;

  default:
    alert('계산 결과가 이상하네요.');
}
```

`case 3`과 `case 5`는 동일한 메시지를 보여준다.

`switch/case`문에서 `break`문이 없는 경우엔 조건에 상관없이 다음 `case`문이 실행되는 부작용이 발생한다. 위 예시에서 `case 3`이 참인 경우엔 `(*)`로 표시한 줄 아래의 코드가 실행되는데, 그 아래 줄엔 `case 5`가 있고 `break`문도 없기 때문에 12번째 줄의 `break`문을 만날 때까지 코드는 계속 실행된다.

## [자료형의 중요성](https://ko.javascript.info/switch#ref-1840)

switch문은 일치 비교로 조건을 확인힌다. 비교하려는 값과 `case`문의 값의 형과 값이 같아야 해당 `case`문이 실행된다.

예시를 통해 switch문에서 자료형이 얼마나 중요한지 알수있다.

```javascript
let arg = prompt("값을 입력해주세요.");
switch (arg) {
  case '0':
  case '1':
    alert( '0이나 1을 입력하셨습니다.' );
    break;

  case '2':
    alert( '2를 입력하셨습니다.' );
    break;

  case 3:
    alert( '이 코드는 절대 실행되지 않습니다!' );
    break;
  default:
    alert( '알 수 없는 값을 입력하셨습니다.' );
}
```

> 1.`0`이나 `1`을 입력한 경우엔 첫 번째 `alert`문이 실행된다.
>
> 2.`2`를 입력한 경우엔 두 번째 `alert`문이 실행된다.
>
> 3.`3`을 입력하였더라도 세 번째 `alert`문은 실행되지 않는다. 앞서 배운 바와 같이 `prompt` 함수는 사용자가 입력 필드에 기재한 값을 문자열로 변환해 반환하기 때문에 숫자 `3`을 입력하더라도 `prompt` 함수는 문자열 `'3'`을 반환한다. 그런데 세 번째 `case`문에선 사용자가 입력한 값과 숫자형 3을 비교하므로, 형 자체가 다르기 때문에 `case 3` 아래의 코드는 절대 실행되지 않는다. 대신 `default`문이 실행된다.





#### 2.15 함수

함수는 프로그램을 구성하는 주요 '구성 요소(building block)'다. 함수를 이용하면 중복 없이 유사한 동작을 하는 코드를 여러 번 호출할 수 있다.

## [함수 선언](https://ko.javascript.info/function-basics#ref-565)

*함수 선언(function declaration)* 방식을 이용하면 함수를 만들 수 있다.(함수 선언 방식은 함수 선언문이라고 부르기도 한다)

```javascript
function showMessage() {
  alert( '안녕하세요!' );
}
```

`function` 키워드, *함수 이름*, 괄호로 둘러싼 매개변수를 차례로 써주면 함수를 선언할 수 있다. 위 함수에는 매개변수가 없는데, 만약 매개변수가 여러 개 있다면 각 매개변수를 콤마로 구분해준다. 이어서 함수를 구성하는 코드의 모임인 '함수 본문(body)'을 중괄호로 감싸 준다.

```javascript
function name(parameters) {
  ...함수 본문...
}
```

새롭게 정의한 함수는 함수 이름 옆에 괄호를 붙여 호출할 수 있다. `showMessage()`같이 할 수 있다.

```javascript
function showMessage() {
  alert( '안녕하세요!' );
}

showMessage();
showMessage();
```

`showMessage()`로 함수를 호출하면 함수 본문이 실행된다. 위 예시에선 showMessage를 두 번 호출했으므로 얼럿 창이 두 번 뜨게 된다.

함수의 주요 용도 중 하나는 중복 코드 피하기다.

얼럿 창에 보여줄 메시지를 바꾸거나 메시지를 보여주는 방식 자체를 변경하고 싶다면, 함수 본문 중 출력에 관여하는 코드 딱 하나만 수정해주면 된다.

## [지역 변수](https://ko.javascript.info/function-basics#ref-566)

함수 내에서 선언한 변수인 지역 변수(local variable)는 함수 안에서만 접근할 수 있다.

```javascript
function showMessage() {
  let message = "안녕하세요!"; // 지역 변수

  alert( message );
}

showMessage(); // 안녕하세요!

alert( message ); // ReferenceError: message is not defined (message는 함수 내 지역 변수이기 때문에 에러가 발생합니다.)
```

## [외부 변수](https://ko.javascript.info/function-basics#ref-567)

함수 내부에서 함수 외부의 변수인 외부 변수(outer variable)에 접근할 수 있다.

```javascript
let userName = 'John';

function showMessage() {
  let message = 'Hello, ' + userName;
  alert(message);
}

showMessage(); // Hello, John
```

함수에선 외부 변수에 접근하는 것뿐만 아니라, 수정도 할 수 있다.

```javascript
let userName = 'John';

function showMessage() {
  userName = "Bob"; // (1) 외부 변수를 수정함

  let message = 'Hello, ' + userName;
  alert(message);
}

alert( userName ); // 함수 호출 전이므로 John 이 출력됨

showMessage();

alert( userName ); // 함수에 의해 Bob 으로 값이 바뀜
```

외부 변수는 지역 변수가 없는 경우에만 사용할 수 있다.

함수 내부에 외부 변수와 동일한 이름을 가진 변수가 선언되었다면, 내부 변수는 외부 변수를 *가리킨다*. 함수 내부에 외부 변수와 동일한 이름을 가진 지역 변수 `userName`가 선언되어 있다. 외부 변수는 내부 변수에 가려져 값이 수정되지 않았다.

```javascript
let userName = 'John';

function showMessage() {
  let userName = "Bob"; // 같은 이름을 가진 지역 변수를 선언한다.

  let message = 'Hello, ' + userName; // Bob
  alert(message);
}

// 함수는 내부 변수인 userName만 사용한다,
showMessage();

alert( userName ); // 함수는 외부 변수에 접근하지 않는다. 따라서 값이 변경되지 않고, John이 출력된다.
```

**전역 변수**

위 예시의 `userName`처럼, 함수 외부에 선언된 변수는 *전역 변수(global variable)* 라고 부른다.

전역 변수는 같은 이름을 가진 지역 변수에 의해 가려지지만 않는다면 모든 함수에서 접근할 수 있다.

**변수는 연관되는 함수 내에 선언하고, `전역 변수는 되도록 사용하지 않는 것이 좋다.`** 비교적 근래에 작성된 코드들은 대부분 전역변수를 사용하지 않거나 최소한으로만 사용한다. 다만 프로젝트 전반에서 사용되는 데이터는 전역 변수에 저장하는 것이 유용한 경우도 있으니 이 점을 기억하자.

## [매개변수](https://ko.javascript.info/function-basics#ref-568)

매개변수(parameter)를 이용하면 임의의 데이터를 함수 안에 전달할 수 있다. 매개변수는 *인수(argument)* 라고 불리기도 한다.

**-> 함수를 정의할 때 사용하는 경우 : Parameter / 함수를 호출할 때 사용하는 경우 : Argument**

아래 예시에서 함수 showMessage는 매개변수 `from` 과 `text`를 가진다.

```javascript
function showMessage(from, text) { // 인수: from, text
  alert(from + ': ' + text);
}

showMessage('Ann', 'Hello!'); // Ann: Hello! (*)
showMessage('Ann', "What's up?"); // Ann: What's up? (**)
```

`(*)`, `(**)`로 표시한 줄에서 함수를 호출하면, 함수에 전달된 인자는 지역변수 `from`과 `text`에 복사된다. 그 후 함수는 지역변수에 복사된 값을 사용한다.



 전역 변수 `from`이 있고, 이 변수를 함수에 전달하였다. 함수가 `from`을 변경하지만, 변경 사항은 외부 변수 `from`에 반영되지 않았다. 함수는 언제나 복사된 값을 사용하기 때문이다.

```javascript
function showMessage(from, text) {

  from = '*' + from + '*'; // "from"을 좀 더 멋지게 꾸며준다.

  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // *Ann*: Hello

// 함수는 복사된 값을 사용하기 때문에 바깥의 "from"은 값이 변경되지 않는다.
alert( from ); // Ann
```

## [기본값](https://ko.javascript.info/function-basics#ref-569)

매개변수에 값을 전달하지 않으면 그 값은 `undefined`가 된다.

위에서 정의한 함수 `showMessage(from, text)`는 매개변수가 2개지만, 아래와 같이 인수를 하나만 넣어서 호출할 수 있다.

```javascript
showMessage("Ann");
```

이렇게 코드를 작성해도 에러가 발생하지 않는다. 두 번째 매개변수에 값을 전달하지 않았기 때문에 `text`엔 `undefiend`가 할당될 뿐이다. 따라서 에러 없이 `"Ann: undefined"`가 출력된다.

매개변수에 값을 전달하지 않아도 그 값이 `undefined`가 되지 않게 하려면 '기본값(default value)'을 설정해주면 된다. 매개변수 오른쪽에 `=`을 붙이고 `undefined` 대신 설정하고자 하는 기본값을 써주면 된다.

```javascript
function showMessage(from, text = "no text given") {
  alert( from + ": " + text );
}

showMessage("Ann"); // Ann: no text given
```

이젠 `text`가 값을 전달받지 못해도 `undefined`대신 기본값 `"no text given"`이 할당된다.

위 예시에선 문자열 `"no text given"`을 기본값으로 설정했다. 하지만 아래와 같이 복잡한 표현식도 기본값으로 설정할 수도 있다.

```javascript
function showMessage(from, text = anotherFunction()) {
  // anotherFunction()은 text값이 없을 때만 호출됨
  // anotherFunction()의 반환 값이 text의 값이 됨
}
```

**매개변수 기본값 평가 시점**

자바스크립트에선 함수를 호출할 때마다 매개변수 기본값을 평가한다. 물론 해당하는 매개변수가 없을 때만 기본값을 평가한다.

위 예시에선 매개변수 `text`에 값이 없는 경우 `showMessage()`를 호출할 때마다 `anotherFunction()`이 호출된다.

### [매개변수 기본값을 설정할 수 있는 또 다른 방법](https://ko.javascript.info/function-basics#ref-570)

가끔은 함수 선언부에서 매개변수 기본값을 설정하는 것 대신 함수가 실행되는 도중에 기본값을 설정하는 게 논리에 맞는 경우가 생기기도 한다.

이런 경우엔 일단 매개변수를 `undefined`와 비교하여 함수 호출 시 매개변수가 생략되었는지를 확인한다.

```javascript
function showMessage(text) {
  if (text === undefined) {
    text = '빈 문자열';
  }

  alert(text);
}

showMessage(); // 빈 문자열
```

이렇게 `if`문을 쓰는 것 대신 논리 연산자 `||`를 사용할 수도 있다.

```javascript
// 매개변수가 생략되었거나 빈 문자열("")이 넘어오면 변수에 '빈 문자열'이 할당된다.
function showMessage(text) {
  text = text || '빈 문자열';
  ...
}
```

이 외에도 모던 자바스크립트 엔진이 지원하는 [null 병합 연산자(nullish coalescing operator)](https://ko.javascript.info/nullish-coalescing-operator) `??`를 사용하면 `0`처럼 falsy로 평가되는 값들을 일반 값처럼 처리할 수 있어서 좋다.

```javascript
// 매개변수 'count'가 넘어오지 않으면 'unknown'을 출력해주는 함수
function showCount(count) {
  alert(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknown
showCount(); // unknown
showCount(undefined); // unknown
```

## [반환 값](https://ko.javascript.info/function-basics#ref-571)

함수를 호출했을 때 함수를 호출한 그곳에 특정 값을 반환하게 할 수 있다. 이때 이 특정 값을 반환 값(return value)이라고 부른다.

```javascript
function sum(a, b) {
  return a + b;
}

let result = sum(1, 2);
alert( result ); // 3
```

지시자 `return`은 함수 내 어디서든 사용할 수 있다. 실행 흐름이 지시자 `return`을 만나면 함수 실행은 즉시 중단되고 함수를 호출한 곳에 값을 반환한다. 위 예시에선 반환 값을 `result`에 할당하였다.

아래와 같이 함수 하나에 여러 개의 `return`문이 올 수도 있다.

```javascript
function checkAge(age) {
  if (age >= 18) {
    return true;
  } else {
    return confirm('보호자의 동의를 받으셨나요?');
  }
}

let age = prompt('나이를 알려주세요', 18);

if ( checkAge(age) ) {
  alert( '접속 허용' );
} else {
  alert( '접속 차단' );
}
```

아래와 같이 지시자 `return`만 명시하는 것도 가능합니다. 이런 경우는 함수가 즉시 종료된다.

```javascript
function showMovie(age) {
  if ( !checkAge(age) ) {
    return;
  }

  alert( "영화 상영" ); // (*)
  // ...
}
```

위 예시에서, `checkAge(age)`가 `false`를 반환하면, `(*)`로 표시한 줄은 실행이 안 되기 때문에 함수 `showMovie`는 얼럿 창을 보여주지 않는다.

**`return`문이 없거나 `return` 지시자만 있는 함수는 `undefined`를 반환한다.**

`return`문이 없는 함수도 무언가를 반환합니다. `undefined`를 반환한다.

```javascript
function doNothing() { /* empty */ }

alert( doNothing() === undefined ); // true
```

`return` 지시자만 있는 경우도 `undefined`를 반환합니다. `return`은 `return undefined`와 동일하게 동작하죠.

```javascript
function doNothing() {
  return;
}

alert( doNothing() === undefined ); // true
```

**`return`과 값 사이에 절대 줄을 삽입하지 마세요.**

반환하려는 값이 긴 표현식인 경우, 아래와 같이 지시자 `return`과 반환하려는 값 사이에 새 줄을 넣어 코드를 작성하고 싶을 수도 있습니다.

```javascript
return
 (some + long + expression + or + whatever * f(a) + f(b))
```

자바스크립트는 return문 끝에 세미콜론을 자동으로 넣기 때문에 이렇게 `return`문을 작성하면 안 됩니다. 위 코드는 아래 코드처럼 동작합니다.

```javascript
return;
 (some + long + expression + or + whatever * f(a) + f(b))
```

따라서 반환하고자 했던 표현식을 반환하지 못하고 아무것도 반환하지 않는 것처럼 되어버립니다.

표현식을 여러 줄에 걸쳐 작성하고 싶다면 표현식이 `return` 지시자가 있는 줄에서 시작하도록 작성해야 합니다. 또는 아래와 같이 여는 괄호를 `return` 지시자와 같은 줄에 써줘도 괜찮습니다.

```javascript
return (
  some + long + expression
  + or +
  whatever * f(a) + f(b)
  )
```

이렇게 하면 의도한 대로 표현식을 반환할 수 있습니다.

## [함수 이름짓기](https://ko.javascript.info/function-basics#function-naming)

함수는 어떤 `동작`을 수행하기 위한 코드를 모아놓은 것이다. 따라서 함수의 이름은 대개 동사이다. 함수 이름은 가능한 한 간결하고 명확해야 한다. 함수가 어떤 동작을 하는지 설명할 수 있어야 하죠. 코드를 읽는 사람은 함수 이름만 보고도 함수가 어떤 기능을 하는지 힌트를 얻을 수 있어야 한다.

함수가 어떤 동작을 하는지 축약해서 설명해주는 동사를 접두어로 붙여 함수 이름을 만드는 게 관습입니다. 다만, 팀 내에서 그 뜻이 반드시 합의된 접두어만 사용해야 한다.

`"show"`로 시작하는 함수는 대개 무언가를 보여주는 함수다.

이 외에 아래와 같은 접두어를 사용할 수 있다.

- `"get…"` – 값을 반환함
- `"calc…"` – 무언가를 계산함
- `"create…"` – 무언가를 생성함
- `"check…"` – 무언가를 확인하고 불린값을 반환함

위 접두어를 사용하면 아래와 같은 함수를 만들 수 있다.

```javascript
showMessage(..)     // 메시지를 보여줌
getAge(..)          // 나이를 나타내는 값을 얻고 그 값을 반환함
calcSum(..)         // 합계를 계산하고 그 결과를 반환함
createForm(..)      // form을 생성하고 만들어진 form을 반환함
checkPermission(..) // 승인 여부를 확인하고 true나 false를 반환함
```

접두어를 적절히 활용하면 함수 이름만 보고도 함수가 어떤 동작을 하고 어떤 값을 반환하는지 쉽게 알 수 있다.

**함수는 동작 하나만 담당해야 한다.**

함수는 함수 이름에 언급되어있는 동작을 정확히 수행해야 한다. 그 이외의 동작은 수행해선 안 된다.

독립적인 두 개의 동작은 독립된 함수 두 개에서 나눠서 수행할 수 있게 해야 한다. 한 장소에서 두 동작을 동시에 필요로 하는 경우라도 말이다(이 경우는 제3의 함수를 만들어 그곳에서 두 함수를 호출한다).

> 개발자들이 빈번히 하는 실수들.
>
> 1.`getAge` 함수는 나이를 얻어오는 동작만 수행해야 한다. `alert` 창에 나이를 출력해주는 동작은 이 함수에 들어가지 않는 것이 좋다.
>
> 2.`createForm` 함수는 form을 만들고 이를 반환하는 동작만 해야 한다. form을 문서에 추가하는 동작이 해당 함수에 들어가 있으면 좋지 않다.
>
> 3.`checkPermission` 함수는 승인 여부를 확인하고 그 결과를 반환하는 동작만 해야 한다. 승인 여부를 보여주는 메시지를 띄우는 동작이 들어가 있으면 좋지 않다.



위 예시들은 접두어의 의미가 합의되었다고 가정하고 만들었다. 본인이나 본인이 속한 팀에서 접두어의 의미를 재합의하여 함수를 만들 수도 있긴 하지만, 아마도 위 예시에서 사용한 접두어 의미와 크게 차이가 나진 않을 거다. 어찌 되었든 접두어를 사용하여 함수 이름을 지을 땐, 해당 접두어에 어떤 의미가 있는지 잘 이해하고 있어야 한다. 해당 접두어가 붙은 함수가 어떤 동작을 하는지, 어떤 동작은 하지 못하는지 알고 있어야 한다. 접두어를 붙여 만든 모두 함수는 팀에서 만든 규칙을 반드시 따라야 한다. 팀원들은 이 규칙을 충분히 이해하고 있어야 하며, 팀원들 사이에 이 규칙이 잘 공유되어야 한다.

**아주 짧은 이름**

정말 *빈번히* 쓰이는 함수 중에 이름이 아주 짧은 함수가 있다.

[jQuery](http://jquery.com/) 프레임워크에서 쓰이는 함수 `$`와 [Lodash](http://lodash.com/) 라이브러리의 핵심 함수 `_` 말이다.

이 함수들은 지금까지 소개한 함수 이름짓기에 관련된 규칙을 지키지 않고 있다. 예외에 속하죠. 함수 이름은 간결하고 함수가 어떤 일을 하는지 설명할 수 있게 지어야한다.

## [함수 == 주석](https://ko.javascript.info/function-basics#ref-572)

함수는 간결하고, 한 가지 기능만 수행할 수 있게 만들어야 한다. 함수가 길어지면 함수를 잘게 쪼갤 때가 되었다는 신호로 받아들이셔야 한다. 함수를 쪼개는 건 쉬운 작업은 아니다. 하지만 함수를 분리해 작성하면 많은 장점이 있기 때문에 함수가 길어질 경우엔 함수를 분리해 작성할 것을 권유한다.

함수를 간결하게 만들면 테스트와 디버깅이 쉬워지고, 함수 그 자체로 주석의 역할까지 한다!

같은 동작을 하는 함수, `showPrimes(n)`를 두 개 만들어 비교해 보자. `showPrimes(n)`은 `n`까지의 [소수(prime numbers)](https://en.wikipedia.org/wiki/Prime_number)를 출력해준다.

첫 번째 `showPrimes(n)`에선 레이블을 사용해 반복문을 작성해본다.

```javascript
function showPrimes(n) {
  nextPrime: for (let i = 2; i < n; i++) {

    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }

    alert( i ); // 소수
  }
}
```

두 번째 `showPrimes(n)`는 소수인지 아닌지 여부를 검증하는 코드를 따로 분리해 `isPrime(n)`이라는 함수에 넣어서 작성했다.

```javascript
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    if (!isPrime(i)) continue;

    alert(i);  // a prime
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if ( n % i == 0) return false;
  }
  return true;
}
```

두 번째 `showPrimes(n)`가 더 이해하기 쉽다. `isPrime` 함수 이름을 보고 해당 함수가 소수 여부를 검증하는 동작을 한다는 걸 쉽게 알 수 있다. 이렇게 이름만 보고도 어떤 동작을 하는지 알 수 있는 코드를 *자기 설명적(self-describing)* 코드라고 부른다.

위와 같이 함수는 중복을 없애려는 용도 외에도 사용할 수 있다. 이렇게 함수를 활용하면 코드가 정돈되고 가독성이 높아진다.

## [요약](https://ko.javascript.info/function-basics#ref-573)

> **함수 선언 방식으로 함수를 만들 수 있다**
>
> > ``` javascript
> > function 함수이름(복수의, 매개변수는, 콤마로, 구분한다) {
> >   /* 함수 본문 */
> > }
> > ```
>
> 1.함수에 전달된 매개변수는 복사된 후 함수의 지역변수가 된다.
>
> 2.함수는 외부 변수에 접근할 수 있다. 하지만 함수 바깥에서 함수 내부의 지역변수에 접근하는 건 불가능하다.
>
> 3.함수는 값을 반환할 수 있다. 값을 반환하지 않는 경우는 반환 값이 `undefined`가 된다.
>
> 4.깔끔하고 이해하기 쉬운 코드를 작성하려면 함수 내부에서 외부 변수를 사용하는 방법 대신 지역 변수와 매개변수를 활용하는 게 좋다.
>
> 5.개발자는 매개변수를 받아서 그 변수를 가지고 반환 값을 만들어 내는 함수를 더 쉽게 이해할 수 있다. 매개변수 없이 함수 내부에서 외부 변수를 수정해 반환 값을 만들어 내는 함수는 쉽게 이해하기 힘들다.



> **함수 이름을 지을 땐 아래와 같은 규칙을 따르는 것이 좋다.**
>
> 1.함수 이름은 함수가 어떤 동작을 하는지 설명할 수 있어야 한다. 이렇게 이름을 지으면 함수 호출 코드만 보아도 해당 함수가 무엇을 하고 어떤 값을 반환할지 바로 알 수 있다.
>
> 2.함수는 동작을 수행하기 때문에 이름이 주로 동사이다.
>
> 3.`create…`, `show…`, `get…`, `check…` 등의 잘 알려진 접두어를 사용해 이름을 지을 수 있다. 접두어를 사용하면 함수 이름만 보고도 해당 함수가 어떤 동작을 하는지 파악할 수 있다.



#### 2.16 함수 표현식

# 함수 표현식

자바스크립트는 함수를 특별한 종류의 값으로 취급한다. 다른 언어에서처럼 "특별한 동작을 하는 구조"로 취급되지 않는다.

이전 챕터에서 아래와 같은 *함수 선언(Function Declaration) 방식으로 함수를 만들었었다.

```javascript
function sayHi() {
  alert( "Hello" );
}
```

함수 선언 방식 외에 *함수 표현식(Function Expression)* 을 사용해서 함수를 만들 수 있다.

```javascript
let sayHi = function() {
  alert( "Hello" );
};
```

함수를 생성하고 변수에 값을 할당하는 것처럼 함수가 변수에 할당되었다. 함수가 어떤 방식으로 만들어졌는지에 관계없이 함수는 값이고, 따라서 변수에 할당할 수 있다. 위 예시에선 함수가 변수 `sayHi`에 저장된 값이 되었다.

위 예시를 간단한 말로 풀면 다음과 같다: “함수를 만들고 그 함수를 변수 `sayHi`에 할당하기”

함수는 값이기 때문에 `alert`를 이용하여 함수 코드를 출력할 수도 있다.

```javascript
function sayHi() {
  alert( "Hello" );
}

alert( sayHi ); // 함수 코드가 보임
```

마지막 줄에서 `sayHi`옆에 괄호가 없기 때문에 함수는 실행되지 않는다. 어떤 언어에선 괄호 없이 함수 이름만 언급해도 함수가 실행된다. 하지만 자바스크립트는 괄호가 있어야만 함수가 호출된다.

자바스크립트에서 함수는 값이다. 따라서 함수를 값처럼 취급할 수 있다. 위 코드에선 함수 소스 코드가 문자형으로 바뀌어 출력되었다.

함수는 `sayHi()`처럼 호출할 수 있다는 점 때문에 일반적인 값과는 조금 다르다. 특별한 종류의 값이다.

하지만 그 본질은 값이기 때문에 값에 할 수 있는 일을 함수에도 할 수 있다. 변수를 복사해 다른 변수에 할당하는 것처럼 함수를 복사해 다른 변수에 할당할 수도 있다.

```javascript
function sayHi() {   // (1) 함수 생성
  alert( "Hello" );
}

let func = sayHi;    // (2) 함수 복사

func(); // Hello     // (3) 복사한 함수를 실행(정상적으로 실행된다)!
sayHi(); // Hello    //     본래 함수도 정상적으로 실행된다.
```

1. `(1)`에서 함수 선언 방식을 이용해 함수를 생성한다. 생성한 함수는 `sayHi`라는 변수에 저장된다.
2. `(2)` 에선 `sayHi`를 새로운 변수 `func`에 복사한다. 이때 `sayHi` 다음에 괄호가 없다는 점에 유의해야한다. 괄호가 있었다면 `func = sayHi()` 가 되어 `sayHi` *함수* 그 자체가 아니라, *함수 호출 결과(함수의 반환 값)* 가 `func`에 저장되었을 거다.
3. 이젠 `sayHi()` 와 `func()`로 함수를 호출할 수 있게 되었다.

함수 `sayHi`는 아래와 같이 함수 표현식을 사용해 정의할 수 있다.

```javascript
let sayHi = function() {
  alert( "Hello" );
};

let func = sayHi;
// ...
```

동작 결과는 동일하다.

**끝에 세미 콜론은 왜 있나요?**

함수 표현식의 끝에 왜 세미 콜론 `;`이 붙는지 의문이 들 수 있다. 함수 선언문에는 세미 콜론이 없는데 말이다.

```javascript
function sayHi() {
  // ...
}

let sayHi = function() {
  // ...
};
```

- `if { ... }`, `for { }`, `function f { }` 같이 중괄호로 만든 코드 블록 끝엔 `;`이 없어도 된다.
- 함수 표현식은 `let sayHi = ...;`과 같은 구문 안에서 값의 역할을 한다. 코드 블록이 아니고 값처럼 취급되어 변수에 할당된다. 모든 구문의 끝엔 세미 콜론 `;`을 붙이는 게 좋다. 함수 표현식에 쓰인 세미 콜론은 함수 표현식 때문에 붙여진 게 아니라, 구문의 끝이기 때문에 붙여졌다.

## [콜백 함수](https://ko.javascript.info/function-expressions#ref-310)

매개변수가 3개 있는 함수, `ask(question, yes, no)`를 작성해보면, 각 매개변수에 대한 설명은 아래와 같다.

- `question`

  질문

- `yes`

  "Yes"라고 답한 경우 실행되는 함수

- `no`

  "No"라고 답한 경우 실행되는 함수

함수는 반드시 `question(질문)`을 해야 하고, 사용자의 답변에 따라 `yes()` 나 `no()`를 호출한다.

```javascript
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

function showOk() {
  alert( "동의하셨습니다." );
}

function showCancel() {
  alert( "취소 버튼을 누르셨습니다." );
}

// 사용법: 함수 showOk와 showCancel가 ask 함수의 인수로 전달됨
ask("동의하십니까?", showOk, showCancel);
```

이렇게 함수를 작성하는 방법은 실무에서 아주 유용하게 쓰인다. 면대면으로 질문하는 것보다 위처럼 컨펌창을 띄워 질문을 던지고 답변을 받으면 간단하게 설문조사를 진행할 수 있다. 실제 상용 서비스에선 컨펌 창을 좀 더 멋지게 꾸미는 등의 작업이 동반되긴 하지만, 일단 여기선 그게 중요한 포인트는 아니다.

**함수 `ask`의 인수, `showOk`와 `showCancel`은 \*콜백 함수\* 또는 \*콜백\*이라고 불린다.**

함수를 함수의 인수로 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백 함수의 개념이다. 위 예시에선 사용자가 "yes"라고 대답한 경우 `showOk`가 콜백이 되고, "no"라고 대답한 경우 `showCancel`가 콜백이 된다.

아래와 같이 함수 표현식을 사용하면 코드 길이가 짧아진다.

```javascript
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

ask(
  "동의하십니까?",
  function() { alert("동의하셨습니다."); },
  function() { alert("취소 버튼을 누르셨습니다."); }
);
```

`ask(...)` 안에 함수가 선언된 게 보이시나요? 이렇게 이름 없이 선언한 함수는 *익명 함수(anonymous function)* 라고 부른다. 익명 함수는 (변수에 할당된 게 아니기 때문에) `ask` 바깥에선 접근할 수 없다. 위 예시는 의도를 가지고 이렇게 구현하였기 때문에 바깥에서 접근할 수 없어도 문제가 없다.

자바스크립트를 사용하다 보면 콜백을 활용한 코드를 아주 자연스레 만나게 됩니다. 이런 코드는 자바스크립트의 정신을 대변한다.

**함수는 "동작"을 나타내는 값이다.**

문자열이나 숫자 등의 일반적인 값들은 *데이터*를 나타내지만,함수는 하나의 *동작(action)*을 나타낸다. 동작을 대변하는 값인 함수를 변수 간 전달하고, 동작이 필요할 때 이 값을 실행할 수 있다.

## [함수 표현식 vs 함수 선언문](https://ko.javascript.info/function-expressions#ref-311)

**함수 표현식과 선언문의 차이**

첫 번째는 문법이다.

- *함수 선언문:* 함수는 주요 코드 흐름 중간에 독자적인 구문 형태로 존재한다.

  ```javascript
  // 함수 선언문
  function sum(a, b) {
    return a + b;
  }
  ```

- *함수 표현식:* 함수는 표현식이나 구문 구성(syntax construct) 내부에 생성된다. 아래 예시에선 함수가 할당 연산자 `=`를 이용해 만든 “할당 표현식” 우측에 생성되었다.

  ```javascript
  // 함수 표현식
  let sum = function(a, b) {
    return a + b;
  };
  ```

두 번째 차이는 자바스크립트 엔진이 *언제* 함수를 생성하는지에 있다.

**함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성한다. 따라서 실행 흐름이 함수에 도달했을 때부터 해당 함수를 사용할 수 있다.**

스크립트가 실행되고, 실행 흐름이 `let sum = function…`의 우측(함수 표현식)에 도달 했을때 함수가 생성된다. 이때 이후부터 해당 함수를 사용(할당, 호출 등)할 수 있다.

하지만 함수 선언문은 조금 다른데, **함수 선언문은 함수 선언문이 정의되기 전에도 호출할 수 있습니다.** 따라서 전역 함수 선언문은 스크립트 어디에 있느냐에 상관없이 어디에서든 사용할 수 있다.

이게 가능한 이유는 자바스크립트의 내부 알고리즘 때문이다. 자바스크립트는 스크립트를 실행하기 전, 준비단계에서 전역에 선언된 함수 선언문을 찾고, 해당 함수를 생성한다. 스크립트가 진짜 실행되기 전 "초기화 단계"에서 함수 선언 방식으로 정의한 함수가 생성되는 것이다. 스크립트는 함수 선언문이 모두 처리된 이후에서야 실행된다. 따라서 스크립트 어디서든 함수 선언문으로 선언한 함수에 접근할 수 있는 것이다.

```javascript
sayHi("John"); // Hello, John

function sayHi(name) {
  alert( `Hello, ${name}` );
}
```

함수 선언문, `sayHi`는 스크립트 실행 준비 단계에서 생성되기 때문에, 스크립트 내 어디에서든 접근할 수 있다.

그러나 함수 표현식으로 정의한 함수는 함수가 선언되기 전에 접근하는 게 불가능하다.

```javascript
sayHi("John"); // error!

let sayHi = function(name) {  // (*) 마술은 일어나지 않는다.
  alert( `Hello, ${name}` );
};
```

함수 표현식은 실행 흐름이 표현식에 다다랐을 때 만들어진다. 위 예시에선 `(*)`로 표시한 줄에 실행 흐름이 도달했을 때 함수가 만들어진다. 아주 늦다.

세 번째 차이점은, 스코프다. **엄격 모드에서 함수 선언문이 코드 블록 내에 위치하면 해당 함수는 블록 내 어디서든 접근할 수 있다. 하지만 블록 밖에서는 함수에 접근하지 못한다.**

런타임에 그 값을 알 수 있는 변수 `age`가 있고, 이 변수의 값에 따라 함수 `welcome()`을 다르게 정의해야 하는 상황이다. 그리고 함수 `welcome()`은 나중에 사용해야 하는 상황이라고 가정해 보자.

함수 선언문을 사용하면 의도한 대로 코드가 동작하지 않는다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

// 조건에 따라 함수를 선언함
if (age < 18) {

  function welcome() {
    alert("안녕!");
  }

} else {

  function welcome() {
    alert("안녕하세요!");
  }

}

// 함수를 나중에 호출한다.
welcome(); // Error: welcome is not defined
```

함수 선언문은 함수가 선언된 코드 블록 안에서만 유효하기 때문에 이런 에러가 발생한다.

또 다른 예시를 살펴보자.

```javascript
let age = 16; // 16을 저장했다 가정하자.

if (age < 18) {
  welcome();               // \   (실행)
                           //  |
  function welcome() {     //  |
    alert("안녕!");        //  |  함수 선언문은 함수가 선언된 블록 내
  }                        //  |  어디에서든 유효하다
                           //  |
  welcome();               // /   (실행)

} else {

  function welcome() {
    alert("안녕하세요!");
  }
}

// 여기는 중괄호 밖이기 때문에
// 중괄호 안에서 선언한 함수 선언문은 호출할 수 없다.

welcome(); // Error: welcome is not defined
```

그럼 `if`문 밖에서 `welcome` 함수를 호출할 방법은 없는 걸까? 함수 표현식을 사용하면 가능하다. `if`문 밖에 선언한 변수 `welcome`에 함수 표현식으로 만든 함수를 할당하면 된다

이제 코드가 의도한 대로 동작한다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

let welcome;

if (age < 18) {

  welcome = function() {
    alert("안녕!");
  };

} else {

  welcome = function() {
    alert("안녕하세요!");
  };

}

welcome(); // 제대로 동작한다.
```

물음표 연산자 `?`를 사용하면 위 코드를 좀 더 단순화할 수 있다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

let welcome = (age < 18) ?
  function() { alert("안녕!"); } :
  function() { alert("안녕하세요!"); };

welcome(); // 제대로 동작합니다.
```

**함수 선언문과 함수 표현식 중 무엇을 선택해야 하나요?**

함수 선언문을 이용해 함수를 선언하는 걸 먼저 고려하는 게 좋다. 함수 선언문으로 함수를 정의하면, 함수가 선언되기 전에 호출할 수 있어서 코드 구성을 좀 더 자유롭게 할 수 있다.

함수 선언문을 사용하면 가독성도 좋아진다. 코드에서 `let f = function(…) {…}`보다 `function f(…) {…}` 을 찾는 게 더 쉽기 때문이다. 함수 선언 방식이 더 “눈길을 사로잡는다”. 그러나 어떤 이유로 함수 선언 방식이 적합하지 않거나, (위 예제와 같이) 조건에 따라 함수를 선언해야 한다면 함수 표현식을 사용해야 한다.

## 

> ## [요약](https://ko.javascript.info/function-expressions#ref-312)
>
> - 함수는 값이다. 따라서 함수도 값처럼 할당, 복사, 선언할 수 있다.
> - “함수 선언(문)” 방식으로 함수를 생성하면, 함수가 독립된 구문 형태로 존재하게 된다.
> - “함수 표현식” 방식으로 함수를 생성하면, 함수가 표현식의 일부로 존재하게 된다.
> - 함수 선언문은 코드 블록이 실행되기도 전에 처리됩니다. 따라서 블록 내 어디서든 활용 가능하다.
> - 함수 표현식은 실행 흐름이 표현식에 다다랐을 때 만들어진다.
>
> **함수를 선언해야 한다면 함수가 선언되기 이전에도 함수를 활용할 수 있기 때문에, 함수 선언문 방식을 따르는 게 좋다. 함수 선언 방식은 코드를 유연하게 구성할 수 있도록 해주고, 가독성도 좋다. 마지막으로 함수 표현식은 함수 선언문을 사용하는게 부적절할 때에 사용하는 것이 좋다.**



#### 2.17 화살표 함수 기본

함수 표현식보다 단순하고 간결한 문법으로 함수를 만들 수 있는 방법이 있다.

바로 화살표 함수(arrow function)를 사용하는 것이다. 화살표 함수라는 이름은 문법의 생김새를 차용해 지어졌다고 한다.

```javascript
let func = (arg1, arg2, ...argN) => expression
```

이렇게 코드를 작성하면 인자 `arg1..argN`를 받는 함수 `func`이 만들어진다. 함수 `func`는 화살표(`=>`) 우측의 `표현식(expression)`을 평가하고, 평가 결과를 반환한다.

아래 함수의 축약 버전이라고 할 수 있다.

```javascript
let func = function(arg1, arg2, ...argN) {
  return expression;
};
```

좀 더 구체적인 예시를 살펴보자.

```javascript
let sum = (a, b) => a + b;

/* 위 화살표 함수는 아래 함수의 축약 버전이다.

let sum = function(a, b) {
  return a + b;
};
*/

alert( sum(1, 2) ); // 3
```

보시는 바와 같이 `(a, b) => a + b`는 인수 `a`와 `b`를 받는 함수다. `(a, b) => a + b`는 실행되는 순간 표현식 `a + b`를 평가하고 그 결과를 반환한다.

- 인수가 하나밖에 없다면 인수를 감싸는 괄호를 생략할 수 있다. 괄호를 생략하면 코드 길이를 더 줄일 수 있다.

  예시:

  ```javascript
  let double = n => n * 2;
  // let double = function(n) { return n * 2 }과 거의 동일하다.
  
  alert( double(3) ); // 6
  ```

- 인수가 하나도 없을 땐 괄호를 비워놓으면 된다. 다만, 이 때 괄호는 생략할 수 없다.

  ```javascript
  let sayHi = () => alert("안녕하세요!");
  
  sayHi();
  ```

화살표 함수는 함수 표현식과 같은 방법으로 사용할 수 있다.

아래 예시와 같이 함수를 동적으로 만들 수 있다.

```javascript
let age = prompt("나이를 알려주세요.", 18);

let welcome = (age < 18) ?
  () => alert('안녕') :
  () => alert("안녕하세요!");

welcome();
```

화살표 함수를 처음 접하면 가독성이 떨어진다. 익숙지 않기 때문이다. 하지만 문법이 눈에 익기 시작하면 적응은 식은 죽 먹기가 된다.

함수 본문이 한 줄인 간단한 함수는 화살표 함수를 사용해서 만드는 게 편리하다. 타이핑을 적게 해도 함수를 만들 수 있다는 장점이 있다.

## [본문이 여러 줄인 화살표 함수](https://ko.javascript.info/arrow-functions-basics#ref-5)

위에서 소개해 드린 화살표 함수들은 `=>` 왼쪽에 있는 인수를 이용해 `=>` 오른쪽에 있는 표현식을 평가하는 함수들이다.

그런데 평가해야 할 표현식이나 구문이 여러 개인 함수가 있을 수도 있다. 이 경우 역시 화살표 함수 문법을 사용해 함수를 만들 수 있다. 다만, 이때는 중괄호 안에 평가해야 할 코드를 넣어주어야 한다. 그리고 `return` 지시자를 사용해 명시적으로 아래와 같이 결과값을 반환해 주어야 한다.

```javascript
let sum = (a, b) => {  // 중괄호는 본문 여러 줄로 구성되어 있음을 알려준다.
  let result = a + b;
  return result; // 중괄호를 사용했다면, return 지시자로 결괏값을 반환해주어야 한다.
};

alert( sum(1, 2) ); // 3
```



> ## [요약](https://ko.javascript.info/arrow-functions-basics#ref-6)
>
> 화살표 함수는 본문이 한 줄인 함수를 작성할 때 유용하다. 본문이 한 줄이 아니라면 다른 방법으로 화살표 함수를 작성해야 한다.
>
> 1. 중괄호 없이 작성: `(...args) => expression` – 화살표 오른쪽에 표현식을 둔다. 함수는 이 표현식을 평가하고, 평가 결과를 반환한다.
> 2. 중괄호와 함께 작성: `(...args) => { body }` – 본문이 여러 줄로 구성되었다면 중괄호를 사용해야 한다. 다만, 이 경우는 반드시 `return` 지시자를 사용해 반환 값을 명기해 주어야 한다.





#### 2.18 기본 문법 요약

## [코드 구조](https://ko.javascript.info/javascript-specials#ref-1)

여러 개의 구문은 세미콜론을 기준으로 구분할 수 있다.

```javascript
alert('Hello'); alert('World');
```

줄 바꿈도 여러 개의 구문을 구분하는 데 사용되므로 아래 코드는 정상적으로 동작한다.

```javascript
alert('Hello')
alert('World')
```

이런 동작 방식을 '세미콜론 자동 삽입(automatic semicolon insertion)'이라고 부른다. 그런데 세미콜론 자동 삽입이 동작하지 않을 때도 있다.

```javascript
alert("이 메시지가 출력된 후에 에러가 발생합니다.")

[1, 2].forEach(alert)
```

코딩 컨벤션과 같은 코드 스타일 지침서 대부분은 문장의 끝에 세미콜론을 붙이는 걸 권장한다.

코드 블록(`{...}` )이나 코드 블록과 함께 구성되는 문법(예: 반복문) 끝엔 세미콜론을 붙이지 않아도 괜찮다.

```javascript
function f() {
  // 함수 선언문 끝엔 세미콜론이 필요 없다.
}

for(;;) {
  // 반복문 끝엔 세미콜론이 필요 없다.
}
```

세미콜론이 없어도 되는 자리에 ‘여분의’ 세미콜론을 붙이더라도 해당 세미콜론은 무시되기 때문에 에러가 발생하지 않는다.

## [엄격 모드](https://ko.javascript.info/javascript-specials#ref-2)

모던 자바스크립트에서 지원하는 모든 기능을 활성화하려면 스크립트 맨 위에 `'use strict'`를 적어줘야 한다.

```javascript
'use strict';

...
```

`'use strict'`는 스크립트 최상단이나 함수 본문 최상단에 있어야 한다.

`'use strict'`가 없어도 코드는 정상적으로 동작한다. 다만, 모던한 방식이 아닌 옛날 방식으로 동작한다. '하위 호환성’을 지키면서 말이다. 될 수 있으면 모던한 방식을 사용하는 걸 추천한다.

참고로, 추후에 배우게 될 클래스와 같은 몇몇 모던 기능은 엄격 모드를 자동으로 활성화한다.

## [변수](https://ko.javascript.info/javascript-specials#ref-3)

변수는 아래와 같은 키워드를 이용해 선언할 수 있다.

- `let`
- `const` – 한 번 값을 할당하면 더는 값을 바꿀 수 없는 상수를 정의할 때 쓰인다.
- `var` – 과거에 쓰이던 키워드로 현재는 사용을 지양하고 있다.

변수 이름 명명 규칙은 다음과 같다.

- 숫자와 문자를 사용하되 첫 글자는 숫자가 될 수 없다.
- 특수기호는 `$`와 `_`만 사용할 수 있다.
- 비 라틴계 언어의 문자나 상형문자도 사용할 수 있지만 잘 쓰이진 않다.

자바스크립트는 동적 타이핑을 허용하기 때문에, 자료형을 바꿔가며 값을 할당할 수 있다.

```javascript
let x = 5;
x = "John";
```

자바스크립트는 여덟 가지 기본 자료형을 지원한다.

- 정수와 부동 소수점을 저장하는 데 쓰이는 `숫자형`
- 아주 큰 숫자를 저장할 수 있는 `BigInt형`
- 문자열을 저장하는 데 쓰이는 `문자형`
- 논리값 `true/false`을 저장하는 데 쓰이는 `불린형`
- ‘비어있음’, '존재하지 않음’을 나타내는 `null` 값만을 위한 독립 자료형 `null`
- 값이 할당되지 않은 상태를 나타내는 `undefined` 값만을 위한 독립 자료형 `undefined`
- 복잡한 자료구조를 저장하는 데 쓰이는 `객체형`과 고유한 식별자를 만들 때 사용되는 `심볼형`

`typeof` 연산자는 값의 자료형을 반환해준다. 그런데 두 가지 예외 사항이 있다.

```javascript
typeof null == "object" // 언어 자체의 오류
typeof function(){} == "function" // 함수는 특별하게 취급된다.
```

## [상호작용](https://ko.javascript.info/javascript-specials#ref-4)

호스트 환경이 브라우저인 경우, 다음과 같은 UI 함수를 이용해 사용자와 상호작용할 수 있다.

- [`prompt(question, [default\])`](https://developer.mozilla.org/ko/docs/Web/API/Window/prompt)

  프롬프트 창에 매개변수로 받은 `question`을 넣어 사용자에게 보여준다. ‘확인’ 버튼을 눌렀을 땐 사용자가 입력한 값을 반환해주고, ‘취소’ 버튼을 눌렀을 땐 `null`을 반환한다.

- [`confirm(question)`](https://developer.mozilla.org/ko/docs/Web/API/Window/confirm)

  컨펌 대화상자에 매개변수로 받은 `question`을 넣어 사용자에게 보여줍니다. 사용자가 ‘확인’ 버튼을 누르면 `true`를, 그 외의 경우는 `false`를 반환한다.

- [`alert(message)`](https://developer.mozilla.org/ko/docs/Web/API/Window/alert)

  `message`가 담긴 얼럿 창을 보여준다.

세 함수는 모두 *모달* 창을 띄워주는데, 모달 창이 닫히기 전까지 코드 실행이 중지된다. 사용자는 모달 창 외에 페이지에 있는 그 무엇과도 상호작용할 수 없다.

```javascript
let userName = prompt("이름을 알려주세요.", "영희");
let isTeaWanted = confirm("차 한 잔 드릴까요?");

alert( "방문객: " + userName ); // 영희
alert( "차 주문 여부: " + isTeaWanted ); // true
```

## [연산자](https://ko.javascript.info/javascript-specials#ref-5)

자바스크립트는 아래와 같은 다양한 연산자를 제공한다.

- 산술 연산자

  사칙 연산에 관련된 연산자 `* + - /`와 나머지 연산자 `%`, 거듭제곱 연산자 `**`가 대표적인 산술 연산자에 속한다.이항 덧셈 연산자 `+`는 피연산자 중 하나가 문자열일 때 나머지 하나를 문자형으로 바꾸고 두 문자열을 연결한다.`alert( '1' + 2 ); // '12', 문자열 alert( 1 + '2' ); // '12', 문자열`

- 할당 연산자

  `a = b` 형태의 할당 연산자와 `a *= 2` 형태의 복합 할당 연산자가 있다.

- 비트 연산자

  비트 연산자는 인수를 32비트 정수로 변환하여 이진 연산을 수행한다. 자세한 내용은 [docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)에서 볼 수 있다.

- 조건부 연산자

  조건부 연산자는 자바스크립트 연산자 중 유일하게 매개변수가 3개인 연산자다. `cond ? resultA : resultB`와 같은 형태로 사용하고, `cond`가 truthy면 `resultA`를, 아니면 `resultB`를 반환한다.

- 논리 연산자

  AND 연산자 `&&`와 OR 연산자 `||`은 단락 평가를 수행하고, 평가가 멈춘 시점의 값을 반환한다(꼭 `true`나 `false`일 필요는 없다). NOT 연산자 `!`는 피연산자의 자료형을 불린형으로 바꾼 후 그 역을 반환한다.

- null 병합 연산자

  null 병합 연산자 `??`는 피연산자 중 실제 값이 정의된 피연산자를 찾는 데 쓰인다. `a`가 `null`이나 `undefined`가 아니면 `a ?? b`의 평가 결과는 `a`이고, `a`가 `null`이나 `undefined`이면 `a ?? b`의 평가 결과는 `b`가 된다.

- 비교 연산자

  동등 연산자 `==`는 형이 다른 값끼리 비교할 때 피연산자의 자료형을 숫자형으로 바꾼 후 비교를 진행한다. `null`과 `undefined`는 자기끼리 비교할 땐 참을 반환하지만 다른 자료형과 비교할 땐 거짓을 반환한다.`alert( 0 == false ); // true alert( 0 == '' ); // true`기타 비교 연산자들 `< > <= >=` 역시 피연산자의 자료형을 숫자형으로 바꾼 후 비교를 진행한다.일치 연산자 `===`는 피연산자의 형을 변환하지 않는다. 형이 다르면 무조건 다르다고 평가한다. `null`과 `undefined`는 특별한 값이다. 두 값을 `==` 연산자로 비교하면 `true`를 반환하지만, 다른 값과 비교하면 무조건 `false`를 반환한다.크고 작음을 비교하는 연산자의 피연산자로 문자열이 들어오면 글자 단위로 크기 비교가 이뤄진다. 다른 타입의 값이 들어오면 숫자형으로 형 변환한 후 비교를 진행한다.

- 기타 연산자

  쉼표 연산자 등의 기타 연산자도 있다.

## [반복문](https://ko.javascript.info/javascript-specials#ref-6)

- while, do-while, for 문은 아래와 같이 작성할 수 있다.

  ```javascript
  // 1
  while (condition) {
    ...
  }
  
  // 2
  do {
    ...
  } while (condition);
  
  // 3
  for(let i = 0; i < 10; i++) {
    ...
  }
  ```

- `for(let...)` 안쪽에 선언한 변수는 오직 반복문 내에서만 사용할 수 있다. `let`을 생략하고 기존에 선언되어있는 변수를 사용하는 것도 가능하다.

- 지시자 `break`나 `continue`는 반복문 전체나 현재 실행 중인 반복을 빠져나가는 데 사용된다. 레이블은 중첩 반복문을 빠져나갈 때 사용한다.

## ['switch’문](https://ko.javascript.info/javascript-specials#ref-7)

'switch’문은 `if`문을 사용해 재작성할 수 있다. 'switch’문은 조건을 확인할 때 내부적으로 일치 연산자 `===`를 사용해 비교를 진행한다.

```javascript
let age = prompt('나이를 알려주세요.', 18);

switch (age) {
  case 18:
    alert("Won't work"); // prompt 함수는 항상 문자열을 반환하므로, 이 case문엔 절대 도달할 수 없다.
    break;

  case "18":
    alert("낭랑 18세이시군요!");
    break;

  default:
    alert("어떤 case문에도 해당하지 않습니다.");
}
```

## [함수](https://ko.javascript.info/javascript-specials#ref-8)

세 가지 방법으로 함수를 만들 수 있다.

1. 함수 선언문: 주요 코드 흐름을 차지하는 방식

   ```javascript
   function sum(a, b) {
     let result = a + b;
   
     return result;
   }
   ```

2. 함수 표현식: 표현식 형태로 선언된 함수

   ```javascript
   let sum = function(a, b) {
     let result = a + b;
   
     return result;
   };
   ```

3. 화살표 함수:

   ```javascript
   // 화살표(=>) 우측엔 표현식이 있음
   let sum = (a, b) => a + b;
   
   // 대괄호{ ... }를 사용하면 본문에 여러 줄의 코드를 작성할 수 있음. return문이 꼭 있어야 함.
   let sum = (a, b) => {
     // ...
     return a + b;
   }
   
   // 인수가 없는 경우
   let sayHi = () => alert("Hello");
   
   // 인수가 하나인 경우
   let double = n => n * 2;
   ```

- 함수는 지역 변수를 가질 수 있다. 지역 변수는 함수의 본문에 선언된 변수로, 함수 내부에서만 접근할 수 있다.
- 매개변수에 기본값을 설정할 수 있다. 문법은 다음과 같다. `function sum(a = 1, b = 2) {...}`
- 함수는 항상 무언가를 반환한다. `return`문이 없는 경우는 `undefined`를 반환한다.