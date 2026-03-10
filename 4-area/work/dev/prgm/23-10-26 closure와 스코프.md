--- 

creation date: 23-10-26 
modification date: 23-10-26 
tag: 

---


https://meetup.nhncloud.com/posts/86
https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/JavaScript#closure


### scope
- js는 함수 레벨, 블록 레벨의 렉시컬 스코프 규칙을 따름. (블록레벨은 es6)


### 함수 레벨 스코프
- var 변수, 함수 선언식은 함수레벨 스코프
```js
function foo() {
    if (true) {
        var color = 'blue';
    }
    console.log(color); // blue
}
foo();
```

- var color는 블록레벨 스코프 ㄴㄴ임. 함수 레벨 스코프. 
	- 블록 레벨이었다면 color 변수는 if문 끝에서 없어짐

### 블록 레벨 스코프
- let, const는 블록 레벨 스코프

```js
function foo() {
    if(true) {
        let color = 'blue';
        console.log(color); // blue
    }
    console.log(color); // ReferenceError: color is not defined
}
foo();
```

### lexical scope vs dynamic scope


```js

var x = 'aaa'

function foo() {
  var x = 'bbb'
  bar()
}

function bar() {
	console.log(x)
}

foo()
bar()
```

렉시컬 스코프의 좋은 예시


### 중첩스코프 (스코프 체인, 스코프 버블)

```js

var x = 'aaa'

function foo() {
	var y = 'bbb';
	functino bar() {
		var z = 'ccc'
		console.log(x)
		console.log(y)
		console.log(z)
	}
	bar()
}

foo()

```


### 클로저
- 스코프를 기억하는 함수?
- 환경(렉시컬 환경)을 기억하는 함수?
- 자유변수를 기억하는 함수?

- 종합하면, 함수가 무언가를 기억하고 다시 사용. 
- 클로저 = 함수 + 함수를 둘러싼 환경 (렉시컬 스코프)
	- 함수를 만들고 그 함수 내부 코드의 스코프를 함수 생성 당시의 렉시컬 스코프로 고정
- 자바스크립트의 클로저
	- 클로저는 함수 생성 시점에 생성
	- = 함수가 생성될 떄 렉시컬 환경을 클로저하여 실행될 떄 이용
	- 



```js
function foo() {
    var x = 'a';
    function bar() {
        console.log(color);
    }
    bar();
}
foo();
```

- bar함수는 클로저?
	- bar는 자신의 렉시컬 스코프 체인을 통해 foo의 x를 참조. 즉, foo 스코프를 외부 스코프 참조로 저장
	- 그래서 클로저일까? ㄴㄴ
- foo밖으로 나오지 않았기 떄문에 클로저 아님.

```js
var x = 'a';
function foo() {
	// 2
    var x = 'b'; 
    function bar() {
	    // 1
        console.log(x); 
    }
    return bar;
}
// 3
var baz = foo();
// 4
baz(); 
```

- bar는 외부 환경 참조로 foo의 환경을 저장
- bar를 global의 baz란 이름으로 저장
- global에서 baz(bar)를 호출 
- bar는 자신의 스코프에서 x를 찾는다.
- 없음. 자신의 외부 환경 참조를 찾는다.
- foo의 스코프를 뒤진다. b값이 찾아짐

- Foo의 렉시컬환경 인스턴스는 Foo() 실행 끝난 후 GC가 회수할까?
	- ㄴㄴ bar는 여전히 바깥 렉시컬 환경인 Foo의 렉시컬 환경을 참조한다. bar는 baz가 여전히 참조하고 있기 때문.


#### 개 유명한 반복문 클로저

```js
function count() {
    var i;
    for (i = 1; i < 10; i += 1) {
        setTimeout(function timer() {
            console.log(i);
        }, i*100);
    }
}
count();
```

- 0.1초가 지날 동안 이미 i는 10이됨.
- 의도대로 1~9 찍히게 하려면?
	- 새로운 스코프를 추가하여 반복 시마다 그곳에 따로 값을 저장
	- ES6에서 추가된 블록 스코프를 이용하는 방식
	- 


```js
 function count() {
     var i;
     for (i = 1; i < 10; i += 1) {
         (function(countingNumber) {
             setTimeout(function timer() {
                 console.log(countingNumber);
             }, i * 100);
         })(i);
     }
 }
 count();

 function count() {
     'use strict';
     for (let i = 1; i < 10; i += 1) {
         setTimeout(function timer() {
             console.log(i);
         }, i * 100);
     }
 }
 count();
```



































