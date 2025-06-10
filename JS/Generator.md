## 제네레이터에 대해 설명해주세요.

제네레이터는 자바스크립트에서 이터러블 객체를 쉽게 만들거나 함수의 실행 흐름을 원하는 시점에 멈추고 다시 시작할 수 있게 해주는 특별한 함수입니다.
제네레이터는 ES6에서 도입되었으며 비동기 프로그래밍과 데이터 스트림 처리를 혁신적으로 바꿨습니다.

#### 기본 개념과 문법
일반 함수와 달리 제네레이터는 `function*`키워드로 정의하며 함수 내부에서는 `yield`키워드를 사용하여 값을 순차적으로 반환합니다.

제네레이터 함수는 호출되더라도 즉시 코드를 실행하지 않고 제네레이터 객체를 반환합니다.
이 객체는 이터레이터이자 이터러블입니다.

이터레이터는 `next()`메서드를 통해 제네레이터 함수 내부의 코드를 실행합니다.
`next()`가 호출되면 제네레이터는 이전에 멈췄던 `yield`지점부터 다음 `yield`를 만날 때까지 코드를 실행합니다.

```
function* myGenerator() {
  console.log('첫 번째 실행');
  yield 1; // 여기서 멈춤
  console.log('두 번째 실행');
  yield 2; // 여기서 멈춤
  console.log('세 번째 실행');
  yield 3; // 여기서 멈춤
  console.log('실행 완료');
  // return '끝!'; // return 값은 done: true일 때 value가 됨
}

// 1. 제네레이터 함수를 호출하여 제네레이터 객체를 얻는다. (아직 함수 내 코드는 실행되지 않음)
const gen = myGenerator();

// 2. next()를 호출하여 코드를 실행한다.
console.log(gen.next());
// 첫 번째 실행
// { value: 1, done: false }

console.log(gen.next());
// 출력:
// 두 번째 실행
// { value: 2, done: false }

console.log(gen.next());
// 세 번째 실행
// { value: 3, done: false }

console.log(gen.next());
// 실행 완료
// { value: undefined, done: true } // 더 이상 yield가 없으므로 done: true
```

제네레이터 객체는 이터러블이므로 `for ...of`반복문, 스프레드 문법, 구조 분해 할당 등 이터러블을 사용하는 모든 곳에서 사용할 수 있습니다.
이 방식이 `next()`를 직접 호출하는 것보다 훨씬 일반적입니다.

```
function* numberGenerator() {
  yield 10;
  yield 20;
  yield 30;
}

// 1. for...of 반복문 사용
for (const num of numberGenerator()) {
  console.log(num); // 10, 20, 30 순서로 출력
}

// 2. 스프레드 문법 사용
const numbers = [...numberGenerator()];
console.log(numbers); // [10, 20, 30]
```

### 특징
#### 1. 양방향 통신
제네레이터는 `yield`를 통해 값을 밖으로 내보낼 뿐만 아니라, `next()`메서드의 인자를 통해 값을 안으로 전달 받을 수도 있습니다.
```
function* twoWayCommGenerator() {
  const message1 = yield '첫 번째 질문: 좋아하는 색은?';
  console.log(`사용자 응답 1: ${message1}`);

  const message2 = yield '두 번째 질문: 좋아하는 음식은?';
  console.log(`사용자 응답 2: ${message2}`);
}

const genComm = twoWayCommGenerator();

// 첫 번째 next()는 인자를 전달해도 무시됨 (yield를 만나기 전까지 변수에 할당할 곳이 없음)
console.log(genComm.next().value); // "첫 번째 질문: 좋아하는 색은?"

// 두 번째 next() 호출 시 '빨강'이라는 값을 제네레이터 안으로 전달
console.log(genComm.next('빨강').value); // "두 번째 질문: 좋아하는 음식은?"
// 콘솔 출력: "사용자 응답 1: 빨강"

// 세 번째 next() 호출 시 '피자'라는 값을 전달
genComm.next('피자');
// 콘솔 출력: "사용자 응답 2: 피자"
```

#### 2. `yield*`를 이용한 위임
`yield*`표현식은 다른 제네레이터나 이터러블 객체에 실행을 위임합니다.
현재 제네레이터는 위임된 이터러블이 끝날 때까지 멈추고 그 이터러블의 값들을 순서대로 반환합니다.
```
function* generatorA() {
  yield 1;
  yield 2;
}

function* generatorB() {
  yield 3;
  yield 4;
}

function* combinedGenerator() {
  yield* generatorA();
  yield 5;
  yield* generatorB();
  yield* [6, 7];
}
```

### 주요 활용 사례
제네레이터의 **실행 상태를 보존하면서 실행을 중지하고 재개할 수 있다**는 독특한 특징은 다양한 고급 프로그래밍 패턴에서 유용하게 사용됩니다.

#### 1. 지연 평가
필요한 시점에만 값을 계산하여 메모리를 효율적으로 사용할 수 있습니다.
특히 무한한 데이터 시퀀스를 다룰때 유용합니다.
```
function* fibonacci() {
  let [prev, curr] = [0, 1];
  while (true) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

const fib = fibonacci();
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
console.log(fib.next().value); // 3
// ... 이 과정은 무한히 계속될 수 있음
```

#### 2. 비동기 처리 제어
`async/await`문법이 등장하기 전 제네레이터는 비동기 코드를 동기적인 코드처럼 보이게 작성하는데 널리 사용됐습니다.
`yield`를 사용해 Promise가 완료될 때까지 기다리고, 완료되면 `next()`를 호출하여 다음 작업을 진행하는 방식으로 진행했습니다.

실제 `async/await`은 제네레이터와 Promise를 기반으로 작성되었습니다. 
제네레이터의 동작 원리를 이해하면 `async/await`의 내부를 더 깊이 이해할 수 있습니다.

#### 3. 복잡한 반복 로직 및 커스텀 이터러블 구현
트리 구조의 깊이 우선 탐색과 같은 반복문으로 구현하기 어려운 복잡한 순회 로직을 제네레이터를 사용하여 훨씬 간결하고 명확하게 구현할 수 있습니다.
