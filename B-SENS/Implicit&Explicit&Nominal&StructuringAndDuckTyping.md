# Implicit, Explicit, Nominal, Structuring and DuckTyping

## 1. implicit conversion of values(값의 암시적 변환)

javascript는 값의 타입에 관대한 언어입니다. 숫자가 아닌 다른 타입의 값이더라도 계산에 쓰이는 연산자가 사용된 경우에 숫자 타입으로 다뤄집니다.

~~~
'2' - '1'
// 1
'2' * '1'
// 2
~~~

그러나 숫자 타입이 아닌 문자열로 변환되는 경우가 있는데요. 이 상황에서 문제가 발생합니다.

~~~
'1' + '2'
//"12"
~~~

## 1-1 Boolean 암시적 형변환

Boolean 변환시 false 반환되는 값들은 다음과 같습니다.

- undefined, null
- Boolean: false
- Number: -0, +0, NaN
- String: ''

다른 모든 값들은 true로 반환됩니다. 

## 1-2 문자열 암시적 형변환

'+'는 두가지 역할이 있습니다.
- 숫자 값 더하기 연산
- 문자열 합치기

문자열에서 '+' 연산자가 적용되기 때문에 생기는 문제가 되는 케이스가 있습니다. 
~~~
'1'+'1'
// "11"
~~~

또한 형변환을 놓치게 되면 나타나는 오류 케이스들도 있습니다. 

~~~
> Boolean(false)  // truthy?
false
> String(false)
'false'
> Boolean('false')  // truthy?
true

> Boolean(undefined)  // truthy?
false
> String(undefined)
'undefined'
> Boolean('undefined')  // truthy?
true
~~~

## 1-3 객체의 암시적 형변환(chrome 기준)

객체의 암시적 형변환은 숫자 또는 문자열이 예측 가능한 경우에 이루어집니다.
변환 순서는 다음과 같습니다.

- 객체 프로퍼티를 함수 실행 합니다. 결과가 숫자 원시값인 경우 그 값을 반환합니다. 
- 그렇지 않은 경우는, toString ()를 호출 합니다. 결과가 원시값인 경우에 그 값이 반환됩니다. 
- 그렇지 않으면 TypeError를 발생 시킵니다.

~~~
3 * {valueOf: function(){return 3}}
// 9

toString.call(+{a: 1})
// [object Number]

function returnObject() { return {} }
3 * { valueOf: returnObject, toString: returnObject }
// TypeError: Cannot convert object to primitive value

''+[]
// ""
0+[]
// "0"

{}+{}
// "[object Object][object Object]"

[]+{}
// "[object Object]"
{}+[]
// 0

{}+[]+[]
// "0"

~~~

~~~
ToPrimitive(input, PreferredType?)
// PreferredType는 number or string이다. 

function ToPrimitive(input, preferredType){
  
  switch (preferredType){
    case Number:
      return toNumber(input);
      break;
    case String:
      return toString(input);
      break
    default:
      return toNumber(input);  
  }
  
  function isPrimitive(value){
    return value !== Object(value);
  }

  function toString(){
    if (isPrimitive(input.toString())) return input.toString();
    if (isPrimitive(input.valueOf())) return input.valueOf();
    throw new TypeError();
  }

  function toNumber(){
    if (isPrimitive(input.valueOf())) return input.valueOf();
    if (isPrimitive(input.toString())) return input.toString();
    throw new TypeError();
  }
}
~~~

- {}+[]
{}는 statement로 인식되며 empty block으로 인터프린트 된다. 따라서 +[]의 결과와 같다. 배열은 숫자 타입으로 변환 시 0이 출력되기 때문에 '0'이 출력된다. 

- []+{}
{}는 빈 오브젝트로 인터프린트된다. []는 연산자의 앞에 있기때문에 자바스크립트 엔진에 의해 ""으로 반환되어 결과가 출력된다. 
[]의 변환 결과인 ""와 {}의 스트링 변환값 "[object Object]"이 더해진 "[object Object]"가 출력된다.


## 2. Explicit conversion of values(값의 명시적 변환)

형 변환에 대한 사이드 이펙트를 줄이기 위해서 명시적 형변환이 필요합니다. 

~~~
var a = 10;
var b = String( a );

var c = "10";
var d = Number( c );

b; // "10"
d; // 10
~~~

## 3. Structural vs. Nominal typing explained

타입 시스템의 중요한 두 가지 요소는 Structural typing과 Nominal typing입니다.
두 가지의 주어진 타입이 호환되는지 또는 타입이 다른 타입의 하위 타입인지 결정하는 것입니다.
자바스크립트는 두 가지

## 3-1 Nominal Typing

~~~
class Person {
    public name: string;
}

class Employee extends Person {
    public salary: number;
}

class Manager extends Employee { }

class Product {
    public name: string;
    public price: number;
}
~~~

예를 들면, 클래스 선언에 "extends"가 쓰였기 때문에 Employee 클래스는 Person 클래스의 서브타입입니다.
또한 Product는 "extends"가 없기때문에 Person의 서브타입이 아닙니다.
즉, 상속이 가능한 타입을 말합니다.
상속에 의해 Manager는 Employee의 하위 타입일 뿐 아니라 Person의 하위 타입입니다. 
Structural typing은 OOP의 다양한 언어들(C++, C#, Java, Objective-C...)이 지원하는 타입입니다.

## 3-2 Structural Typing

두가지 타입의 구조가 동일할 경우에 같은 것으로 간주됩니다.
두가지 타입 T1, T2가 있을때, T1이 T2에있는 모든 public 멤버를 가진 경우에만 T1이 유형 T2의 sub type으로 간주됩니다. 
예제에서 Product는 Person의 sub type이 아니라고했습니다. 
그러나 Structural 시스템에서 Product는 Person의 public field를 모두 갖고 있기 때문에 
실제로 Person의 구조적 sub type으로 간주됩니다. 
실제로 그 반대는 사실이 아닙니다.(Product의 price field가 없기 때문에 Person은 Product의 subtype이 아닙니다.)


## 3-3 Comparison

Nominal type system은 일반적으로 타입 안전성과 유지 보수성을 향상시키는 장점이 있습니다.
structual typing은 대체적으로 유연하다고 알려져 있습니다.
Nominal type system은 서브타입 관계를 명시적으로 선언하는 추가관계로 확장된 구조적 타이핑입니다 (extends 절처럼)


~~~
export public interface Identifiable {
    public get name(): string

    static check(identifiable: Identifiable): boolean {
        return identifiable.name != 'anonymous';
    }
}
~~~
~~~
class A {

    public get name(): string { return 'John'; }
}
~~~
~~~
import { Identifiable } from 'Identifiable';

class A implements Identifiable {
    @Override
    public get name(): string { return 'John'; }
}

Identifiable.check(new A());
~~~

다음과 같이 선언할 수 있습니다. 
~~~
Identifiable.check(new A());
~~~

정상적으로 코드가 실행될 것입니다. 

~~~
export public interface Identifiable {
    public get id(): string
  // …
}
~~~

위와 같이 구조를 바꾸게 된다면 nominal typing에서 오류("Class A must implement getter id." and "The getter name must implement a getter from an interface.")가 발생합니다.
하지만 structural typing에서는 오류가 발생하지 않습니다. 

structural typing은 유연하다는 장점이 있고 nominal typing이 유지보수 측면의 잠점이 있습니다.


## 3-4 Combination of Nominal and Structural Typing in javascript

자바 스크립트 언어는 객체 지향 아이디어와 functional 아이디어가 혼합되어 있습니다. 
JavaScript에서는 두가지 타입의 사용이 혼합되는 경향이 있습니다. 
클래스 (또는 생성자 함수)가 객체 지향적이며 함수가 기능적 측면에서 더 많이 사용되는 경향이 있으므로 동시에 두 가지를 모두 사용합니다.

## 4 Duck Typing

"만약 그것이 오리처럼 걷고, 오리처럼 꽥꽥거린다면, 그것은 오리일것이다."
-> 맥락 없이는 이 문장은 많은 의미를 전달해주지 못합니다.

덕 타이핑에서는, 객체의 타입보다 객체가 사용되는 양상이 더 중요하다.
예를 들면, 덕 타이핑이 없는 프로그래밍 언어로는 오리 타입의 객체를 인자로 받아 객체의 걷기 메소드와 꽥꽥거리기 메소드를 차례로 호출하는 함수를 만들 수 있다. 
반면에, 같은 함수를 덕 타이핑이 지원되는 언어에서는 인자로 받는 객체의 타입을 검사하지 않도록 만들 수 있다. 걷기 메소드나 꽥꽥거리기 메소드를 호출 할 시점에서 객체에 두 메소드가 없다면 런타임 에러가 발생하고, 두 메소드가 제대로 구현되어 있다면 함수는 정상적으로 작동한다. 여기에는 인자로 받은 객체가 걷기 메소드와 꽥꽥거리기 메소드를 갖고 있다면 객체를 오리 타입으로 간주하겠다는 암시가 깔려있다. 바로 이 점이 앞에서 인용한 덕 테스트의 사상과 일치하기 때문에 덕 타이핑이라는 이름이 붙었다.

"Duck Typing은 한 객체(Object)가 특정한 목적에 적합한지 판별하는 데 관한 것이다. 
일반적인 타이핑의 경우, 객체의 적합성은 오로지 객체의 타입의 따라 정해진다. 
Duck Typing에서는 실제 객체의 타입보다는, 객체의 (알맞은 의미를 가진) 메서드와 속성들의 존재 여부로 적합성을 판단한다."
-- 위키피디아 --
