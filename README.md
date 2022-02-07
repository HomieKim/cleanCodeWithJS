# Clean Code With JS


## 변수 다루기

* **var를 지양하자**
    - var는 함수 스코프, let & const느 블록 단위 스코프를 가집니다.
    - let과 const는 블록 스코프 뿐만 아니라 TDZ(Temporal Dead Zone)이라는 속성 또한 가집니다. 안전한 코드 작성 가능
    - var를 사용하면 중복적으로 선언가능, 값도 중복으로 할당 가능 -> 안전하지 않음
    - let과 const 차이는 재할당 차이

* **function scocp & block scope**
    - var는 함수 단위의 스코프를 사용합니다.
    ```js
    var global = '전역'
    if(global === '전역'){
        var global = '지역';
        console.log(global);  // 지역
    }
    console.log(global); // 지역

    // if문이 함수가 아니기 때문에 전역 범위의 변수에 영향을 끼칩니다.
    // let은 블록 단위 스코프
    ```
    - let 보다는 const를 권장

* **전역 공간 사용 최소화**
    - 전역공간은 window와 global로 나뉜다. (전역공간 == 최상위)
    - 브라우저 환경에서 돌아가는 경우 window가 최상위
    - node.js환경에서는 global이 최상위
    - 자바스크립트는 실행환경에 변경이 가능합니다. (어디서나 접근가능)
    - 코드 파일이 분리되어있어도 전역 공간 사용시 런타임에서 참조가 가능합니다.
    - const 와 let을 사용하는 것이 좋음
    - window, global 조작 x
    - IIFE, Module, closure, 스코프 나누기 등으로 해결 가능

* **임시 변수 제거하기**
    - 임시변수란 특정 scope안에서 전역변수 처럼 활용되는 변수르 말합니다.
    ```js
    function getElements() {
        const result = {} // 임시 객체

        result.title = document.querySelector('.title');
        result.text = document.querySelector('.text');
        result.value = document.querySelector('.value');

        return result;
     }
    // 위 함수는 다음과 같이 수정할 수 있습니다.
    function getElements() {
        return {
            title : document.querySelector('.title'),
            text : document.querySelector('.text'),
            value : document.querySelector('.value'),
        };
     }
    ```
    - 한 함수는 하나의 역할만 할 수 있게 하는 게 좋습니다.
    - 임시변수를 사용해서 함수내부에서 추가적인 로직을 작성하는 것을 지양해야합니다.
    - 함수를 잘게 나누거나 바로 반환 하는 형태로 해결 가능
    - 고차 함수 사용 으로 해결가능(map, filter, reduce ...)
    - 선언형 , 명령형 프로그래밍 공부

* **호이스팅 주의하기**
    - 호이스팅을 간단하게 정의하면 선언과 할당이 분리된 것을 말합니다.
    - var 같은 경우는 runtime 이전에 선언부가 최상단으로 끌어올려짐 (undefined로 초기화, 런타임에 할당됨)
    - 함수 역시 호이스팅 되기 때문에 const로 함수를 선언하고 할당해주는 것을 권장 (함수 표현식)

## 타입 다루기

* **타입검사**
    - typeof 연산자는 피연산자를 평가해서 문자열로 반환해 줍니다.
    - 객체 타입의 경우 typeof 연산자로 다루기 애매함 (ex. 함수는 'function'타입, class도 'function'타입으로 나옴, class경우 생성자 함수로 인식)
    - typeof null 은 'object'로 반환됨(언어적인 오류로 보는게 마땅)
    - js는 동적 타입 언어이기 때문에 동적으로 변하는 타입을 전부 고려하기 어려움
    - instanceof 연산자도 typeof 랑 비슷, 객체의 프로토타입 체인을 검사하는 것
    ```js
    const arr = [];
    const func = function () {};
    const date = new Date();

    arr instanceof Array // true
    func instanceof Function // true
    date instanceof Date // true

    arr instanceof Object // true
    func instanceof Object // true
    date instanceof Object // true
    ```
    - 타입 검사 시 prototype을 이용하는 방법도 있다.
    ```js
    const arr = [];
    const func = function () {};
    const date = new Date();

    arr instanceof Array // true
    func instanceof Function // true
    date instanceof Date // true

    arr instanceof Object // true
    func instanceof Object // true
    date instanceof Object // true

    Object.prototype.toString.call(arr); // '[object Array]'
    Object.prototype.toString.call(func); // '[object Function]'
    Object.prototype.toString.call(date); // '[object Date]'
    ```

* **undefined & null**
    - 의미적인 차이는 undefined 같은 경우는 무언가를 정의했지만 없는 상태고, null은 명시적으로 없다는 것을 보여주는 것
    ```js
    !null // true
    !!null // false

    null === false // false
    !null === true // true
    // null은 수학적으로 0을 명시합니다. , undefined는 NaN
    // undefined는 아무것도 지정하지 않았을 때의 기본값
    ```
    - undefined :  선언했지만 값은 정의되지 않고 할당 x
    - undefined, null -> 값이 없거나 정의 되지 않은 경우
    - undefined 타입은 undefined, null의 타입은 object

* **eqeq 줄이기**
    - eq는 동등연산자를 말합니다(`==`)
    - `===`는 엄격한 동등연산자, 일치연산자 라고 말합니다.
    - 동등 연산자 사용 시 형변환이 일어 납니다.
    - 동등연산자를 이용하는 경우는 에러위험이 있으므로 필요한 경우 수동으로 형변환하여 사용합니다.
    ```js
    const ticketNum = $('#ticketNum'); // 값이 0일 경우라고 가정
    
    // 좋지 않은 코드
    if(ticketNum.value == 0) { ... }
    // 형변환 하여 사용
    if(Number(ticketNum.value) === 0) { ... }
    // 혹은 dom에서 가져올 때 asNumber로 가져올 수 있음
    if(ticketNum.valueAsNumber === 0) { ... }
    ```

* **형변환 주의하기**
    - 참고 사이트 : 자바스크립트 type table (https://dorey.github.io/JavaScript-Equality-Table/)
    ```js
    // 암묵적
    11 + ' 문자와 결합' // 11 문자와 결합

    !!'문자열' // true
    !!''     // false

    // 명시적
    parseInt('9.9999', 10); // 9
    // 참고) parseInt()의 두번째 파라미터는 몇진수로 바꿀지 명시 가능
    ```
    - 코드의 가독성과 안전성을 위해 암묵적으로 이루어지는 형변환을 명시적으로 바꾸어 주는 것이 좋습니다.
    ```js
    // 예
    String(11 + '문자열');
    Boolean('문자열');
    ```

* **isNaN**
    - js에서 숫자를 다루는 방법은 다양합니다. 숫자(`Number`)를 표현하는 방식은 부동소수점 방식을 사용합니다.
    ```js
    // 정수를 다루는 방법 이 따로 존재 예)
    Number.MAX_SAFE_INTEGER // 가장 큰 정수값
    Number.isInteger  // 정수인지 판별

    // is Not A Number => 숫자가 아니다
    ```
    - Number타입을 검사할 때는 isNaN을 사용합니다.
    - isNaN 경우 결과가 반대로 나오기 때문에 헷갈립니다.
    ```js
        isNaN(123) // false => 숫자가 아니다가 아니다 => 숫자가 맞다
    ```
    - isNaN의 경우 느슨한 검사를 수행하기 때문에 Number.isNaN으로 엄격한 검사를 하는 것을 권장 합니다.
    ```js
        isNaN(123+'테스트');  // true
        Number.isNaN(123+'테스트') // false
    ```
    - isNaN의 경우 함수로 넘어오는 파라미터를 Number타입으로 형변환을 시도합니다.
    - Number.isNaN은 주어진 값의 타입이 number이고 NaN일 때만 true를 반환 합니다.
    ```js
    // true 경우
    Number.isNaN(NaN);
    Number.isNaN(Number.NaN);
    Number.isNaN(0 / 0);
    // false 경우
    Number.isNaN(true);
    Number.isNaN(null);
    Number.isNaN(123);
    Number.isNaN("123");
    Number.isNaN("12.3");
    Number.isNaN("");
    ```

## 경계 다루기

* min - max
    - 최솟값과 최댓값을 다룰 때는 상수로 정의해 식별자로 이름만 보고 알아 볼 수 있게 명시적으로 작성합니다.
    - 최소,최대 값을 다룰 때 경계를 모호하지 않게 다루어야 합니다. (포함 되는지 vs 안되는지)
    - 이상, 초과 vs 이하, 미만 경계 결정하기
    ```js
    // 최대, 최소를 식별자에 명시
    const MAX_NUMBER = 10;
    const MIN_NUMBER = 1;
    // 식별자에 경계에 관한 정보를 넣어주는 방법
    const MAX_NUMBER_LIMIT = 10;
    const MIN_NUMBER_IN = 1;
    ```
* begin - end
    - 시작과 끝이 고정되어 있지 않는 경우가 있습니다.(ex. 체크인, 체크아웃 날짜)
    ```js
    // 시작과 끝을 암묵적인 규칙을 통해 명시적으로 표현할 수 있습니다.
    // 예
    function reservationDate(beginDate, endDate){
        //.. some code
    }

    reservationDate('YYYY-MM-DD', 'YYYY-MM-DD');
    ```
* first - last
    - 포함된 양 끝을 의미
    - 데이터가 순차적이지 않고 특정한 규칙이 없을 때 사용하는 네이밍
    - 예를 들어 HTML dom요소 같은 경우도 first child 와 last child 사용가능

* prefix - suffix
    - 프로그래밍 관점에서 prefix의 개념을 사용해서 일종의 약속을 만듬
    - 예를 들어 getter/ setter 나 리액트의 Hook, vue의 스타일가이드, 제이쿼리 등 prefix를 통해 문법을 규칙화 하고 있음
    - 리덕스의 경우도 액션 타입을 정의 할 때 suffix를 사용해서 정의하곤 함

* 매개변수의 순서
    - 호출하는 함수의 네이밍과 인자 순서의 연관성을 고려하여 사용
    ```js
    getRandomNumber(1, 50);
    getDates('2021-01-01', '2021-01-31');
    getSuffleArrya(1,5);
    ```
    - 함수에 넘겨주는 인자의 순서에도 관계성을 있게 하는 것이 좋음(시작-끝, 최대-최소 등등)
    - 함수의 네이밍과 인자들만 보고 함수의 내용을 유추가능하게 하는 것이 좋음
    - 때문에, 함수의 매개변수 자체를 많이 하는 것이 좋지 않음 (2개 넘지 않도록 만드는게 좋음)
    - 인자가 많을 경우 해결법
    ```js
    // 1. 객체로 받기
    // 2. 나머지 매개변수 사용
    // 3. argument 객체 사용
    // 4. wrapping하는 함수 사용(함수 쪼개기)
    ```