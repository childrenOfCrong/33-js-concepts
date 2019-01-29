# this, call, apply, and bind

## - this

​	함수가 생성될때, <code>this</code> 가 생성되는데 이 <code>this</code> 는 함수를 가지고 있는  객체를 참조하고 있습니다. 그래서 <code>this</code> 는 함수 그자체와는 관련이 없고, 함수가 어떻게 불려지는 지에 따라 달라집니다.

### 1. 함수 실행

```javascript
var myFunction = function () {
	console.log(this);
};

myFunction(); // window
```

 초기값으로 <code>this</code> 는 <code>window</code> 를 가르킵니다. 이는  당연한 것이겠죠? 아까 얘기했듯이 <code>myFunction</code> 이 생성될 때, 이 함수는 전역스코프에 들어가게되고, <code>window</code> 객체가 이 함수를 가지고 있으니까요. 단, 스크립트가 strict 모드(<code>"use strict"</code> ) 에서 실행된다면 <code>this</code> 는 <code>undefined</code> 가 됩니다.

### 2. 객체 메소드

```javascript
var myMethod = function () {
  console.log(this);
};

var myObject = {
  myMethod: myMethod
};

myMethod(); 
myObject.myMethod();
```

이건 어떻게 될까요? 위에서 말했듯이, <code>this</code> 는  함수 자신과는 관계과 없고, 어떻게 함수가 불려지냐에 따라 결정됩니다. 위 <code>myMethod</code> 는 전역스코프에서 불렸으니 <code>this</code> 는 <code>window</code> 가 됩니다. <code>myObject.myMethod</code> 는 객체에 의해 불러지니 <code>this</code> 는 <code>myObject</code>가 됩니다. 이를 암시적 바인딩(implicit binding)이라고 합니다.

### 3. 명시적 바인딩(explicit binding)

명시적 바인딩은 말그대로 우리가 직접 <code>this</code> 를 바인딩해줍니다. 함수 객체의 메소드인 <code>call</code>, <code>apply</code> 를 사용하면 됩니다. 

```javascript
var myMethod = function () {
  console.log(this);
};

var myObject = {
  myMethod: myMethod
};

myMethod() // this === window
myMethod.call(myObject, args1, args2, ...) // this === myObject
myMethod.apply(myObject, [array of args]) // this === myObject
```

<code>call, apply</code> 의 첫번째 인자는 명시적으로 바인딩하고 싶은 <code>this</code> 입니다. 다음 인자는 호출한 함수의 인자입니다. <code>call</code> 과 <code>apply</code> 의 차이점은 개별인자로 넘겨주냐 배열형태로 넘겨주냐의 차이입니다. 또한 명시적 바인딩이 암시적 바인딩보다 우선순위를 가집니다. 

```javascript
var myMethod = function () 
  console.log(this);
};

var obj1 = {
  myMethod: myMethod
};

var obj2 = {
  myMethod: myMethod
};

obj1.myMethod(); // obj1
obj2.myMethod(); // obj2

obj1.myMethod.call( obj2 ); // obj2
obj2.myMethod.call( obj1 ); // obj1
```

<code>obj1.myMethod</code> 에서 <code>this</code> 는 obj1로 바인딩되어있지만, <code>call</code> 에 의해 obj2로 명시적으로 바인딩됩니다. 

<code>bind</code> 는 <code>call, apply</code> 와 비슷하지만 조금 다릅니다. 우리가 지정한 <code>this</code> 를 바인딩한 **함수** 를 반환합니다. 하지만 한가지 주의할 게 있습니다. <code>bind</code> 바인딩은 명시적 바인딩보다 우선순위를 가집니다.

```javascript
var myMethod = function () {
  console.log(this.a);
};

var obj1 = {
  a: 2
};

var obj2 = {
  a: 3
};

myMethod = myMethod.bind(obj1);
myMethod.call( obj2 ); // 2

```

<code>myMethod.call( obj2 )</code>  실행을 했지만, 위에서 <code>bind</code> 를 통해 바인딩했기 때문에 우선순위가 낮은 명시적 바인딩의 <code>this</code> 는 효력이 없습니다. 

### 4. new  바인딩

```javascript
function foo(something) {
  this.a = something;
}

var obj1 = {};

var bar = foo.bind( obj1 );
bar( 2 );
console.log( obj1.a ); // 2

var baz = new bar( 3 );
console.log( obj1.a ); // 2
console.log( baz.a ); // 3
```

<code>new</code> 키워드로 객체 인스턴스를 생성하면, <code>this</code> 는 인스턴스 그자체가 됩니다. 그러니 암시적 바인딩, 명시적 바인딩 등을 신경쓸 필요가 없죠.  



## - bind

<code>bind</code> 메소드는 우리가 설정된 <code>this</code> 를 가진 함수를 반환합니다. 나중에 <code>bound</code> 를 실행하면 이 함수는 <code>this</code> 는 우리가 설정한 값이 됩니다. 

```javascript
var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
};

var pokemonName = function() {
    console.log(this.getPokeName() + 'I choose you!');
};

var logPokemon = pokemonName.bind(pokemon); // creates new object and binds pokemon. 'this' of pokemon === pokemon now

logPokemon(); // 'Pika Chu I choose you!'
```

 위의 코드를 뜯어보면,

1. JS 엔진은 <code>pokemonName</code> 함수의 인스턴스를 생성합니다. 쉽게 말하면 똑같은 함수를 복사한 새로운 객체(함수)를 생성하는 거죠. 그리고는 <code>pokemon</code> 을 이 함수의 <code>this</code> 로 바인딩합니다. 
2. 이러한 설정이 끝나고 <code>logPokemon</code> 함수를 실행하면, 보기에는 글로벌 스코프에서 불러서 <code>this</code> 가 window인 거 같지만,  <code>bind</code> 메소드를 통해 <code>this</code> 를 바인딩했기때문에, 우리가 원하는 결과를 얻을 수 있습니다.



## - call / apply

​	<code>call</code> 과 <code>apply</code> 는 함수를 즉시 실행하기 위해 사용합니다. <code>bind</code> 메소드와 비슷하게 <code>this</code> 를 바인딩하지만 함수를 반환하지않고 즉시 실행하죠. 또한 <code>bind</code> 메소드는 함수를 복사한 함수를 생성한다고 했지만, <code>call, apply</code> 메소드는 그렇지않습니다. 

```javascript
var pokemon = {
    firstname: 'Pika',
    lastname: 'Chu ',
    getPokeName: function() {
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
    }
};

var pokemonName = function(snack, hobby) {
    console.log(this.getPokeName() + ' loves ' + snack + ' and ' + hobby);
};

pokemonName.call(pokemon,'sushi', 'algorithms'); // Pika Chu  loves sushi and algorithms
pokemonName.apply(pokemon,['sushi', 'algorithms']); // Pika Chu  loves sushi and algorithms

```



 프론트엔드를 공부하면서 this 바인딩과 관련하여 가장 많이 쓰이는 부분은 사실 이벤트핸들러에 있다고 생각합니다. 사용자 UI와 관련하여 인터랙션이 증가하면서 프론트개발자들은 사용자에게 좀 더 직관적인 이벤트를 제공해줘야하죠. 그렇기때문에 이벤트 핸들러 관련하여 this를 알아볼까 합니다. 

```javascript
const btn = document.querySelector("button");
const user = {
    name: "Torco",
    age: 29,
    getName(){
        alert(this.name);
    }
}

btn.addEventListener("click", user.getName);
// 해결책: btn.addEventListener("click", user.getName.bind(user));

```

예를 들어, 위와 같은 코드가 있다고 생각해봅시다. 아주 간단합니다. 버튼을 클릭하면 사용자의 이름이 뜨는 거죠. 버튼의 클릭이벤트로 <code>user.getName</code> 을 넘겨주었습니다.(이벤트 리스너는 이런식으로 분리해서 넣는 것이 좋습니다. 테스트가 용이하죠.) 
버튼을 클릭하면 어떻게 될까요? 아무것도 뜨지 않는 것을 확인할 수 있습니다. 이벤트를 DOM 요소에 추가시킬 때는 자동적으로 DOM 요소를 이벤트리스너의 <code>this</code> 로 바인딩 시켜버립니다. 그러니 리스너의 <code>this</code> 는 btn이 되는 거죠.
그런데 <code>user.getName</code> 를 넘겨줬으면 이 메소드의 <code>this</code> 는 <code>user</code> 이니까 리스너의 <code>this</code> 도 <code>user</code>  아닌가하고 생각할 수 있습니다. 저도 처음에는 그렇게 생각했으나, 조금만 생각해보면 쉽게 답을 구할 수 있습니다. <code>this</code> 를 결정함에 있어서 가장 중요한 건 위에서 계속 얘기했듯이 함수(메소드) 그 자체가 아니라, 어떻게 호출되냐에 따라 결정된다고 했습니다. <code>user.getName</code> 이 콜백함수로 전달되면서 콜백함수는 getName 의 주소를 참조하고 있습니다. 그럼 클릭을 하게 되면, 단순히 

```javascript
function (){
    alert(this.name);
}
```

함수가 실행되는거죠. 함수가 실행되면서 <code>this</code> 를 찾게되고 버튼이 됩니다. 버튼의 name을 찾게 되고 정의되어있지 않으니 아무것도 뜨지 않게 되는 거죠. 
예시는 너무 간단하지만, 이벤트를 다루다보면 많이 생각해야하는 부분이죠. 이벤트 자체가 비동기이다 보니 사실 리스너에 <code>this</code> 가 있으면 복잡해질 수도 있죠. 

```javascript
run() {
    this.searchEl.addEventListener("input", this.inputEvent.bind(this));
}

inputEvent() {
    if(!this.debouncer) this.debouncer = debounce(this.showKeywords.bind(this), 500);
    this.debouncer();
}
```

위는 제가 자동검색을 구현하면서 사용한 코드의 일부입니다. 자동적으로 <code>searchEl</code> 에 바인딩되는 <code>this</code> 를 바꾸기 위해 <code>bind</code> 메소드를 사용하여 제가 원하는 값으로 설정했습니다. 

```javascript
run() {
    this.searchEl.addEventListener("input", () => {
        if(!this.debouncer) this.debouncer = debounce(this.showKeywords.bind(this), 500);
   		this.debouncer();
    })
}
```

화살표 함수를 사용하면 <code>this</code> 바인딩 문제를 부분적으로 해결할 수는 있어요. 화살표함수의 <code>this</code> 바인딩은 현재 컨텍스트를 감싸고 있는 상위 컨텍스트의 <code>this</code> 가 정적으로 바인딩되는 거 다 아시죠? 그러니 화살표함수 내부의 <code>this</code> 는 제가 원하는 this가 될 수 있었습니다. 하지만, 테스트가 용이하지 않아 이벤트 리스너는 분리하는 것이 좋다고 하네요. 그리고 개인적인 생각이지만 위의 코드가 좀더 가독성이 좋네요.


