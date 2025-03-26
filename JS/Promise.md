## 자바스크립트 Promise에 대해서 설명해주세요.

Promise는 자바스크립트 *비동기 작업의 결과를 나타내는 객체*이며 ES6에 도입되었습니다. 

Promise가 등장하기 전에는 비동기 처리를 위해 콜백함수를 사용했습니다.
하지만 콜백 함수를 중첩해서 사용하면 코드가 복잡해지고 가독성이 떨어지는 '콜백 지옥' 문제가 발생했습니다.

Promise는 이러한 콜백 지옥 문제를 해결하고, 비동기 처리를 더 쉽고 깔끔하게 관리하기 위해 도입되었습니다.
Promise는 아래와 같은 세 가지 상태를 가집니다.
* `pending`(대기) : 비동기 작업이 아직 완료되지 않은 초기 상태.
* `fulfilled`(이행) : 비동기 작업이 성공적으로 완료된 상태. 결과 값을 가집니다.
* `rejected`(거부/실패) : 비동기 작업이 거부되거나 실패한 상태. error값을 가집니다.

Promise 객체는 `new Promise()` 생성자를 통해 생성하고 `resolve`와 `reject`콜백 함수를 인자로 받는 executor 함수를 전달합니다.
비동기 작업이 성공하면 `resolve`를 호출하고 실패하면 `reject`를 호출합니다.

```
const testPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const randomNumber = Math.random();
    if(randomNumber > 0.5) {
      resolve(randomNumber); // resolve() 호출
    }else {
      reject(new Error("number too small")); // reject() 호출
    }
  }, 1000);
});
```

`then()` 메소드를 사용하여 Promise가 성공했을 때 콜백 함수를 등록하고, `catch()` 메소드를 사용하여 실패했을 때 콜백 함수를 등록할 수 있습니다.
`finally()` 메소드는 성공, 실패 여부에 상관없이 실행됩니다.

```
testPromise
  .then((value) => {
    console.log("성공", value);
  })
  .catch((error) => {
    console.error("실패", error);
  })
  .finally(() => {
    console.log("프로미스 종료");
  });
```

`Promise.all()`은 여러 **Promise를 병렬로 실행**하고 모두 성공했을 때 결과를 반환하는 메소드입니다. 
하나라도 실패하면 전체가 실패처리 됩니다. 성공한 것만 사용하려면 `Promise.allSettled()`를 사용할 수 있습니다.
