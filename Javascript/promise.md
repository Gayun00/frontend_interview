
## 🧐 promise란 무엇인가?
비동기 동작을 처리하기 위해 es6에 도입된 문법이다.



자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다.
전통적인 콜백 패턴은 `콜백 지옥`아러 불리는 현상으로 인해 가독성이 나쁘고 버동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있다.

따라서 비동기 처리를 위한 또 다른 패턴으로 프로미스가 도입된 것인데, 전통적인 콜백 패턴이 가진 단점을 보완하여 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.


> 자세히 알아보자.


<br>

### 1️⃣ Promise 사용법
Promise라는 클래스이다. 따라서 이것의 인스턴스를 생성해 promise객체로 비동기 로직을 처리한다.
Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인자로 전달받는다.
```js
  const promise = new Promise((resolve, reject) => {

    if (/* 비동기 작업 성공 */) {
      resolve('succeed');
    }
    else { /* 비동기 작업 실패 */
      reject('failed');
    }
  });
```

Promise는 비동기 처리가 성공했는지, 또는 실패했는지 등의 상태정보(state)를 갖는다.


<br>

### 2️⃣ Promise의 state 상태정보

#### pending
비동기 처리가 아직 수행되지 않은 상태
resolve 혹은 reject함수가 호출되지 않은 상태이다.
#### fulfilled
비동기 처리가 성공한 상태
비동기 처리가 성공하면 콜백함수의 인자로 전달받은 resolve 함수가 호출되고  이때 resolve 메서드의 인자로 비동기 처리 결과를 전달한다.프로미스는 fulfilled 상태가 된다.
#### rejected
비동기 처리가 실패한 상태
실패 시 reject메서드를 호출하고 이때 reject 메서드의 인자로 비동기 처리 결과를 전달한다.프로미스는 rejected 상태가 된다.
#### settled
비동기 처리가 수행된 상태로, 성공 혹은 실패한 상태
resolve 혹은 reject함수가 호출된 상태

<br>

### 3️⃣ 후속 처리 메서드
Promise로 구현된 비동기 함수는 Promise 객체를 반환한다.
이 Promise 객체는 위에서 말한 state, 상태정보를 갖는데, 이 상태에 따라 후속 처리 메소드를 체이닝 방식으로 호출한다.


#### then
then 메서드는 두 개의 콜백 함수를 인자로 받는다.
첫 번째 콜백 함수는 성공 시 호출된다.
두 번째 콜백 함수는 실패시 호출된다.

#### catch
예외(비동기 처리에서 발생한 에러와 then 메서드에서 발생한 에러)가 발생하면 호출된다.
catch 메서드는 Promise를 반환한다.

> then과 catch모두 에러 후속 처리를 한다고? 차이점은 무엇일까?

사실 then보다는 catch로 에러 처리를 하는 것이 더 좋다고 한다.

#### Promise 에러 처리
비동기 처리 결과에 대한 후속 처리는 Promise 객체가 제공하는 후속 처리 메서드 then, catch, finally 모두를 사용할 수 있다.

비동기 처리 시에 발생한 에러는 then 메서드의 두 번째 콜백 함수로 처리할 수 있다.
그러나 단점이 있다.
then의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하며, 코드 또한 복잡해지기 때문에 가독성에 좋지 않다.
```js
  promiseExample(callback)
    .then(res => console.log(res), err => console.error(err));
    .then(res => ...., err => ...);
 ```

catch 메서드를 모든 then메서드를 호출한 이후에 호출한다면, 비동기 처리에서 발생하는 에러(reject함수가 호출된 상태)뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.

```js
promiseExample(callback)
  .then(res => console.xxx(res))
  .catch(err => console.error(err)); // TypeError: console.xxx is not a function
```

또한, catch 메서드를 사용하는 것이 가독성이 좋고 명확하다. 따라서 에러 처리는 then 메서드에서 하지 말고 catch 메서드를 사용하는 것이 더 좋다고 한다.

<br>


## 🧐 비동기 처리란?
#### 동기식
어떤 함수가 동기적으로 로직을 수행한다는 것은 직렬적으로 태스크를 수행한다는 말이다. 즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행중이면 다음 태스크는 대기한다.
반면 비동기식 로직은 병렬적으로 태스크를 수행한다. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 즉시 다음 태스크를 수행한다.
자바스크립트의 대부분의 DOM 이벤트와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기식 처리 모델로 동작한다.


<br>

## 🧐 callback 패턴과 그 문제점
### 1️⃣ callback funtion이란?
함수의 매개변수로 전달되는 함수를 말한다.
#### 고차함수 (Higher Order Function)
그리고 매개변수로 함수를 전달받는 함수를 고차함수라고 한다.
#### 일급함수 
함수를 다른 변수와 동일하게 다루는 언어는 일급함수를 가졌다고 표현한다.

다르게 말하면, 
일급 함수를 가진 언어에서는 함수를 변수처럼 사용할 수 있다. 예를 들면,
- 함수를 변수에 할당하거나
- 함수를 또 다른 함수의 인자로 전달하거나
- 함수의 반환값으로 함수를 전달할 수 있다.

> 일급함수는 함수형 프로그래밍 특징 중 하나로, 자바스크립트 언어 또한 함수형 프로그래밍 기법을 구사할 수 있기에 이 특징을 지닌다.

<br>

### 2️⃣ 콜백함수의 문제점 - 콜백지옥
콜백함수를 전달해주면서 함수 내부에서 계속해서 연속적으로 호출하기 위해서는 콜백이 계속해서 연결되어 가독성이 좋지 않은 비효율적인 코드를 작성하게 된다.

자바스크립트에서 비동기식 처리 모델은 요청을 병렬로 처리하여 다른 요청이 블로킹(작업 중단)되지 않는 장점이 있다.
하지만, 비동기 처리를 위해 콜백 패턴을 사용하면 처리 순서를 보장하기 위해 여러 개의 콜백 함수가 네스팅(nesting, 중첩)되어 복잡도가 높아지는 콜백 헬(Callback hell) 이 발생한다는 단점이 있다.
콜백 헬은 가독성을 나쁘게 하며 실수를 유발하는 원이이 된다.
밑에 코드는 콜백 헬의 예제이다.
```js
  step1(function(value1) {
    step2(value1, function(value2) {
      step3(value2, function(value3) {
        step4(value3, function(value4) {
          step5(value4, function(value5) {
              // value5를 사용하는 처리
          });
        });
      });
    });
  });
```

<br>
## 🧐 promise와 callback
### 1️⃣ 차이점과 장단점
```
function async(callback) {
  setTimeout(() => {
    callback("1초 후 실행");
  }, 1000);
}

async(function (msg) {
  console.log(msg);
});
```


```
function async() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("1초 후 실행");
    }, 1000);
  })
}

async().then(res => {
  console.log(res);
});

```

#### 비동기 로직 처리 용이
callback을 사용하면 비동기 로직의 결과값을 처리하기 위해서는 callback 내부에서만 처리해야 한다.
또한 콜백 함수 밖에서는 비동기에서 온 값을 알 수가 없다.

#### 비동기 결과값 저장
하지만 promise를 사용하면 비동기에서 온 값이 promise객체에 저장되기 때문에 코드 작성이 용이해진다.
또한 콜백함수는 매번 비동기를 실행해야만 그 값을 가져와 사용할 수 있으나, 프로미스는 .then 메서드를 통해서 저장되어 있는 값을 원하는 때 사용할 수 있다.

#### 가독성
비동기 로직의 결과를 다음 비동기로 전달해 실행해야 할 때 callback 은 점점 깊어져서 가독성이 떨어진다.
promise를 사용할 경우 코드의 가독성이 높아지고 뎁스가 많아지지 않는다.

#### 동기 작업의 추가/수정의 유연함

add15 전에 add14라는 비동기 함수를 추가하고 싶다고 하자.
콜백패턴은 다음과 같이 작성해야 한다.
```js
add5(0, (number) => add10(number, (number) => add14(number, add15(number, () => log(number))));

//출처: https://simsimjae.tistory.com/420 [104%]
```

그리고 기존 코드를 일일히 찾아서 수정해야 한다.

promise 패턴은 단순히 사이에 한 줄만 추가하면 된다.

```js
add5(0) .then(number => add10(number)) .then(number => add14(number)) .then(number => add15(number)) .then(number => log(number));

//출처: https://simsimjae.tistory.com/420 [104%]
```
연속된 비동기 작업을 수정, 삭제, 추가하기 편하다.

#### 비동기 작업 상태를 확인하기 용이
콜백패턴은 비동기 작업이 실패했는지, 성공했는지, 대기중인지를 알려주지 않는다. 하지만 promise는
비동기 작업을 추상화한 객체이기 때문에 비동기 작업이 현재 어떤 상태인지를 쉽게 값으로 확인할 수 있다.

<br>

### 2️⃣ promise.all
여러 프로미스를 모두 성공시킨 이후에, 완료된 로직을 실행하고 싶은 경우에 사용한다.

즉 여러개의 비동기 작업들이 존재하고 이들이 모두 완료되었을 때, 작업을 진행하고 싶다면 Promise.all을 활용하면 된다.
Promise.all메서드는 Promise가 담겨 있는 배열 등의 이터러블을 인자로 전달 받는다.
전달받은 모든 프로미스를 병렬로 처리하고 그 처리 결과를 resolve하는 새로운 프로미스를 반환한다.
```js
  Promise.all([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
    new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
    new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
  ]).then(console.log) // [ 1, 2, 3 ]
    .catch(console.log);
```
위의 코드를 살펴보면 모든 Promise의 처리가 성공하면 각각의 프로미스가 resolve한 처리 결과를 배열에 담아 resolve하는 새로운 프로미스를 반환한다.
이때, 첫번째 프로미스가 가장 나중에 처리되더라도 Promise.all 메서드가 반환하는 프로미스는 첫번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 담는다. 그리고 그 배열을 resolve하는 새로운 프로미스를 반환한다. 즉, `처리 순서가 보장된다`.






```js
let urls = [
  'https://api1',
  'https://api2',
  'https://api3'
];

// map every url to the promise of the fetch
let requests = urls.map(url => fetch(url));

// Promise.all waits until all jobs are resolved
Promise
	.all(requests)
  .then(responses => responses.forEach(
    response => alert(`${response.url}: ${response.status}`)
  ));
```

<br>

### 3️⃣ Promise Chaining

비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우, 함수의 호출이 계속해서 겹쳐 콜백 지옥이 발생한다.
프로미스는 후속 처리 메서드를 체이닝(chainning)하여 여러 개의 프로미스를 연결하여 사용할 수 있다. 이를 통해 콜백 헬을 해결한다.
Promise 객체를 반환한 비동기 함수는 프로미스 후속 처리 메서드인 then이나 catch 메서드를 사용할 수 있다.
따라서, then 메서드가 Promise 객체를 반환하도록 하면(then 메서드는 기본적으로 Promise를 반환한다.) 여러 개의 프로미스를 연결하여 사용할 수 있다.
```

  const url = 'http://jsonplaceholder.typicode.com/posts';

 // 포스트 id가 1인 포스트를 검색하고 프로미스를 반환한다.
  promiseAjax('GET', `${url}/1`)
    // 포스트 id가 1인 포스트를 작성한 사용자의 아이디로 작성된 모든 포스트를 검색하고 프로미스를 반환한다.
    .then(res => promiseAjax('GET', `${url}?userId=${JSON.parse(res).userId}`))
    .then(JSON.parse)
    .then(render)
    .catch(console.error);
```

> 그런데, 이 프로미스 체이닝도 한계가 있다.


<br>

## 🧐 async, await란?
ES8에 도입된 문법으로, 비동기 함수를 선언한다.
async function 은 promise를 반환한다.

<br>

### 1️⃣ async
function 앞에 async 키워드를 붙이면 해당 함수는 항상 Promise를 반환한다.
Promise가 아닌 값을 반환하더라도 Promise가 반환되도록 만든다.

> 어떻게 그게 가능하지?

resolved promised로 값을 감싸 반환한다.

```js
  async function f() {
    return 1;
  }
```

<br>

### 2️⃣ await
말 그대로 Promise가 처리될 때까지 이 await 키워드가 붙은 함수의 실행을 기다리도록 한다.
자바스크립트는 await 키워드를 만나면 Promise가 처리 될 때까지 기다렸다가 결과를 반환한다.
```js
  async function f() {
    let promise = new Promise((resolve, reject) => {
      setTimeout(() => resolve("완료"), 1000);
    });
    let result = await promise;

    console.log(result); // "완료"
  }

  f();
```

- 위의 예시에서 await 키워드가 붙은 promise의 결과값이 반환될 때까지 기다린다.
- 1초 뒤 promise가 처리되어 반환된 '완료'문자열이 result 변수에 할당된다.
- 그 뒤 result의 값이 출력된다.


<br>

### 3️⃣ 즉시 실행 함수
Immediately invoked function expression IIFE
외부 함수 없이 바로 await을 사용하고 싶을 때는 IIFE를 사용한다.

async를 항상 붙여서 await가 써야 하지만 이렇게 함수를 선언한 뒤 호출문을 작성하지 않고 바로 실행해 사용하고 싶을 때는 이 방법을 쓰면 된다.

```jsx
(async function() {
	// 내용
})();

(async () => {
	// 내용
})();

const IIFE = (async () => {
  console.log('즉시 실행 함수 표현식');
})();
```

### 4️⃣ 예외 처리
async/await에서는 try/catch문을 사용해 예외를 처리한다.

```js
  async function logTodoTitle() {
    try {
		//
      }
    } catch (error) {
		//
    }
  }
```

<br>




<br>

## 🧐 promise와 async/await의 차이점

### 1️⃣ 가독성
promise chaining 또한 그 뎁스가 길어지면 가독성이 떨어지게 된다.
```js
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      // do something
      return promise2(value1)
        .then(value2 => {
          // do something          
          return promise3(value1, value2)
        })
    })
}
```

async await을 사용하면 직관적이고 가독성 좋은 코드를 작성할 수 있다.
```js
const makeRequest = async () => {
  const value1 = await promise1()
  const value2 = await promise2(value1)
  return promise3(value1, value2)
}
```

<br>

### 2️⃣ 중간값 사용
위의 예시 코드에서도 확인할 수 있듯이, 한번 promise에서 처리한 값을 가지고 다른 액션을 취해야할 때 promise에서는 계속해서 return 해주어야 하지만 async await에서는 깔끔하게 변수에 할당해 사용할 수 있다.
```js
const makeRequest = async () => {
  const value1 = await promise1()
  const value2 = await promise2(value1)
  return promise3(value1, value2)
}
```

<br>
<br>