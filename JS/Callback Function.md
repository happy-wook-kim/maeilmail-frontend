## 콜백 함수에 대해 설명해주세요.

콜백 함수는 다른 함수의 인자로 전달되는 함수입니다.
전달된 콜백 함수는 자신을 인자로 전달받은 함수 내부에서 특정 시점(특정 작업 완료, 이벤트 발생 등)에 응답으로 호출됩니다.

### 주요 사용 목적
* **비동기 처리** : JavaScript는 싱글 스레드 언어이지만 시간이 오래 걸리는 작업을 기다리지 않고 다음 코드를 실행하기 위해 비동기 방식을 사용합니다. 
콜백 함수는 이러한 비동기 작업이 완료됐을 때 성공 또는 실패에 대한 결과를 처리하거나 후속 작업을 수행하기 위해 사용되었습니다.
* **이벤트 처리** : 웹 브라우저에서 버튼 클릭이나 키보드 입력 등 이벤트가 발생했을 때 실행될 함수를 등록할 때도 콜백 함수가 사용됩니다.
* **반복 작업** : 배열의 `forEach` `map` `filter`메소드처럼 각 요소에 대해 특정 작업을 수행하는 함수를 전달할 때에도 콜백 함수가 사용됩니다.

비동기 작업을 수행하기위해 사용되었습니다.
```
// 데이터를 가져오는 비동기 함수 (setTimeout으로 비동기 흉내)
function getData(userId, onSuccess, onError) {
  setTimeout(() => {
    // 성공/실패 시나리오를 랜덤하게 시뮬레이션
    const success = Math.random() > 0.3; // 70% 확률로 성공

    if (success) {
      const data = { id: userId, name: `User ${userId}`, email: `user${userId}@example.com` };
      console.log("데이터 가져오기 성공!");
      // 성공 시에는 onSuccess 콜백 함수를 호출하며 데이터를 전달
      onSuccess(data);
    } else {
      const error = new Error("데이터를 가져오는데 실패했습니다.");
      console.error("데이터 가져오기 실패!");
      // 실패 시에는 onError 콜백 함수를 호출하며 에러 객체를 전달
      onError(error);
    }
  }, 1500);
}

// 성공 시 실행될 콜백 함수
function handleSuccess(userData) {
  console.log("성공 콜백 실행됨:", userData);
  // 여기서 받아온 데이터로 추가 작업 수행
}

// 실패 시 실행될 콜백 함수
function handleError(error) {
  console.error("실패 콜백 실행됨:", error.message);
  // 여기서 에러 상황에 맞는 처리 수행
}

// getData 함수 호출 시 성공/실패 콜백 함수를 인자로 전달
getData(123, handleSuccess, handleError);

console.log("getData 함수 호출 완료. 결과 기다리는 중...");
```
위 예시처럼 `getData`함수는 비동기 작업을 수행하고 작업의 결과에 따라 미리 전달받은 콜백 함수가 호출됩니다.
나의 동작에 대한 응답(Callback)으로서의 역할을 수행합니다.

### 콜백 지옥
복잡한 로직에서 비동기 작업이 순차적으로 필요할 수 있습니다. 
예를 들어 사용자 ID를 가져오고 그 ID로 게시글 목록을 조회하고 첫 번째 게시글 데이터에서 댓글을 조회하는 로직이 필요할 수 있습니다.
이런 상황을 콜백 함수로 구현하면 콜백 함수 내부에 또 다른 비동기 함수 호출과 콜백 함수가 중첩되는 구조가 반복됩니다.
```
getUser(userId, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      // 댓글 처리 로직...
      getLikes(comments[0].id, (likes) => {
        // 좋아요 처리 로직...
        // 계속 중첩...
      }, handleError);
    }, handleError);
  }, handleError);
}, handleError);
```
이처럼 코드가 계속 들여쓰기 되며 깊어지는 형태를 **콜백 지옥**이라고 부릅니다.
콜백 지옥은 가독성이 매우 떨어지는 코드이며 오류가 발생했을때 오류를 추적하여 디버깅하기 어렵게 만듭니다.

이러한 문제들로 인해 ES6에서 `Promise`가 나타났습니다.
`Promise`는 비동기 동작 수행 결과를 반환하는 객체로 `.then()` `.catch()`메소드를 통해 성공 실패시 로직을 간편하게 작성할 수 있습니다.

이후 ES8에서 async/await가 추가되면서 비동기 코드를 마치 동기 코드처럼 순차적으로 작성할 수 있게 되었습니다.
`try ... catch`와 결합하여 동기 코드와 유사한 방식으로 에러를 처리할 수 있어 가독성과 유지보수성을 크게 향상시켰습니다.
