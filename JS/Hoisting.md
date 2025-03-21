## 자바스크립트 호이스팅에 대해서 설명해주세요.

호이스팅은 JavaScript는 **코드를 실행하기 전에, 코드를 한번 훑어보면서 변수 및 함수 선언을 미리 파악**해두는 것입니다.
변수 및 함수 선언이 해당 스코프의 최상단으로 끌어올려지는 것처럼 동작하는 것이며 
실제 코드가 이동하는 것은 아니지만, 코드 실행 전에 변수와 함수가 이미 선언된 것처럼 동작합니다.
그래서 함수를 선언 이전에 불러오는 것이 가능합니다.

```
console.log(foo); // undefiend
console.log(bar); // Uncaught ReferenceError: Cannot access 'bar' before initialization
console.log(baz); // Uncaught ReferenceError: Cannot access 'baz' before initialization

var foo = 10;
let bar = 20;
const baz = 30;
```

ES6 이전에는 `var`키워드를 사용해 변수를 선언했습니다. `var`변수는 선언 전에 사용할 수 있지만 중복 선언이나 함수 레벨 스코프를 가짐으로서 다양한 문제가 발생했습니다.
가장 큰 문제는 예측불가능한 점으로 예상치 못한 에러를 유발한다는 것이었습니다.

이런 문제를 해결하고자 `let` `const`와 같은 키워드로 변수를 선언할 수 있게 되었습니다.
`let`과 `const`로 선언된 변수는 선언 전에 접근하려고 하면 **ReferenceError**가 발생합니다.
변수 선언부터 실제 변수 선언 코드까지 구간을 **TDZ(Temporal Dead Zone)**라고 합니다.

`let` `const` 또한 호이스팅이 적용됩니다.

```
let a = 10;

if(true) {
  console.log(a); // 10
}
```

```
let a = 10;

if(true) {
  console.log(a); // ReferenceError 발생
  let a = 20;
}
```

위 코드는 전역 변수 a가 선언되어 있음에도 `if`문 안에있는 블록 스코프를 참조하기에 나타나는 에러입니다. 이를 통해 호이스팅이 적용된다는 사실을 알 수 있습니다.
