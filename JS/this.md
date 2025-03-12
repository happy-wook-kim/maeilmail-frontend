## 상황에 따라 this 바인딩이 어떻게 이뤄지는지 설명해주세요.

Javascript에서 `this`는 함수 호출 방식에 따라 값이 동적으로 결정되는 특별한 키워드입니다.
`this`는 '이 함수를 호출한 어떤 것' 이라고 생각할 수 있습니다. 하지만, 호출 방식에 따라 '어떤 것'이 달라지므로 헷갈리게 합니다.

`this`는 매개변수와 객체 자신이 가지고 있는 멤버변수명이 같을 경우 이를 구분하기 위해 사용됩니다.
또한, 코드 재사용성과 유연성을 가지므로 `this`를 사용합니다.

```
// this 없이 객체마다 별도의 함수를 정의해야 함 (코드 중복)
const person1 = {
  name: "Alice",
  greet: function() {
    console.log("Hello, " + person1.name); // person1에 고정
  }
};

const person2 = {
  name: "Bob",
  greet: function() {
    console.log("Hello, " + person2.name); // person2에 고정
  }
};

person1.greet(); // Hello, Alice
person2.greet(); // Hello, Bob
```

```
// this를 사용하여 하나의 함수를 여러 객체에서 재사용
function greet() {
  console.log("Hello, " + this.name); // this를 사용하여 호출한 객체의 name 속성에 접근
}

const person1 = {
  name: "Alice",
  greet: greet // greet 함수를 person1의 메서드로 할당
};

const person2 = {
  name: "Bob",
  greet: greet // greet 함수를 person2의 메서드로 할당
};

person1.greet(); // Hello, Alice
person2.greet(); // Hello, Bob

// call, apply를 사용하여 더 유연하게 활용
greet.call({ name: "Charlie" }); // Hello, Charlie
greet.call({ name: "Bob" }); // Hello, Bob
```
위와 같이 `this`를 사용하면 `greet`함수는 person1과 같은 객체에 묶여 있지 않습니다.

`this`는 함수가 호출되는 방식에 따른 값의 변경 예시를 설명하겠습니다.

1. 전역 호출

전역에서 함수가 호출되면, `this`는 **전역 객체**를 참조합니다.
```
function globalFunction() {
  console.log(this);
}
globalFunction(); // 브라우저: window, Node.js: global
```

2. 메서드 호출

객체의 메서드로 호출된 함수에서는 `this`가 **해당 객체**를 참조합니다.
```
const person = {
  name: 'Selly',
  hello: function () {
    console.log(this.name);
  }
};
person.hello(); // Selly
```

3. 생성자 함수와 클래스

생성자 함수나 클래스에서 `this`는 **새로 생성되는 객체, 인스턴스**를 참조합니다.
```
function Person(name) {
  this.name = name;
};
const person = new Person('Selly');
console.log(person.name); // Selly
```

4. 명시적 바인딩

`call()`, `apply()`, `bind()` 메서드를 사용하면 `this`를 명시적으로 바인딩할 수 있습니다.
```
function greet() {
  console.log(this.name);
}
const user = { name: "Selly" };
greet.call(user); // Selly
```

5. 화살표 함수

화살표 함수는 **상위 스코프의 `this`값을 상속**받습니다. 
```
const person = {
  name: 'Selly',
  hello: () => console.log(this.name) // undefined
};
```

6. 이벤트 핸들러

이벤트 핸들러에서는 `this`는 일반적으로 **이벤트를 발생시킨 DOM 요소**를 참조합니다.
```
button.addEventListener("click", function () {
  console.log(this); // 클릭된 button 요소
});
```

위 예시 처럼 `this`는 호출 방식에 따라 값이 달라지므로 각 상황을 이해하고 적절하게 사용합니다.
그럼에도 많은 상황에서 헷갈릴 여지가 많아 2015년 ECMAScript 6에서 화살표 함수가 나타났습니다.
