# 파트 1

### 코어 자바스크립트 

---

#### 1.1 자바스크립트란?

##### 간략한 히스토리

웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는 경량 프로그래밍 언어로 시작했다, 그렇기에 초창기 자바스크립트는 웹페이지의 보조적인 기능을 수행하기 위해 한정적인 용도로만 사용되었고, 해당 시기에 대부분의 로직은 주로 웹 서버에서 실행되었으며, 브라우저는 서버로부터 전달받은 HTML과 CSS를 단순히 렌더링하는 수준이었다. 또한 표준화 되지 못해 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 <strong>크로스 브라우징 이슈</strong>가 발생하기 시작했고, 결과적으로 모든 브라우저에서 정상적으로 동작하는 웹페이지를 개발하기가 무척 어려워졌다. 이에 비영리 표준화 기구인 ECMA 인터내셔널을 통해 표준화가 되었으며, 많은 기능의 추가로 인해 비약적인 발전을 함

> ❗<strong>Rendering(렌더링)</strong>: HTML, CSS, 자바스크립트로 작성된 문서를 해석해서 브라우저에 시각적으로 출력하는 것을 말함

> ❗<strong>Server Side Rendering(SSR)</strong>: 서버에서 데이터를 HTML 로 변환해서 브라우저에게 전달하는 과정을 말함

> ❗**파싱(parsing: 구문 분석)**: 하나의 프로그램을 런타임 환경(예를 들면, **브라우저** 내 자바스크립트 엔진)이 실제로 실행할 수 있는 내부 포맷으로 분석하고      변환하는 것을 말함

> ❗자바스크립트 엔진과 동작원리: [참고 링크](https://medium.com/humanscape-tech/javascript-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC%EB%A5%BC-%EC%82%B4%ED%8E%B4%EB%B4%85%EC%8B%9C%EB%8B%A4-aef465c9c43)
>

#### 1.2 매뉴얼과 명세서

##### 자바스크립트는 끊임없이 발전하는 언어다보니, 새로운 기능이 정기적으로 추가되어 매뉴얼과 명세서의 적극적인 활용이 필요함

<strong>명세서</strong>: [ECMA-262 명세서(specification)](https://www.ecma-international.org/publications/standards/Ecma-262.htm)는 자바스크립트와 관련된 가장 심도 있고 상세한 정보를 담고 있는 공식 문서입니다. 이 명세서에서 자바스크립트라는 언어를 정의

<strong>매뉴얼</strong>: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference Mozilla 재단이 운영하는 **MDN JavaScript Reference**엔 다양한 예제와 정보가 있으며, 특정 함수나 메서드에 대한 깊이 있는 정보를 얻고 싶다면 해당 사이트를 확인하면 많은 도움이 됨

#### 1.3 코드 에디터

<strong>Integrated Development Environment, IDE(통합개발환경)</strong>: 보통 '프로젝트 전체’를 관장하는 다양한 기능을 제공하는 강력한 에디터를 지칭, 수많은 파일로 구성된 프로젝트를 불러오고, 파일 간의 탐색 작업이 수월해짐. 또한 단순히 열려있는 파일뿐만 아니라 전체 프로젝트에 기반한 자동 완성 기능도 사용할 수 있으며,  [git](https://git-scm.com/)과 같은 버전 관리 시스템, 테스팅 환경 등, '프로젝트 수준’의 작업도 IDE에서 할 수 있다.

<strong>lightweight editor(경량 에디터)</strong>: 파일을 열고 바로 수정하고자 할 때 주로 사용하며, 다양한 플러그인을 지원, 디렉터리 레벨 문법 분석기나 자동완성기능 등을 플러그인을 설치해 사용할 수 있으며, 플러그인을 사용하면 경량 에디터에서도 IDE 못지않게 다양한 기능을 사용할 수 있다. 요즘엔 경량 에디터와 IDE 사이의 엄격한 구분이 사라져가는 추세.

#### 1.4 개발자 콘솔

 브라우저는 스크립트에 문제가 있어서 에러가 발생해도 이를 사용자에게 직접 보여주지 않는다. 그렇기에 브라우저에는 '개발자 도구'라는 것이 내장되어 있으며, 해당 도구를 이용하면 에러를 확인할 수 있고, 스크립트에 대한 쓸만한 정보도 얻을 수 있다.

![img](https://ko.javascript.info/article/devtools/chrome.png)

> ❗**ReferenceError(예외 처리): OO is not defined**
>
>   -> OO 부분을 잘못 입력했을 때 주로 발생하며 해당 부분을 수정하면 대부분 해결 됨

> ❗**SyntaxError(구문 오류): Invalid or unexpected token**
>
>   -> token(기호)을 잘못 입력했을 때 발생되며, 예를들어 문자열을 입력하기 위해 따옴표를 열었으나 따옴표를 마지막에 닫지 않았을 때 주로 발생.

---

### 자바스크립트 기본

#### 2.1 Hello, world!

<strong> 2.1.1 html 파일을 통한 자바스크립트를 실행하는 방법</strong>

```<script>```태그를 이용!!!

여러가지 방법이 존재하지만 외부 자바스크립트 파일을 import할 시에는 html파일에서 ```head```태그에 ```<script src="path/to/외부스크립트파일이름.js"></script>``` 와 같이 입력하는 방법을 많이 사용

> ❗```<script>``` 태그는 HTML 문서 상 어디에 위치해야 할까?
>
> ```<head>``` 태그 또는 ```<body>``` 태그 어느 위치던 크게 상관없지만, 웹 검색이나 서적마다 작성 위치가 다르고 관련해서 전혀 언급이 없음
>
> **브라우저 동작 방식**: [참고 링크](https://blog.martinwork.co.kr/javascript/2018/10/13/how-js-works-in-browser.html)
>
> 1. HTML을 읽기 시작
>
> 2. HTML을 파싱
>
> 3. DOM트리를 생성
>
> 4. Render트리(DOM tree + CSS에 CSSOM 트리 결합)가 생성
>
> 5. 화면에 표시
>
>    ![img](https://media.vlpt.us/post-images/takeknowledge/aea046b0-2404-11ea-addc-59a0f147306b/image.png)
>
>    HTML을 파싱한 다음 DOM 트리를 생성하는데, 브라우저는 HTML 태그들을 읽어나가는 도중 `<script>` 태그를 만나면 파싱을 중단하고 javascript 파일을 로드 후 javascript 코드를 파싱하기 시작하고, 해당 작업이 완료되면 그 후에 HTML 파싱이 계속된다.
>
>    
>
>    이로인해 HTML태그들 사이에 script 태그가 위치하면 두가지 문제가 발생하는데,
>
>    1. HTML을 읽는 과정에 스크립트를 만나면 중단 시점이 생기고 그만큼 화면에 표시되는 것이 지연
>
>    2. DOM 트리가 생성되기전에 자바스크립트가 생성되지도 않은 DOM의 조작을 시도시에 문제가 발생
>
>          **그렇기 때문에 위와 같은 상황을 막기 위해 script 태그는 body 태그 최하단 에 위치하는 게 가장 좋다**

#### 2.2 코드 구조 ####

**Expression(표현식)**: 값을 만들어내는 간단한 코드

```j
123
11 + 12 + 13 * 14
'Hello'
```

**Statement(문)**: 어떤 작업을 수행하는 문법 구조(syntax structure)와 명령어(command)를 의미하는데, 쉽게 말해 하나 이상의 표현식이 모이면 Statement(문)이 되며, 하나의 Expression(표현식)도 문장의 종결을 의미하는 세미콜론 또는 줄바꿈을 넣으면 Statement(문)라고 부른다.

**Program(프로그램)**: Statement(문)이 모여 Program(프로그램)이 된다.

> ❗ Javascript에서 Statement(문)의 종결을 의미하기 위해 끝에 semicolon(세미콜론)을 넣는다. 또한 줄 바꿈이 있으면 이를 ‘암시적’ 세미콜론으로 해석합니다. 이런 동작 방식을 [세미콜론 자동 삽입(automatic semicolon insertion)](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion)이라 부르며 대부분의 경우에는 줄 바꿈은 semicolon(세미콜론)을 의미하지만, **항상** 을 의미하진 않는다.
>
> 그렇기 때문에, 줄 바꿈으로 Statement(문)를 나눴더라도, Statement(문) 사이엔 semicolon(세미콜론)을 넣는 것이 좋으며 자바스크립트 커뮤니티에서도 이를 규칙으로 정해 권장하고 있다.
>
> **세미콜론은 *생략할 수 있다.* 하지만 세미콜론을 사용하는 것이 여러모로 안전하므로 이를 기억하고 따르도록 하자!!**

#### 2.3 엄격 모드

ECMAScript5(ES5)부터 만들어진 기능이며, 새로운 기능들을 ES5 이전 하위 호환성을 위해 ```use strict``` 키워드를 통해 활성화 시에만 사용가능하도록 하게 만든 Mode가 바로 Strict Mode(엄격 모드)다. 특히 JavaScript 코드에 더욱 엄격한 오류 검사를 적용하여 기존에 무시되던 오류를 발생시킬 가능성이 높거나 Javascript 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

> ```"use strict"```는 반드시 최상단에 위치시켜야 한다.**
>
>  `"use strict"`**를 취소할 방법은 없기 때문에 적용 시 유의해야 한다.**
>
> ❗ ```"use strict"```를 반드시 사용해야 할까?
>
> > 모던 자바스크립트는 '클래스’와 '모듈’이라 불리는 진일보한 구조를 제공하는데, 이 둘을 사용하면 `"use strict"`가 자동으로 적용된다. 따라서 이 둘을 사용하고 있다면 스크립트에 `"use strict"`를 붙일 필요가 없다. 
> >
> > **결론: 코드를 클래스와 모듈을 사용해 구성한다면 `"use strict"`를 생략해도 된다.** 

> ❗ 흔하진 않지만, 오래된 브라우저에서 'use strict'를 사용할 수 없다면, 아래 코드와 같이 즉시실행함수를 통해 엄격모드를 사용 가능한다.
>
> ```javascript
> (function() {
>   'use strict';
> 
>   // ...테스트하려는 코드...
> })()
> ```

#### 2.4 변수와 상수

> ❗**선언**: 상수나 변수를 만드는 과정이며, **변수와 상수는 같은 이름으로 두 번이상 선언하면 에러가 발생한다.**
>
> ❗변수와 상수 이름 규칙
>
> > 변수명에는 오직 문자와 숫자, 그리고 기호 `$`와 `_`만 들어갈 수 있다.
> >
> > 첫 글자는 숫자가 될 수 없다.
> >
> > 대·소문자 구별
> >
> > reserved name(예약어)을 사용할 수 없다.

> **Variable(변수)**: 변할 수 있는 수(위가 뚫려 있어서 값을 꺼내서 버리고 다시 넣을 수 있는 상자)또는 저장소
>
> ```let``` 키워드를 통해 선언(`var` – 오래된 변수 선언 키워드이며, 현재는 잘 사용하지 않는다, 변수를 중복해서 선언할 수 있다는 위험성, 변수가 속하는 범위가 애매한다는 이유로 ```let``` 키워드가 등장하면서 대체 됨)
>
> ``` javascript
> let message;
> 
> message = 'Hello!';
> 
> message = 'World!'; // 값이 변경
> 
> alert(message);
> ```

> **Constant**(상수): 항상 같은 수(한번 값을 넣으면 꺼낼 수 없는 모든 면이 막힌 단단한 상자)또는 저장소
>
> ```const``` 키워드를 통해 선언
>
> ``` javascript
> const myBirthday = '18.04.1982';
> 
> myBirthday = '01.01.2001'; // error, can't reassign the constant!
> ```
>
> > ❗상수관련 에러 종류
> >
> > > **Identifier has already declared **
> > >
> > > => 같은 이름으로 상수를 한번 더 선언시 해당 오류 발생
> > >
> > > ``` javascript
> > > > const name = "name이란 변수 선언"
> > >   undefined
> > > > const name = "name이란 변수를 한번 더 선언!"
> > >   Uncaught SyntaxError: Identifier 'name' has already been declared
> > > ```
> > >
> > > **Missing initializer in const declaration**
> > >
> > > => 상수는 한 번만 선언할 수 있으므로 선언할 때 반드시 값을 함께 지정해줘야 함, 만약 상수를 선언할 때 값을 지정해주지 않는다면 해당 오류가 발생
> > >
> > > ``` javascript
> > > > const pi
> > >   Uncaught SyntaxError: Missing initializer in const declaration
> > > ```
> > >
> > > **Assignment to constant variable** 
> > >
> > > => 한 번 선언된 상수의 자료는 변경할 수 없다. 만약 값을 변경하면 해당 오류가 발생
> > >
> > > ``` javascript
> > > > const name = "name이라는 이름의 상수를 선언"
> > >   undefined
> > > > name = "이 값을 변경하면?"
> > >   TypeError: Assignment to constant variable
> > > ```

#### 2.5 자료형 ####

**Data type(자료형)**: 프로그래밍에서 프로그램이 처리할 수 있는 모든 것을 Data(자료)라고 부르며, 자료 형태에 따라 나눠 놓은 것을 Data Type(자료형)이라 한다.

> Javascript에서 제공하는 Data type(자료형)은 ```number, string, boolean, null, undefined, obeject, symbol```이 있으며, 가장 기본적이면서도 많이 사용하는 자료형은 ```number, string, boolean``` 이다.

**number data type(숫자 자료형)**: Javascript는 소수점이 있는 숫자와 없는 숫자를 모두 같은 자료형으로 인식함.

> ``` javascript
> > 123
> 123
> > 123.123 // 소숫점이 있던 없든 모두 같은 숫자 자료형
> 123.123
> ```

**boolean data type(불 자료형)**: ```true``` 와 ```false``` 두 값만 가지는 자료형이며, 두 대상을 비교할 수 있는 ```비교 연산자```를 사용해도 해당 자료형을 만들 수 있다.

> | **비교 연산자** |                   **설 명**                   |
> | :-------------: | :-------------------------------------------: |
> |  **`==, ===`**  |    양쪽이 같다, 양쪽이 같다(자료형도 일치)    |
> |  **`!=, !==`**  | 양쪽이 다르다, 양쪽이 다르다(자료형도 불일치) |
> |     **`>`**     |                왼쪽이 더 크다                 |
> |     **`<`**     |               오른쪽이 더 크다                |
> |    **`>=`**     |             왼쪽이 더 크거나 같다             |
> |    **`<=`**     |            오른쪽이 더 크거나 같다            |
>
> ``` javascript
> > 12 > 123
> false
> > 10 === 10
> true
> > 10 === '10'
> false
> ```

**null data type(널 자료형)**: ‘존재하지 않는(nothing)’ 값, ‘비어 있는(empty)’ 값, ‘알 수 없는(unknown)’ 값을 나타내는 데 사용하는 자료형

> ``` javascript
> > let age = null
> ```

**undefined data type(Undefined 자료형)**: '값이 할당되지 않은 상태’를 나타낼 때 사용하는 자료형

> ``` javascript
> > let age // 변수는 선언했지만, 값을 할당하지 않았다면 해당 변수에 undefined가 자동으로 할당
> undefined
> > let name = 'Jihun Kim'
> > name = undefined
> undefined
> ```

**Object and symbol(객체와 심볼 자료형)**: `객체(object)`형은 특수한 자료형이며, 객체형을 제외한 다른 자료형은 문자열이든 숫자든 한 가지만 표현할 수 있기 때문에 원시(primitive) 자료형이라 부른다. `심볼(symbol)`형은 객체의 고유한 식별자(unique identifier)를 만들 때 사용함

> ❗typeof 연산자 
>
> `typeof` 연산자는 두 가지 형태의 문법을 지원한다.
>
> > 연산자 형태: `typeof x`
> >
> > 함수 형태: `typeof(x)`
> >
> > ``` javascript
> > typeof undefined // "undefined"
> > 
> > typeof 0 // "number"
> > 
> > typeof 10n // "bigint"
> > 
> > typeof true // "boolean"
> > 
> > typeof "foo" // "string"
> > 
> > typeof Symbol("id") // "symbol"
> > 
> > typeof Math // "object" 
> > 
> > typeof null // "object" (Javascript 설계 실수이며 현재는 이전 개발된 프로그램과의 호환성등으로 수정하지 않음)
> > 
> > typeof alert // "function"
> > ```

> ❗template literal(템플릿 리터럴) or template strings(템플릿 문자열)
>
> 과거(ES6 이전) Javascript는 문자열 내부에 표현식을 삽입할 때 다음과 같은 문자열 연결 연산자(+)를 사용해야 했음
>
> ``` javascript
> > console.log('표현식 123 + 456의 값은 ' + (123 + 456) + '입니다!')
> 표현식 123 + 456의 값은 579입니다!
> ```
>
> 이런 작성법에 문제가 있는 건 아니지만, 표현식을 많이 결합할수록 코드가 복잡해진다. 그렇기 때문에 Javascript에서는 ```template literal(템플릿 리터럴)```이라는 기능이 추가되어 코드를 간단하게 작성할 수 있다.
>
> template literal(템플릿 리터럴)은 backtick(`)기호로 감싸 만들며,  문자열 내부에  ${...}기호를 사용하여 표현식을 넣으면 표현식이 문자열 안에서 계산된다.
>
> ``` javascript
> > console.log(`표현식 123 + 456의 값은 ${123 + 456}입니다!`)
> 표현식 123 + 456의 값은 579입니다!
> ```
>
> 

#### 2.6 alert, prompt, confirm을 이용한 상호작용 ####

> 브라우저 환경에서 사용되는 최소한의 사용자 인터페이스 기능, 세 함수들은 모두 모달 창이라는 것을 띄워주는데, 모달 창이 떠 있는 동안은 스크립트의 실행이 일시 중단돠며 사용자가 창을 닫기 전까진 나머지 페이지와 상호 작용이 불가능하다.

**alert**: 이 함수가 실행되면 사용자가 ‘확인(OK)’ 버튼을 누를 때까지 메시지를 보여주는 창이 계속 떠있게 되며, 메시지가 있는 작은 창은 *모달 창(modal window)* 이라고 부른다. '모달’이란 단어엔 페이지의 나머지 부분과 상호 작용이 불가능하다는 의미가 내포되어 있으며 사용자는 확인 버튼을 누르기 전까지, 모달 창 바깥에 있는 버튼을 누른다든가 하는 행동을 할 수 없다.

> modal window(모달 창)
>
> > 모달 창의 위치는 브라우저가 결정하는데, 대개 브라우저 중앙에 위치한다.
> >
> > 모달 창의 모양은 브라우저마다 다릅니다. 개발자는 창의 모양을 수정할 수 없다.

**prompt**: 이 함수가 실행되면 텍스트 메시지와 입력 필드(input field), 확인(OK) 및 취소(Cancel) 버튼이 있는 모달 창을 띄워준다.

> 브라우저에서 제공하는 `prompt` 함수는 두 개의 인수를 받습니다.
>
> ``` javascript
> result = prompt(title, [default])
> ```
>
> **title**: 사용자에게 보여줄 문자열 **default**: 입력 필드의 초깃값(선택값)
>
> `prompt` 함수는 사용자가 입력 필드에 기재한 문자열을 반환하며, 사용자가 입력을 취소한 경우는 `null`이 반환된다.

> ❗**인수를 감싸는 대괄호 `[...]`의 의미**
>
>   `default`를 감싸는 대괄호는 이 매개변수가 필수가 아닌 선택값이라는 것을 의미한다.

**confirm**: 이 함수는 매개변수로 받은 `question(질문)`과 확인 및 취소 버튼이 있는 모달 창을 보여주며, 사용자가 확인버튼를 누르면 `true`, 그 외의 경우는 `false`를 반환한다.

> ❗세 사용자 인터페이스 함수의 출력 결과 정리
>
> **alert**: 확인 버튼을 누를 때까지 메세지를 출력해준다.
>
> **confirm**: 사용자가 [확인]을 선택하면 true를 [취소]를 선택하면 false 값을 반환한다.
>
> **prompt**: 사용자가 첫번째 인수에 값을 입력할 경우 해당 값을 반환하며, [취소]를 선택하면 ```null```을 반환한다.

