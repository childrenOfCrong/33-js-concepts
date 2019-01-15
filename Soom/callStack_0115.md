# **Call Stack**

A **call stack** is a mechanism for an interpreter (like the JavaScript interpreter in a web browser) to keep track of its place in a script that calls multiple function— what function is currently being run and what functions are called from within that function, etc.

- When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the function.
- Any functions that are called by that function are added to the call stack further up, and run where their calls are reached.
- When the current function is finished, the interpreter takes it off the stack and resumes execution where it left off in the last code listing.
- If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.











### <코드로 보기>

- [call stack 작동원리 살펴보기 - loupe](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gbXVsdGlwbHkoYSxiKXsKICAgIHJldHVybiBhKmI7Cn0KCmZ1bmN0aW9uIHNxdWFyZShuKXsKICAgIHJldHVybiBtdWx0aXBseShuLG4pOwp9CgpmdW5jdGlvbiBwcmludFNxdWFyZShuKXsKICAgIHZhciBzcXVhcmVkID0gc3F1YXJlKG4pOwogICAgY29uc29sZS5sb2coc3F1YXJlZCk7Cn0KCnByaW50U3F1YXJlKDQpOw%3D%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

- [stack overflow - loupe](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gZm9vKCl7CiAgICByZXR1cm4gZm9vKCk7Cn0KCmZvbygpOw%3D%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)









![javascript callstackì ëí ì´ë¯¸ì§ ê²ìê²°ê³¼](https://image.slidesharecdn.com/sonlejs-event-loop-160805060652/95/javascript-event-loop-11-638.jpg?cb=1470377352)













## **JavsScript** 

##  : a <u>single-threaded</u> <u>non-blocking</u> <u>asynchronous</u> <u>concurrent</u> language

1. single-threaded 

2. non-blocking

3. asynchronous

4. concurrent





## 1. single-threaded

- one thread == one call stack == one thing at a time == Run-to-Completion

- process : 현재 실행중인 프로그램 (RAM에 올라와있다.) / 프로그램은 하나지만 프로세스는 여러개를 띄울 수 있다 (메모장 여러개 띄우기) 그리고 `각각 가상 주소공간`을 할당받는다. 

- thread : 프로세스에 시간이라는 개념을 추가, 프로세스에서의 실행 흐름

  ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Multithreaded_process.svg/220px-Multithreaded_process.svg.png)

- single thread : 시간이 흐르면서 실행되는 코드의 줄기가 하나 (코드를 위에서부터 한줄한줄 해석)

- multi thread : 동시에 여러개의 코드를 실행

  - 두 개의 쓰레드로 작업한 시간이 싱글쓰레드로 작업한 시간보다 더 걸릴수도 있음 - context switching 시간 때문

    (context switching = 프로세스 or 쓰레드 간의 전환)

  - Deadlock 상황에 빠질수도 있음(운영체제 혹은 소프트웨어의 잘못된 자원 관리로 인하여 둘 이상의 프로세스(심하면  운영체제 자체도 포함해서)가 함께 멈추어 버리는 현상)

    ​	1) 휴대폰이 정지되어 인터넷으로 복구하려니 범용 공인인증서를 요구한다. 
    ​	2) 범용 공인인증서를 발급받으려 하니, 휴대번호 인증을 요구한다. 

    ![img](https://s3.namuwikiusercontent.com/s/19ddbf5daa82d7d7a67051de77d4f6af646a00f916f7e9711d6c6c2749fc1f9c01238bdc1bd94d611af7e8b089b9865319b820bb569a8341262745cce1967b81e4418677ebf9ef558a80498af5c58b329acd69fc4c362e84142322c380d964f3)





## 2. Non-blocking

**Non-blocking I/O(Asynchronous I/O 혹은 Non-sequential I/O)**

- Doing other things without stopping your programming from running.

- 입출력 처리는 시작만 해둔 채 완료되지 않은 상태에서 다른 처리 작업을 계속 진행할 수 있도록 멈추지 않고 입출력 처리를 기다리는 방법

- I/O 처리를 하는 전통적인 방법은 I/O 작업을 시작하면 완료될 때까지 기다리는 방법이다. 기존에는 synchronous I/O 혹은 blocking I/O를 통해 I/O 작업을 진행하는 동안 프로그램의 진행을 멈추고(block) 기다리는 방식이 사용됨 

  --> 많은 I/O 작업이 있는 경우 I/O 작업이 진행되는 동안 프로그램이 아무일도 하지 않고 시간을 소비 

- 반면, Non-blocking I/O 방식을 사용하면 외부에 I/O 작업을 하도록 요청한 후 즉시 다음 작업을 처리함으로써 시스템 자원을 더 효율적으로 사용할 수 있음
- 그러나 I/O 작업이 완료된 이후에 처리해야하는 후속 작업이 있다면, I/O 작업이 완료될 때까지 기다려야 하므로 이 후속 작업이 프로세스를 멈추지 않도록 만들기 위해, I/O 작업이 완료된 이후 후속 작업을 이어서 진행할 수 있도록 별도의 약속(Polling, Callback function 등)을 함









## 3. Asynchronous 

- 프로그램의 주 실행흐름을 멈추어서 기다리는 부분없이 즉시 다음 작업을 수행할 수 있도록 만드는 프로그래밍 방식
- 코드의 실행 결과 처리 및 활용을 별도의 채널에 맡겨둔 뒤 결과를 기다리지 않고 바로 다음 코드를 실행하는 방식으로 프로그램을 진행









## 4. Concurrent 

- Concurrency란 각 프로그램 조각들이 실행 순서와 무관하게 동작할 수 있도록 만들어 한 번에 여러 개의 작업을 처리할 수 있도록 만든 구조. 하나의 작업자가 여러 개의 작업을 번갈아가며 수행할 수 있게함
- 동시성을 확보하게 되면, 작업 순서와 상관없이 각 작업이 완료되지 않았더라도 필요에 따라 번갈아 가며 작업을 수행함으로써 전체 작업 수행 속도를 향상시킬 수 있음











### <코드로 보기>

- [비동기 함수 실행 - loupe](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gdGVzdDEoKXsKICAgIGNvbnNvbGUubG9nKCJ0ZXN0MSIpOwogICAgdGVzdDIoKTsKfQoKZnVuY3Rpb24gdGVzdDIoKXsKICAgIGxldCB0aW1lciA9IHNldFRpbWVvdXQoZnVuY3Rpb24oKXsKICAgICAgICBjb25zb2xlLmxvZygidGVzdDIiKTsKICAgIH0sIDApOwogICAgdGVzdDMoKTsKfQoKZnVuY3Rpb24gdGVzdDMoKXsKICAgIGNvbnNvbGUubG9nKCJ0ZXN0MyIpOwp9Cgp0ZXN0MSgpOw%3D%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

  











![img](http://prashantb.me/content/images/2017/01/js_runtime.png)



#	Conclusion

#### one thread == one call stack == one thing at a time == Run-to-Completion

-> JavaScript 엔진이 단일 호출 스택을 사용한다. 



#### But, 실제 구동되는 환경(브라우져, Node.js 등)에서는 주로 여러 개의 스레드가 사용됨

- Node.js는 비동기 IO를 지원하기 위해 libuv 라이브러리를 사용하며, 이 libuv가 이벤트 루프를 제공함. JS 엔진은 비동기 작업을 위해 Node.js의 API를 호출하며 이때 넘겨진 콜백은 libuv의 이벤트 루프를 통해 실행됨 
  * libuv is a multi-platform support library with a focus on asynchronous I/O.













### Reference

1. 자바스크립트와 이벤트 루프 https://github.com/nhnent/fe.javascript/wiki/June-13-June-17,-2016
2. [JS] JavaScript의 Event Loop http://asfirstalways.tistory.com/362
3. Philip Roberts: What the heck is the event loop anyway?  https://www.youtube.com/watch?v=8aGhZQkoFbQ
4. 멈추지 않고 기다리기(Non-blocking)와 비동기(Asynchronous) 그리고 동시성(Concurrency) https://tech.peoplefund.co.kr/2017/08/02/non-blocking-asynchronous-concurrency.html#fn:5
5. 멀티스레드 https://wayhome25.github.io/cs/2017/04/14/cs-15-2/
6. 싱글쓰레드와 멀티쓰레드 http://server-engineer.tistory.com/273
7. MDN
8. 위키피디아
9. 나무위키

1. 싱글쓰레드와 멀티쓰레드 http://server-engineer.tistory.com/273

