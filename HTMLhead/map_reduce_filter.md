# 왜 우리는 map, filter, reduce를 써야 하는가?

------

우선 무의식적으로 쓰기만 했던 map, filter, reduce를 사용하는 이유에 대해서 살펴봅시다.

* 첫번째, 인덱스 (즉, array [i])를 사용할 필요가 없습니다.
* 두번째, 원본 배열에서 전혀 상관이 없는 다른 값을 도출해내지 않습니다. 이는 즉 부작용이 최소화된다는 것을 뜻합니다.
* 세번째, 힘들게 for 루프를 관리 할 필요가 없습니다.
* 네번째, 힘들게 빈 배열을 만들고 그 안에 값을 넣을 필요가 없습니다.
* 다섯번째, 무엇보다도 겁나 간지나잖아요!

* map의 폴리필

```javascript
//map 폴리필
function map(arr, condition) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    newArr.push(condition(arr[i], i, arr));
  }
  return newArr;
}
```

condition은 우리가 만들어주는 함수라고 볼 수 있습니다. filter도 위와같습니다.

* filter의 폴리필

```javascript
//filter 폴리필
function filter(arr, condition) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    if (condition(arr[i], i, arr) === true) {
      newArr.push(arr[i]);
    }
  }
  return newArr;
}
```

우리는 필터를 사용할 때 리턴값을 true혹은 false로 반환하게 만들어야 합니다. 그에 유의하여 폴리필을 짜면 위와 같습니다.

* reduce의 폴리필

```javascript
//reduce폴리필
function reduce(arr, condition, firVal) {
  let accumulatedValue;
  if (firVal !== undefined) {
    arr.unshift(firVal);
  }
  for (let i = 0; i < arr.length; i++) {
    if (i === 0) {
      accumulatedValue = arr[0];
      i++;
    }
    accumulatedValue = condition(accumulatedValue, arr[i], i, arr);
  }
  return accumulatedValue;
}
```

reduce를 사용할 때에는, 4개의 인자를 사용하게 되는데, 첫번째는 이전값, 두번째는 축적값, 그리고 그다음은 현재 배열 내에서 이루어지고 있는 index값 그리고 마지막은 현재 우리가 사용하고 있는 배열의 값을 나타냅니다.

사용법은 무궁무진합니다. 객체를 사용해서 map과 filter의 사용법을 예로 들어보죠.

```javascript
const songArr = [
  { rank: 1, name: "YES OR YES", artist: "TWICE" },
  { rank: 2, name: "Cheer Up", artist: "TWICE" },
  { rank: 3, name: "그 노래", artist: "존박" },
  { rank: 4, name: "flaming", artist: "정성하" },
  { rank: 5, name: "twilight", artist: "정성하" },
  { rank: 6, name: "우아하게", artist: "TWICE" },
  { rank: 7, name: "Knock Knock", artist: "TWICE" },
  { rank: 8, name: "이게 아닌데", artist: "존박" },
  { rank: 9, name: "i'm your man", artist: "존박" },
  { rank: 10, name: "빗속에서", artist: "존박" },
  { rank: 11, name: "가을안부", artist: "먼데이키즈" },
  { rank: 12, name: "그 때 헤어지면 돼", artist: "로이킴" },
  { rank: 13, name: "피노키오", artist: "로이킴" },
  { rank: 14, name: "눈사람", artist: "정승환" },
  { rank: 15, name: "love love love", artist: "로이킴" }
];

const allSongName = songArr.map(song => song.name);
// console.log(allSongName);
const findStrSongSinger = songArr.filter(song => song.artist.includes("존"));
// console.log(findStrSongSinger); //이렇게 쓰면 연관 검색어를 찾을 때에도 사용할 수 있음.

```

그럼 map함수를 위의 폴리필에 대입하며 천천히 디버깅하듯 알아가볼까요?

```javascript
function map(arr, condition) {
    //1
  const newArr = [];
    //2
  for (let i = 0; i < arr.length; i++) {
    newArr.push(condition(arr[i], i, arr));
  }
  return newArr;
}
```

* 첫번째로 우리는 새로운 배열을 만듭니다. `newArr = []`
* 두번째로는 반복문을 돌기 시작하죠. 여기서 `condition`함수는 `(song) => song.name` 과 같습니다.
`i`가 `0`인 반복문을 한번 돌아볼까요?

`newArr.push(condition( { rank: 1, name: "YES OR YES", artist: "TWICE" },  0,  songArr))`

* 자 여기서 `condition` 함수는 `song => song.name` 라고 했으니, 함수에서 첫번째 인자인 
* `{ rank: 1, name: "YES OR YES", artist: "TWICE" }` 만을 사용하며 
* song이 `{ rank: 1, name: "YES OR YES", artist: "TWICE" }` 이되고, `song.name` 은 `"YES OR YES"` 가 됩니다.

이런 방식으로 반복문이 돌아서 결국엔 이런값이 나오게 되는 것입니다.



다음으로 filter함수를 위의 폴리필에 대입하며 천천히 디버깅하듯 알아가 볼까요?

```javascript
//filter 폴리필
function filter(arr, condition) {
  const newArr = [];
  for (let i = 0; i < arr.length; i++) {
    if (condition(arr[i], i, arr) === true) {
      newArr.push(arr[i]);
    }
  }
  return newArr;
}
```

map 과 똑같이 진행하고, 바로 첫번째 반복문을 들어가서

`if(condition({ rank: 1, name: "YES OR YES", artist: "TWICE" }, 0 , songArr))`

여기서 `condition` 함수는 `song => song.artist.includes("존")` 입니다.  
그러므로 `condition` 함수는 함수에서 첫번째 인자인 `{ rank: 1, name: "YES OR YES", artist: "TWICE" }` 만을 사용하며  
song이 `{ rank: 1, name: "YES OR YES", artist: "TWICE" }` 가되고, `song.artist.includes("존")` 의 값은 false가 되겠죠. 그러므로 `newArr` 에는 `push`가 되지 않구요. 그리하여 이러한 결과값이 나오게 되는 것입니다.

  

그럼 마지막으로 `reduce` 에 대해서 알아봅시다. 예제는 저번에 만들었던 파일을 사용하겠습니다.

```javascript
const arr = [
  {
    type: "arrayOpen",
    value: "[",
    child: []
  },
  {
    type: "arrayOpen",
    value: "[",
    child: []
  },
  {
    type: "number",
    value: "1",
    child: []
  },
  {
    type: "arrayOpen",
    value: "[",
    child: []
  },
  {
    type: "string",
    value: "'2'",
    child: []
  },
  {
    type: "arrayClose",
    value: "]",
    child: []
  },
  {
    type: "arrayClose",
    value: "]",
    child: []
  },
  {
    type: "arrayClose",
    value: "]",
    child: []
  }
];

function parse(str) {
  let result = str.reduce((bef, cur) => {
    if (cur.type === "arrayOpen") {
      bef.push(cur);
    } else if (cur.type === "arrayClose") {
      if(bef.length === 1) {
        return bef;  
      }
      let arrChild = bef.pop();
      bef[bef.length - 1].child.push(arrChild);
    } else if (cur.type === "string" || cur.type === "number") {
      bef[bef.length - 1].child.push(cur);
    } else if (bef.length === 1) {
      return bef;
    }
    return bef;
  }, []);
  return result;
}
console.log(JSON.stringify(parse(arr), null, 2));
```

기억하시다시피 제이슨파서의 한 부분입니다.  
그럼 위처럼 폴리필을 가져와서 하나하나 알아볼까요?

```javascript
//reduce폴리필
function reduce(arr, condition, firVal) {
  let accumulatedValue;
  if (firVal !== undefined) {
    arr.unshift(firVal);
  }
  for (let i = 0; i < arr.length; i++) {
    if (i === 0) {
      accumulatedValue = arr[0];
      i++;
    }
    accumulatedValue = condition(accumulatedValue, arr[i], i, arr);
  }
  return accumulatedValue;
}
```

여기서의 `condition` 함수는 무엇일까요? 

```javascript
(bef, cur) => {
        if(cur.type === 'arrayOpen') {
            bef.push(cur)
        } else if (cur.type === 'arrayClose') {
            let child = bef.pop();
            bef[bef.length - 1].child.push(child);
        } else if (cur.type === 'string' || cur.type === 'number') {
            bef[bef.length - 1].child.push(cur);
        } else if (bef.length === 1) {
            return bef
        }
        return bef
    }, [])
```

맞습니다. 바로 이거에요.

```javascript
console.log(JSON.stringify(reduce(arr, (bef, cur) => {
  if (cur.type === "arrayOpen") {
    bef.push(cur);
  } else if (cur.type === "arrayClose") {
    if(bef.length === 1) {
      return bef;
    }
    let arrChild = bef.pop();
    bef[bef.length - 1].child.push(arrChild);
  } else if (cur.type === "string" || cur.type === "number") {
    bef[bef.length - 1].child.push(cur);
  } else if (bef.length === 1) {
    return bef;
  }
  return bef;
}, []), null, 2));
```

이렇게 사용할 수 있겠죠. 

여기서 `bef` 는 `[]`  와같고 `cur` 는 현재값, ` {type: "arrayOpen",value: "[",child: []}` 과 같습니다. 그럼 위의 함수에서는 `cur.type === 'arrayOpen'` 이므로 `bef(빈배열)` 에 `cur`값이 들어가겠네요. 그리고 배열이 끝날때 까지 반복될 것입니다.

다시 폴리필을 가져와서 

```javascript
//reduce폴리필
function reduce(arr, condition, firVal) {
  let accumulatedValue;
  if (firVal !== undefined) {
      //1
    arr.unshift(firVal);
  }
  for (let i = 0; i < arr.length; i++) {
    if (i === 0) {
      accumulatedValue = arr[0];
      i++;
    }
      //2
    accumulatedValue = condition(accumulatedValue, arr[i], i, arr);
  }
  return accumulatedValue;
}
```

왜 `bef`가 `[]` 이고 `cur` 가  ` {type: "arrayOpen",value: "[",child: []]` 인지 알아보겠습니다.  

자세히 살펴보시면 알겠지만 `reduce`의 인자로 원래의배열, 함수, 그리고 첫번째 값이 들어가긴했어요.  
그렇게 되면 이 폴리필에서  

1. `firVal` 는  `undefined` 가 아니기 때문에, 주어진 배열의 첫번째 값으로 `firVal` 값 즉 빈 배열이 `unshift(firVal)` 를 통해서 들어가게 됩니다.
2. 그 이후에 첫번째 반복문을 돌며 `arr[0]`값, 즉 빈 배열이 `accumulatedValue` 로 들어가게 되고 그 값을 인자로 받아 condition 함수가 실행이됩니다. 여기서 condition 함수는 위에서 보셨죠? 그것으 인자로 `accumulatedValue..` 즉 ` []` , 그리고 `arr[1](i++이 되었으니까.)` 즉  ` {type: "arrayOpen",value: "[",child: []]` 이 들어가게 되겠죠
   그래서  `bef`가 `[]` 이고 `cur` 가  ` {type: "arrayOpen",value: "[",child: []]` 이 되는겁니다.
3. 그래서 `condition(bef, cur)` 의 값이 다시 `accumulatedValue` 로 들어가게 되고, 그 값을 이용해서 반복문을 돌며 인자로 넣고 반복문이 끝날때까지 반복하면 됩니다. 