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

