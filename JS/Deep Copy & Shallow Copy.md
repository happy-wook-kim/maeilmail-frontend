## 얕은 복사와 깊은 복사에 대해서 설명해주세요

얕은 복사는 데이터 자체를 복사하는 것이 아니라 객체가 참조하는 메모리 주소값을 복사하는 것으로 원본 또는 복사본이 수정될 때 원본도 함께 수정됩니다.
하지만, 깊은 복사는 데이터를 복사하여 새로운 메모리에 할당하는 새로운 객체를 만들어 복사하기에 어느쪽의 값을 변경해도 서로의 값은 독립적으로 존재합니다.

### 얕은 복사 예시
* 스프레드 연산자
```
const ori = {
  a: 1,
  b: { c: 2 },
  d: 3
};

const shallowCopy = { ...ori };

ori.a = 10;
shallowCopy.d = 30;
shallowCopy.b.c = 20;

console.log(ori); // { a: 10, b: { c: 20 }, d: 3 }
console.log(shallowCopy); // { a: 1, b: { c: 20 }, d: 30 }
console.log(ori.b === shallowCopy.b); // true (같은 객체를 참조)
```
* Array.from()
```
const ingredientsList = ["noodles", { list: ["eggs", "flour", "water"] }];
const ingredientsListCopy = Array.from(ingredientsList);

console.log(ingredientsListCopy); // ["noodles", { list: ["eggs", "flour", "water"] }];
console.log(ingredientsList === ingredientsListCopy) // false
console.log(ingredientsList.list === ingredientsListCopy.list) // true
```

`Array.from()` 외에도 `Array.slice()` `Array.concat()` `Object.assign()` 모두 동일하게 얕은 복사입니다.

### 깊은 복사 예시
* JSON을 활용한 방법
```
const ori = {
  a: 1,
  b: { c: 2 }
}

const deepCopy = JSON.parse(JSON.stringify(ori));

deepCopy.a = 10;
ori.b.c = 20;

console.log(ori); // { a: 1, b: { c: 2 } }
console.log(deepCopy); // { a: 10, b: { c: 20 } }
console.log(ori.b === shallowCopy.b); // false (다른 객체를 참조)
```
  
* 재귀 함수로 구현
```
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') {
    return obj; // 원시 값이거나 null이면 그대로 반환
  }

  if (Array.isArray(obj)) {
    const copiedArr = [];
    for (let i = 0; i < obj.length; i++) {
      copiedArr[i] = deepClone(obj[i]);
    }
    return copiedArr;
  }

  const copiedObj = {};
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      copiedObj[key] = deepClone(obj[key]);
    }
  }

  return copiedObj;
}

const ori = {
  a: 1,
  b: { c: 2 },
  d: [1, 2, { e: 3 }]
};

const deepCopy = deepClone(ori);

console.log(ori); // { a: 1, b: { c: 2 }, d: [ 1, 2, { e: 3 } ] }
console.log(deepCopy); // { a: 1, b: { c: 20 }, d: [ 1, 2, { e: 30 } ] }
console.log(ori.b === deepCopy.b); // false
console.log(ori.d === deepCopy.d); // false
console.log(ori.d[2] === deepCopy.d[2]); // false
```
