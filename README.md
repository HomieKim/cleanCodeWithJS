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





