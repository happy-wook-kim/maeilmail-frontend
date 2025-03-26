## ES6에 대해 설명해 주세요

ECMA International에서 표준화한 ECMAScript는 **자바스크립트 표준 사양**입니다.
2015년에 발표된 ECMAScript 2015를 **ES6**라고 불리며 자바스크립트 역사상 가장 큰 변화를 가져온 버전입니다. ES6이후로 매년 새로운 표준이 발표되고 있습니다. 

ES6 이전에는 `var`의 호이스팅 문제, 복잡한 비동기 처리, 모듈 시스템 부재 등으로 코드 관리가 어려웠습니다. 
ES6는 이러한 문제점들을 해결하고, 모던 JavaScript 개발의 기반을 마련하여 자바스크립트 생태계에서 중요한 변곡점으로 평가되고 있습니다.

### 1. let, const

기존 사용하던 변수 `var`은 **함수 레벨 스코프**를 가지기에 함수 외부에서 생성한 변수는 모두 전역 변수로 취급합니다. 이로 인한 전역 변수 남발의 가능성이 있고
for문 변수 선언문에서 선언한 변수를 for문 코드 블럭 외부에서 참조할 수 있습니다.
**var 키워드를 생략**하고 변수를 선언할 수 있었으며, **중복 선언**을 허용하는 문제점도 있습니다.
변수 **호이스팅**이 적용되어 선언 전에 사용할 수 있었습니다.

전역변수는 **편리하지만 예상하지 못한 부작용**을 야기합니다. 그렇기에 블록 스코프를 통해 변수의 스코프를 좁혀 사용하는 것이 예측하기 좋은 코드를 만들 수 있습니다.
이러한 관점에서 나타난 `let` `const`. **블록 레벨 스코프**를 가지고 있어 전역 변수와 지역 변수를 구분합니다.
이 두 변수는 선언 전에 호출이 되면 ReferenceError를 내뱉는 선언 전에 접근할 수 없는 임시적인 사각지대(TDZ)에 빠집니다.
`let`은 값의 재할당이 가능한 변수이고 `const`는 값의 재할당이 불가능한 선언과 동시에 할당이 이루어져야하는 상수입니다. 객체를 담고 속성을 변경하는 것은 메모리를 수정하지 않기에 가능합니다.

### 2. 화살표 함수

화살표 함수는 `function` 대신 `=>`를 사용하여 함수를 선언할 수 있게 합니다. 
`this`의 호출방식에 따라 동적으로 가리키는 값이 변경되는 헷갈리는 점에 의해 만들어졌습니다. 
또한, 간단한 함수의 경우 간단하게 작성이 가능하여 코드 가독성도 좋아질 수 있습니다.
```
// 일반 함수
function add(a, b) {
  return a + b;
}

// 화살표 함수
const add = (a, b) => a + b;
```

### 3. 클래스

`class` 키워드를 사용해 클래스를 정의하고 상속, 생성자, 메서드 등을 구현할 수 있습니다. 
```
// 클래스
class Person {
  // 클래스 필드 정의
  constructor(name, age) { // 생성자
    this.name = name; // this는 인스턴스를 가리킨다.
    this.age = age;
  }

  speak() {
    console.log(`${this.name}: hello!`);
  }
}

const person = new Person("kim", 20);
person.speak(); // kim: hello!
```

### 4. 템플릿 리터럴

새로운 문자열 표기법으로 **백틱(`)**을 사용하여 문자열 내에 변수나 표현식을 편리하게 작성할 수 있습니다.

```
const name = 'Selly';
const color = 'Black';

// 템플릿 리터럴 사용
const message = `Hello. My name is ${name} and I love ${color} color.
Thanks a lot.`;

console.log(message) // Hello. My name is Selly and I love Black color.
```

### 5. 객체 리터럴 확장

객체에서도 리터럴을 사용할 수 있습니다. 객체 속성명과 변수명이 같을 때 속성명을 생략할 수 있고, 객체의 속성명을 동적으로 받을 수 있습니다.
```
const name = "Bob";
const age = "20";
const key = "Question";

const person = {
  name,
  age,
  [key]: "What is React?"
}
```

### 6. 프로미스

Promis는 비동기 작업의 최종 성공 또는 실패를 나타내는 객체입니다. 콜백 지옥을 해결하기 위해 만들어졌습니다. 비동기 코드를 더 깔끔하고 가독성 좋게 작성할 수 있습니다.
```
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const data = { message: "Success" };
      // 성공
      resolve(data);

      // 실패
      // reject(new Error("Failed"));
    });
  });
}
```

### 7. 구조 분해 할당

배열이나 객체를 분해하여 값을 쉽게 추출하여 변수에 할당할 수 있습니다.
```
// 배열 구조 분해
const fruit = ['banana', 'orange', 'apple'];
const [first, second, third] = fruit;
console.log(first, second, third); // banana orange apple

// 객체 구조 분해
const user = { id: 12, userName: "Selly" };
const { id, userName } = user;
```

### 8. 전개 연산자

`...`을 사용하여 배열이나 객체를 확장하거나 펼치는데 사용합니다. 이를 사용하여 배열이나 객체의 요소를 추출하거나 복제하여 다른 배열이나 객체에 포함시킬 수 있습니다.
```
// 배열 복제
const arr1 = [1, 2, 3];
const arr2 = [...arr1];
console.log(arr1, arr2); // [1, 2, 3] [1, 2, 3]

// 객체 병합
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj); // { a: 1, b: 2, c: 3, d: 4 }
```

### 9. 모듈

`import` `export`를 사용하여 코드를 모듈 단위로 분리하고 재사용할 수 있습니다. 
```
// utils.js
export function add(a, b) {
  return a + b;
}

// main.js
import { add } from "../utils.js";

const sum = add(3, 4);
console.log(sum); // 7
```

### 10. 파라미터 기본값

함수의 파라미터에 기본값을 부여할 수 있습니다.
```
const addMoney = (money, add = 100) => {
  console.log(money + add);
}
addMoney(100); // 200
```

이 외에도 수많은 변경점이 있습니다. 자세한 사항은 [link](https://262.ecma-international.org/6.0/)에서 확인할 수 있습니다.
이로인해 자바스크립트의 영향력을 강하게 만들었고 언어 활용을 효율적이고 유지보수하기 쉽게 만들었습니다. 이후에도 ES7, ES8 등 매년 새로운 기능이 추가되고 있습니다.
더 나은 코드 작성을 위해 꾸준히 학습해야합니다.
