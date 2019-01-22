# 191119-

------

### statuments and expression

statements는 문이라서, 조건문, 반복문, 스위치문(if, for, while, switch case)등 이런게 다 statement입니다.
expression은, 프로그램이 operator와 함께 값(value)을 만들어내는 표현입니다.
결국 프로그램은 값을 기반으로 움직이기 때문에 expression이 필요하고, statement로 논리적 움직임이 필요한 셈입니다.

expression은 값을 창출해내거나, 값을 평가합니다. 예를 들면 이런거죠

```javascript
5, 2+5, [1,2,3], {}
```

statement는 어떤 행동을 창출해내거나 무언갈 하게 만듭니다.
예를들면 이런거죠

```javascript
const value = 2;
if(value === 2) {
    console.log('hi');
}
```

여기서 `const value = 2;` 는 statement입니다만, 어떤 값을 다른 변수에 할당하는 statements는 expression으로 간주된다고 합니다.
expression과 statements에 대해서는 여기까지 알아보고, 함수에 대해서 이야기를 나누어 봅시다.

자바스크립트에서는 함수를 선언하는 3가지 방법이 있습니다. 

1. Function Declaration
2. Function Expression
3. Named Function Expression

함수 선언문과 함수선언식, 이름 지어진 함수 선언식이 이와 같습니다.
우선 함수 선언문에 대해서 알아 봅시다.

```javascript
function [이름](param1, param2, ...param3) {
    // 함수로직
}
```

보통 이런 형태를 띄고 있는데, 이 함수 선언문의 한 가지 이점은, 전체의 기능이 호이스팅 된다는 점입니다. 이 덕분에 우리는 함수를 선언하기 전에 실행시킬 수 있게 됩니다.

하지만 자바스크립트 호이스팅에 의지하는 대신 함수를 먼저 선언하고 실행하는 것이 코드를 읽는데에도 어색하지 않고, 모범사례라고도 한답디다. 

다음으로는 함수 선언식에 대해서 알아봅시다.
위에서 말씀드렸듯, 어떤 값을 다른 변수에 할당하는 statements는 expression으로 간주됩니다.

```javascript
var hundred = 100;
var string = 'String';
```

함수 표현식의 경우엔 이름없이 함수를 작성한 다음 변수에 할당합니다.

```javascript
var [이름] = function(param1, param2, ...param3) {
	// 함수로직
}
```

함수 표현식은 정의 될 때까지 함수를 실행할 수 없습니다.

마지막으로 이름 이어진 함수 선언식은 두 접근 방식의 결합과 같습니다.

```javascript
const num1 = 10;
const num2 = 20;
const addVariable = function addFunction(param1, param2) {
    return param1 + param2 ;
}
```

자. 여기서 함수가 더 이상 익명이 아니며 이름이 addFunction 인 사실에 주의를 기울입시다. 또한 변수 addVariable에 할당 되는것도 알아둡시다. 이제는 function 키워드에 이름을 추가했기 때문에 해당 이름으로 함수를 실행할 수는 없습니다.

```javascript
const result = addFunction(10, 20);
console.log(result); //ReferenceError: addFunction is not defined
```

오히려 할당 된 변수 이름 인 addVariable만이 이를 참조하고 실행할 수 있습니다.

```javascript
const result = addVariable(10, 20);
console.log(result); //30
```

고려해야할 사항이 있습니다.

첫번째입니다.

```javascript
const num1 = 10;
const num2 = 20;
const addVariable = function addFunction(param1, param2) {
    return param1 + param2;
}

const result1 = addVariable(num1, num2);
const result2 = addFunction(num1, num2);

//여기서는 addFunction 이 addVariable대신 콜스택에 쌓이게 됩니다.

const addVariable2 = function(param1, param2) {
    return param1 + param2;
}

const result3 = addVariable2(num1, num2);

//여기서는 addVariable2 가 콜스택에 쌓입니다.
```

두번째입니다.

정의 된 함수 표현식의 경우 외부에서 addFunction ()을 호출하면 오류가 발생합니다. 하지만, 함수 표현식의 내부에서는 그 함수를 사용할 수 있습니다.

```javascript
const num1 = 10;
const num2 = 20;
const addVariable = function addFunction(param1, param2) {
   let res = param1 + param2;
   if (res === 30) {
        res = addFunction(res, 10);
   }
   return res;
}
const result = addVariable(num1, num2); // ==> 40
```

이 경우에 res가 30인지 하고 만약 그렇다면 동일한 함수를 다시 호출해서 30과 10을 매개변수로 하는 addFunction을 호출하여 최종적으로 40을 출력합니다.

이상입니다.