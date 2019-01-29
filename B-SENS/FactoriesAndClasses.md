객체를 생성하는 기본적인 방법은 객체 리터럴 입니다. 

    let dog = {
    	age: 3,
    	color: white,
    	move: function move(){
    		console.log('move');
    	}
    }
    
    let cat = {
    	age: 2,
    	color: black,
    	move: function move(){
    		console.log('move');
    	}
    }

하지만 이렇게 객체를 생성하는 방법엔 메모리 효율성에 대한 문제와 유지 보수의 문제가 있는데요. 

위의 코드를 예시로 들자면, move 메서드의 중복이 있습니다. 중복된 메서드를 제거하여 조금 더 효율적인 코드를 작성할 수 있을것 같습니다. 

또한 move 메서드의 길이는 매우 짧기 때문에 어떤 수정이 있더라도 문제가 없을 것 같지만 코드가 길어지면 문제가 생길것 같습니다.

이런 문제를 해결하기 위해서 우리는 factoring function& constructor& class를 사용합니다. 

## 1. factory function

객체를 반환하여 간단하게 객체를 생성할 수 있도록 하는 패턴입니다. 

    // factory function
    function createAnimal(age, color){
    	return{
    		age,
    		color,
    		move(){
    			console.log('move')
    		}
    	}
    }
    const animal = createAnimal(2, 'white');

객체를 반환한다는 아이디어는 매우 간단해 보입니다. 

하지만 여기서 가장 중요한 키워드는 '클로저'입니다.

class나 constructor로 인한 객체를 생성하고 내부 프로퍼티에 접근할 수 있는 가장 중요한 부분이 'this'인데요.

closure에 의해 함수 내부 변수로 접근할 때는 this를 고려하지 않아도 된다. 또한 private한 변수로 외부 접근이 불가능하기에 incapsulation 관점에서도 좋은 패턴이다. 

## 2. class

    class createAnimal{
    	constructor(age, color){
    		this.age= age;
    		this.color= color;
    	}
    	move(){
    		console.log('move');
    	}
    }
    const animal = new createAnimal(3, 'black');

여러분이 너무 잘 알고 계시죠..

## 3. factory function VS class

    class Cat{
        constructor(){
            this.sound = 'Meow';
        }
        say(){
            console.log(this.sound);
        }
    }
    const cat = new Cat();
    
    document.querySelector('#lga').addEventListener('click', cat.talk);
    // => this는 Dom element
    document.querySelector('#lga').addEventListener('click', cat.talk.bind(test));
    // => bind 이용해서 해결 가능
    
    function createCat(){
    	const sound = "Meow";
    	return {
    		say(){
    			console.log(sound);
    		}
    	}
    }
    const cat1 = createCat();
    document.querySelector('#lga').addEventListener('click', cat1.say);

객체 생성 방식에 대한 내부 메서드로 접근하는 방식을 비교 했는데요.

DOM Selector에 Event 작동 방식의 차이가 있습니다. 

factory function을 이용한 객체 생성시 강점으로 둔 'this'의 동작 방식의 차이 입니다.

addEventListener() 함수의 'this'는 selector로 인해 정의된 타겟으로 설정됩니다. 

## 4. 성능 비교

Factory: 0.0004ms

Class: 0.0002ms

두 가지 방식의 성능은 0.0002ms의 차이가 있습니다. 대규모 서비스이 아니라면 큰 차이가 나지 않기 때문에 장점에 따라서 적절히 사용하면 될 것 같습니다.
