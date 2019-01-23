# **Scope**

### The set of rules that determines where and how a variable (identifier) can be looked-up.  (변수(=식별자)를 어디서 어떻게 찾아야 하는지 결정짓는 규칙))

if(!scope) 식별자 이름 충돌남...


## **스코프 구분**

![image](https://user-images.githubusercontent.com/40848630/51501105-f2e7cc00-1e13-11e9-9413-cb705544d8ed.png)


### **Global Scope (전역 스코프)**
- 코드 어디에서나 해당 변수 참조 가능 (default scope === root scope) 

- 함수 바깥이나 중괄호 바깥에 선언된 변수 (전역 변수)

- 변수 이름 충돌 방지를 위해 전역 변수는 선언하지 않는 게 좋음

### **Local Scope (지역 스코프)**
- 함수 코드 블록이 만든 스코프로 함수 자신과 하위 함수에서만 참조 가능

- 지역(함수) 내에서 선언된 변수 (지역 변수)

- function scope && block scope

    - **function scope (함수 스코프)**
    
        함수 내부에서 선언된 변수는 선언한 변수 내부에서만 접근할 수 있는 변수
        
        함수 바깥에서는 해당 변수에 접근할 수 없음  
        
    - **block scope (함수 스코프)**
    
        중괄호({}) 내부에서 const / let 으로 변수를 선언하면 중괄호 블록 내부에서만 접근할 수 있음 


```javascript
var a = 1; 

function foo(){
  var b = 2;
  console.log(a) // 1;
}

foo();
console.log(b); // b is not defined
```
parent scope는 child scope `var b`에 접근할 수가 없지만 child scope은 parent scope에 접근가능 하므로 `a`를 찍을 수 있음


```javascript
var a = 1;

function foo(){
  var a = 2;
  console.log(a) // 2;
}

foo();
```
하면 scope conflict이 일어난다. `foo()`이후에 `console.log(a)`하면 `1`이 찍힘

전역변수 `a`와 지역변수 `a`가 중복 선언됨. 전역 영역에서는 전역변수만이 참조 가능하고 함수 내 지역 영역에서는 전역과 지역 변수 모두 참조 가능하나 위 예제와 같이 변수명이 중복된 경우, 지역변수를 우선하여 참조한다.


```javascript
function foo(){
    a = 2; 
}

foo();
console.log(a) // 2;
```
엔진이 root scope variable을 만들게됨. polluting root scope. -> 'use strict'하면 예방할 수 있음


```javascript
'use strict'
function foo(){
    a = 2; 
}

foo();
console.log(a) // Uncaught ReferenceError: a is not defined
```


## **스코프 레벨**

### **함수 레벨 스코프 && 블록레벨 스코프**

```javascript
function foo() {
    if (true) {
        var color = 'blue';
    }
    console.log(color); // blue
}
foo();
```

`var`로 선언된 변수나 `함수 선언식`으로 만들어진 함수는 `함수 레벨 스코프`를 갖는다.

=> 함수 내부 전체에서 유효한 식별자가 됨 (위 코드에서 blue 출력)

=> 만약 `블록 레벨 스코프`였다면 `var color`는 `if`문이 끝날 때 파괴되어 에러 발생

```javascript
function foo() {
    if(true) {
        let color = 'blue';
        console.log(color); // blue
    }
    console.log(color); // ReferenceError: color is not defined
}
foo();
```


## **Lexical Scope**

![image](https://user-images.githubusercontent.com/40848630/51502295-cbdfc900-1e18-11e9-8094-6790621254bd.png)

1. **토크나이징/렉싱** - 코드를 잘게 나누어 토큰으로 만든다.
2. **파싱** - 나눈 토큰을 의미있게 AST(Abstract Syntax Tree)라는 트리로 만든다.
3. **코드생성** - AST를 기계어로 만든다.

렉시컬 스코핑 언어 === 정적 스코핑 <-> 다이나믹 스코핑 (가장 최근에 실행됐던 함수의 스코프를 따라감, 함수의 실행순서에 따라 스코프가 바뀌는 것)

즉, 렉스컬 스코프는 1단계 렉싱과정에서 정의되는 스코프를 말한다.

```javascript
var x = 'global'

function test1() {
    var x = 'local';
    test2();
}

function test2() {
   console.log(x);
}

test1(); //global
test2(); //global
```

`test2()`의 상위 스코프가 무엇인지에 따라 값이 결정됨
 - 함수를 어디서 호출하였는지에 따라 상위 스코프를 결정 --> test1과 전역 스코프
 - 함수를 어디서 선언하였는지에 따라 상위 스코프를 결정 --> 전역 스코프

![image](https://user-images.githubusercontent.com/40848630/51502616-275e8680-1e1a-11e9-98c5-f789c3e2e66f.png)

```javascript
// JavaScript with Dynamic Scope
function foo() {
  console.log(x);
}

function bar() {
  var x = 15;
  foo();
}

var x = 10;
foo(); // 10
bar(); // 15
```
  

### **Reference**

1. You-Dont-Know-JS/scope & closures https://github.com/getify/You-Dont-Know-JS/tree/master/scope%20%26%20closures

2. What is Lexical Scope Anyway? http://astronautweb.co/javascript-lexical-scope/

3. 자바스크립트의 스코프와 클로저 https://meetup.toast.com/posts/86

4. Scope and the JavaScript Compiler — Kyle Simpson — Frontend Masters
 https://www.youtube.com/watch?v=nRZri_CHqnA