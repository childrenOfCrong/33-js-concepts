# 190128-

------

1. ### setInterval, setTimeout, and requestAnimationFrame

   setInterval은 무엇이고 setTimeout은 무엇일까?

   1. setInterval은 정해진 시간마다 콜백함수를 계속해서 실행하는 것.

   2. setTimeout은 정해진 시간뒤에 콜백함수가 실행되는 것.

   3. `clearInterval, clearTimeout`으로 제거할 수 있다.

   4. 재귀적 setTimeout으로 setInterval의 효과를 좀더 유연하게 낼 수 있다.

      ```javascript
      let timerId = setTimeout(function tick() {
        alert('tick');
        timerId = setTimeout(tick, 2000); // (*)
      }, 2000);
      ```

   5. 이렇게 하면 아래의 그림과 같이 된다.
      ![img](https://javascript.info/article/settimeout-setinterval/settimeout-interval.png)

      하지만 setInterval을 쓰면 아래와 같이 된다.![img](https://javascript.info/article/settimeout-setinterval/setinterval-interval.png)
      setInterval의 경우 함수는 clearInterval이 호출 될 때까지 메모리에 남아 있다. 이게 나쁜게, interval함수가 모두 실행되기 이전에 interval함수가 또 실행되게 만들어서 callStack이 완전 지옥이 될 수도 있다.
      이건 부작용이다. 함수는 외부 환경을 참조하므로, 살아있는 동안 외부 변수도 살아 간다. 이렇게 되면 함수 자체보다 훨씬 많은 메모리가 필요할 수 있다고 한다. 따라서 예정된 기능이 더 이상 필요하지 않을 때, 매우 작더라도 setInterval함수는 취소하는 것이 좋다.

      여기서 우리가 엄청 자주 보았던 문제를 하나 보자.

      ```javascript
      console.log('hi');
      setTimeOut(() => {console.log('hello')}, 0);
      console.log('안녕');
      ```

      왜 이렇게 되는지 수도없이 들으셨겠지만 칠판을 통해 설명해보자면.....(대신 설명해주실 분)

      그럼 위와같은 이유로

      ```javascript
      let i = 0;
      
      setTimeout(() => alert(i), 100); // 100000000
      
      // assume that the time to execute this function is >100ms
      for(let j = 0; j < 100000000; j++) {
        i++;
      }
      ```

      이 코드의 값이 왜 100000000 가 alert 되는지 설명해보자.(엄밀히 같은 이유는 아니지만)

   6. 다음으로 requestAnimationFrame을 살펴보자.

      이게 setInterval보다 좋은데 좋은 이유가 3가지 있다.

      1. 브라우저가 애니메이션을 최적화 할 수 있으므로 애니메이션이 부드럽게 처리가된다.
      2. 비활성 탭의 애니메이션이 중지되어 cpu에 더 좋음
      3. 더 배터리친화적임

      ```javascript
      function repeatOften() {
        // Do whatever
        requestAnimationFrame(repeatOften);
      }
      requestAnimationFrame(repeatOften);
      ```

      뭐요론 식으로 쓰면 된단다. 한번 실행하면 재귀적으로 자기자신을 계속 실행한다.
      이것은 setTimeout처럼 ID를 반환하는데, 이걸 통해서 애니메이션 반복또한 중지시킬수 있다. `cancelAnimationFrame` 을 사용해서 말이다.

      https://codepen.io/htmlhead/pen/PVzZRd 이런식으로 사용할 수 있다.

      