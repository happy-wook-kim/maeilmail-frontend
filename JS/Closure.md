## 클로저에 대해 설명해주세요.

클로저(Closure)란 함수가 자신이 정의될 때의 렉시컬 환경을 기억하여 함수가 자신의 렉시컬 스코프 외부에서 실행될 때에도 해당 환경에 접근할 수 있는 기능 또는 그러한 함수 자체를 뜻합니다.

### 클로저의 중요성
변경되면 안되는 중요한 데이터를 보호하고 함수 내부에서만 접근 가능하도록 코드를 작성 해야합니다.
예를 들어, 계정의 id, role 등 고정된 값을 최초로 받아온 다음 이후에는 조작하지 않고 값을 불러오기만 해야합니다.
```
function getMyAuth() {
  let auth = { role: 'master', id: '10000000' };
  // 비동기 데이터 로딩 시나리오
  // async function initAuth() { auth = await fetch(...); }
  // initAuth();

  function getRole() {
    return auth.role;
  }

  function getId() {
    return auth.id;
  }

  return {
    getRole,
    getId
  }
}

const myAuth = getMyAuth();
console.log(myAuth.getId()); // 10000000
console.log(myAuth.getRole()); // master
console.log(myAuth.auth); // undefined

myAuth.auth.role = 'guest'; // auth의 값이 undefined 이므로 에러 발생
console.log(myAuth.getRole()); // master 
```

위 예제처럼 myAuth는 auth의 데이터를 수정할 순 없고 가져올 수만 있게됩니다. 
이처럼 클로저는 **데이터 은닉과 캡슐화**를 구현하여 데이터를 안전하게 관리해야 하는 상황에서 유용합니다.
