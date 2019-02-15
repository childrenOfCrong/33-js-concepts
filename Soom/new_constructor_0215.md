## **new && constructor && instance** ##

- constructor에는 2가지 종류가 있다.  
    - native(built-in) constructor (총 9개 : Object(), Array(), String(), Number(), Boolean(), Date(), Function(), Error() and RegExp())

    - custom constructor (속성과 매서드 직접 정의)

- constructor는 왜 유용한가?
    - 같은 속성과 매서드를 가진 객체를 여러개 찍어낼 때  

- constructor convention
    - regular function과 구분하기 위해 첫 알파벳은 대문자  
    - `new` operator 사용  
    
    ```js
    function Book() { ... }  
    var myBook = new Book();
    ```

    - Book의 instance를 만들어서 (new Book()) 변수 `myBook`에 할당했다!  

- instance의 타입체크하기  
    1) `instanceof` 사용  
    
    ```js
    myBook instanceof Book    // true
    myBook instanceof String  // false
    ```

    - `instanceof` 다음에는 무조건 함수가 와야함

    ```js
    myBook instanceof {};
    // TypeError: invalid 'instanceof' operand ({})
    ```

    2) constructor property 사용
    - 모든 객체 instance는 그것을 생성한 constructor를 가리키는 constructor 속성을 가지고 있음

    ```js
    myBook.constructor == Book;   // true
    ```

    - 즉 모든 객체는 `prototype`의 constructor를 상속받는 것을 알 수 있음 

    ```js
    var s = new String("text");
    s.constructor === String;      // true
    "text".constructor === String; // true

    var o = new Object();
    o.constructor === Object;      // true

    var o = {};
    o.constructor === Object;      // true

    var a = new Array();
    a.constructor === Array;       // true

    [].constructor === Array;      // true
    ```

    - 하지만 overwrite 방지를 위해 `instanceof`를 쓰는 것이 좋습니다. 

- Don't forget the `new` keyword !
    - `new` 빼먹으면 새롭게 생성된 객체가 아닌 전역 객체를 사용하게 됨

    ```js
    function Book(name, year) {
    console.log(this);
    this.name = name;
    this.year = year;
    }

    var myBook = Book("js book", 2014);    // Window
    console.log(myBook instanceof Book);   // false
    console.log(window.name, window.year); // js book, 2014

    var myBook = new Book("js book", 2014); // Book{}
    console.log(myBook instanceof Book);  // true
    console.log(myBook.name, myBook.year); // js book, 2014
    ```

    Q. 이것을 방지하려면?  
    ~~tcirts esu~~

- Scope-Safe Constructor 
    - 대부분의 built-in constructor는 scope-safe
    - `new`가 쓰였는지 안 쓰였는지 파악하는 패턴 사용

    ```js
    function Book(name) { 
        if (!(this instanceof Book)) { 
        // the constructor was called without "new".
        return new Book(name);
        } 
    }
    ```

- `new`의 4가지 규칙 (Four Rules)
    1) It creates a new, empty object.
    2) It binds `this` to our newly created object.
    3) It adds a property onto our newly created object called `__proto__` which points to the constructor function’s prototype object.
    4) It adds a `return this` to the end of the function, so that the object that is created is returned from the function.  

    ```js
    function Book(name, year) {
        this.name = name;
        this.year = year;
    }  

    var myBook = new Book("js book", 2014)
    ```


    ```js
        // 1) 새로운 빈 객체 `myBook` 생성 
        var myBook = {};

        // 2) `myBook`에 `this` 바인드. 여기서 `this`는 `myBook`을 가리킴. 

        // 3) `__proto__` 가 추가됨. `myBook.__proto__`는 이제 `Book.prototype`을 가리킴
        myBook.__proto__ = Book.prototype;

        // 4) `myBook` 객체가 `myBook` 변수로 리턴됨  
        // `new`가 Function Constructor를 실행 
        Book.call(myBook);

        // Book Function Constructor는 this가 myBook으로 치환되어 일반 함수처럼 실행됨
        function Book(name, year){
            myBook.name = name;
            myBook.year = year;
        }

        myBook.name = "js book"
        myBook.year = 2014
    ```