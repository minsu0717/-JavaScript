# -JavaScript
## 자바스크립트 엔진
자바스크립트 엔진은 기본적으로 하나의 쓰레드에서 동작한다. 하나의 쓰레드를 가지고 있다는 것은 하나의 stack을 가지고 있다는 의미와 같고, 하나의 stack이 있다는 의미는 동시에 단 하나의 작업만을 할 수 있다는 의미이다.

자바스크립트가 동시에 단 하나의 작업만을 한다는데 어떻게 여러가지 작업을 비동기로 작업을 할 수 있을까

그 비밀은 바로 **Event Loop와 Queue**에 있다

## Event Loop와 Queue
- Memory Heap : 메모리 할당이 일어나는 곳( 우리가 프로그램에 선언한 변수, 함수 등이 담겨져 있다 )
- call stack : 코드가 실행될 때 쌓이는 곳 stack 형태로 쌓임
- callback queue : 비동기적으로 실행된 콜백함수가 보관되는 영역이다.

call stack이 빈상태 가 되면, callback queue의 첫번째 콜백을 call stack으로 밀어 넣는다.

이러한 이유로 우리는 자바스크립트에서 비동기 작업을 할 수 있다.

## 호이스팅
선언한 변수 및 함수가 단순히 코드 최상단으로 올라오는 것을 의미

변수나 함수가 실제 코드에서 작성된 위치와 관계없이 선언 단계에서 메모리에 저장되기 때문에 발생

단, 변수를 정의할때 var 키워드를 써야함

    function getX() {
      console.log(x); // undefined
      var x = 100;
      console.log(x) // 100
    }
    getX()

다른 언어의 경우엔, 변수 x를 선언하지 않고 출력하려 한다면 오류를 발생할 것이다. 하지만 자바스크립트에서는 undefined라고 하고 넘어간다.

var x = 100 이 구문에서 var x; 를 호이스트하기 때문이다. 즉, 작동 순서에 맞게 코드를 재구성하면 다음과 같다.

    function getX() {
      var x;
      console.log(x)
      x = 100
      console.log(x)
    }
    getX()

함수가 자신이 위치한 코드에 상관없이 함수 선언문 형태로 정의한 함수의 유효범위는 전체 코드의 맨 처음부터 시작한다.

    foo()
    fucntion foo(){
      console.log('hello')
    }
    //console> hello

foo 함수에 대한 선언을 호이스팅하여 global 객체에 등록시키기 때문에 hello가 제대로 출력된다.

    foo()
    var foo = function() {
      console.log('hello')
    }
    // console> Uncaught TypeError: foo is not a function

이 예제는 함수 리터럴을 할당하는 구조여서 호이스팅이 되지 않는다.

## Closure
쉽게 말해 함수 내부에 만든 지역 변수가 사라지지 않고 계속해서 값을 유지하고 있는 상태

    function fncCount() {
      var count = 0;
      function addCount(){
        count ++
        return count
      }
      return addCount;
    }

    var counter = fncCount();

    console.log(counter()) // 1
    console.log(counter()) // 2
    console.log(counter()) // 3
    
