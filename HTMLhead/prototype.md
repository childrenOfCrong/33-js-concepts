# 190210-

------

### 프로토타입

첫번째로 프로토타입이 어디에있는지부터 알아봅시다.

우선 이전에 class가 없을 때 함수로 클래스를 만드는 방법을 만들어봅시다.

![img](https://cdn-images-1.medium.com/max/800/1*y4PkgnYJmLPur2MVzIJZ4A.jpeg)

모두가 아시다시피 new키워드로 만들 수 있습니다. 그런데 만약에 new를 안붙이고 만들면 어떻게 될까요? 그렇게 만들면 브라우저마다 다르긴 하지만 window.name 이 hector로 만들고 할일을 마칩니다. 'use strict'모드에서는 에러가 나겠지요. 
그럼 여기서 Cat.prototype은 어디에 존재할까요?

![img](https://cdn-images-1.medium.com/max/800/1*PYBw1HG7zT_vQAF6-JohxQ.jpeg)

위의 이미지를 보시면 아시겠지만, cat은 이름이 있는 객체가 됩니다. 이것은 `__proto__` 라는 것이 존재하고, 그것은 mewo함수와 Cat생성자 함수의 constructor, 그리고 프로토타입 객체의 프로토타입을 가지고 있습니다. 여기서 우리가 왜 prototype chain이라고 부르는지 알 수 있습니다. 그럼 여기서 이걸 한번 해볼까요? 

![image-20190210130648300](/Users/naduseong/Library/Application Support/typora-user-images/image-20190210130648300.png)

네. 사라졌습니다. 이렇게 하면 만들어놓은 모든 고양이들의 meow함수 능력이 사라지는 겁니다. 왜냐하면, 그 능력을 하나로 모두 공유하고 있었거든요.

함수로 클래스를 만드는 방법을 알아보았으니, 이제는 class를 써볼까요?

![img](https://cdn-images-1.medium.com/max/800/1*9ropehPTGw99dAUNXYFnUQ.jpeg)

이렇게 만들면 객체지향프로그래밍을 할 때 사용할 수 있는 모든 방법들을 사용하게 됩니다. class, super, constructor, static... 이 supercat은 어떻게 생겼을까요?

![img](https://cdn-images-1.medium.com/max/800/1*ADWWzjPuNvoHQlmpMlnIrQ.jpeg)

우리가 extends 를 사용한다는 사실은 prototype chaie 에서 깊이를 추가했다는 것을 의미합니다. 위처럼 생긴것 말이죠. 

자 우리는 프로토타입 체인이 어떤건지 알았습니다. 위에서 말했듯 말이죠. extends를 하면 체인이 하나 더 추가된다는 것을 알았고 class를 사용하면 좀 더 간단하게 class를 만들 수 있다는 것을 알게되었습니다. 그렇다면 prototype chain을 사용한 inheritanc 에 대해서 알아볼까요?

자바스크립트 객체는 속성을 저장하는 동적인 "가방"과 (이것을 **자기만의 속성**이라고 부른다고 합니다.) 프로토타입 객체에 대한 링크를 가집니다. 객체의 어떤 속성에 접근하려할 때 그 객체 자체 속성 뿐만 아니라 객체의 프로토타입, 그 프로토타입의 프로토타입 등 프로토타입 체인의 끝부분에 이를 때까지 그 속성을 탐색합니다. 간단한 예를 들어봅시다.

```javascript
let f = function () {
    this.a = 1;
    this.b = 2;
}
let o = new f(); // {a: 1, b: 2}

//속성값 추가
f.prototype.b = 3;
f.prototype.c = 4;

console.log(o.a); // 1
console.log(o.b); // 2
console.log(o.c); // 4
console.log(o.d); // undefined
```

o.a는 o객체의 속성으로 a가 존재하니까 바로 1.
o.b는 o객체의 속성으로 b가 존재하니까 바로 2, 프로토타입 내부에 3도 존재하시만 앞에서 먼저 찾았으니 그쪽까지 가지 않습니다.
o.c는 o객체의 속성으로 c가 존재하지 않으니까 프로토타입 내부를 찾고 프로토타입 내부에 존재해서 4를 출력합니다.
o.d는 o객체의 속성으로 d가 존재하지 않으니까 프로토타입 내부를 찾고, 거기에도 없으니 그다음 프로토타입 내부를 찾으려 했는데 프로토타입이 비어있기 떄문에 undefined를 출력해줍니다.

속성 상속을 알아보았으니 메서드상속에 대해서 알아봅시다.

자바스크립트는 객체의 속성으로 함수를 지정할 수 있고 속성 값을 사용하듯 쓸 수 있습니다. 속성 값으로 지정한 함수의 상속 역시 위에서 말한 속성의 상속과 같습니다. (단 위에서 언급한 "속성의 가려짐" 대신 "*메소드 오버라이딩, method overriding*" 라는 용어를 사용한다고 합니다.)

상속된 함수가 실행 될 때,  `this`라는 변수는 상속된 오브젝트를 가르킵니다. 그 함수가 프로토타입의 속성으로 지정되었다고 해도 말이입니다

