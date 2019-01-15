# Promise

## 콜백 vs promise

~~~
console.log("start script");

setTimeout(() => {
  console.log('go');
}, 0);

Promise.resolve("promise").then((res, rej) => console.log(res));
~~~

// 각 라인의 순서
// jop queue
// micro microtask


## promise all(실패 우선 연산)

~~~
let promiseArr = [Promise.resolve("Go"), 12]
Promise.all(promiseArr).then(function(values) {
  console.log(values);
});

Promise.all([...promiseArr, Promise.reject('ERROR')]).then(function(values) {
  console.log(values);
});
~~~

## promise race

~~~
let promiseArr = [Promise.resolve("Go"), 12];
Promise.race(promiseArr).then(function(values) {
  console.log(values);
});

Promise.race([...promiseArr, Promise.reject('ERROR')]).then(function(values) {
  console.log(values);
});

Promise.race([Promise.reject('ERROR'), ...promiseArr]).then(function(values) {
  console.log(values);
});
~~~
