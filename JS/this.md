## this에 대해 설명해주세요.

JavaScript에서 `this`는 현재 실행중인 코드의 컨텍스트를 가리키는 키워드 입니다.
컨텍스트란 함수가 호출될 때 생성되는 환경으로 `this`는 이 환경에서 어떤 객체가 현재 작업의 주체인지 나타냅니다.
즉 `this`는 런타임에 동적으로 바인딩되며 함수 호출 방식에 따라 그 값이 바뀝니다.

같은 함수라도 호출 방식에 따라 `this`가 가리키는 대상이 달라질 수 있습니다. 
이러한 특성으로 `this`는 객체 지향 프로그래밍, 이벤트 처리, 콜백 함수 등 다양한 상황에서 활용됩니다.

### this 활용법
#### 1. 일반 함수 호출
일반 함수 호출에서 `this`는 전역 객체를 가리킵니다. 브라우저 환경에서는 `window` Node.js환경에서는 `global`객체입니다.
"use strict" 모드에서는 undefined로 설정됩니다.

#### 2. 메서드 호출
객체의 메서드로 함수를 호출하면 `this`는 해당 객체를 가리킵니다. 메서드를 호출한 객체가 `this`의 컨텍스트가 됩니다.
```
const obj = {
  name: 'name',
  showName: function() {
    console.log(this);
  }
};

obj.showName(); // { name: 'name', showName: [Function] }
```

#### 3. 생성자 함수 호출
`new`키워드로 함수를 호출하면 이는 생성자 함수로 동작하며 `this`는 새로 생성된 인스턴스를 가리킵니다.
```
function Person(name) {
  this.name = name;
}

const person1 = new Person('Alice');
console.log(person1.name); // Alice
console.log(person1); // Person { name: 'Alice' }
```

#### 4. 명시적 바인딩
`call` `apply` `bind`메서드를 사용하면 `this`를 명시적으로 설정할 수 있습니다.
* `call` : 함수를 즉시 호출하며, `this`를 첫 번째 인자로 전달된 객체로 설정합니다.
* `apply` : `call`과 비슷하지만 인자를 배열로 전달합니다.
* `bind` : `this`를 영구적으로 바인딩한 새로운 함수를 반환합니다.
```
function showThis() {
  console.log(this);
}

const obj = { name: 'Object' };

showThis.call(obj); // { name: 'Object' }
showThis.apply(obj); // { name: 'Object' }

const boundFunc = showThis.bind(obj);
boundFunc(); // { name: 'Object' }
```

#### 5. 화살표 함수
화살표 함수는 `this`를 바인딩하지 않습니다. 대신 화살표 함수가 정의된 스코프의 `this`를 사용합니다.
이를, **렉시컬 스코프**라고 부르며 상위 컨텍스트의 `this`를 그대로 가져옵니다.
```
const obj = {
  name: 'name',
  showName: function () {
    const arrowFunc = () => {
      console.log(this);
    }
  }
};

obj.showName(); // { name: 'name', showName: [Function] }
```

#### 6. 이벤트 핸들러
이벤트 핸들러 함수 내에서 `this`는 **이벤트를 발생시킨 DOM 요소**를 가리킵니다.
```
document.getElementById('myButton').addEventListener('click', function() {
  console.log(this); // <button id="myButton">...
});
```

#### 7. 클래스에서 사용
ES6 클래스에서 `this`는 클래스의 인스턴스를 가리킵니다. 메서드 내에서 `this`는 해당 인스턴스를 참조합니다.
```
class MyClass {
  constructor(name) {
    this.name = name;
  }

  showThis() {
    console.log(this);
  }
}

const instance = new MyClass('Instance');
instance.showThis(); // MyClass { name: 'Instance' }
```

#### 결론
JavaScript에서 `this`는 **함수가 호출되는 방식에 따라 동적으로 결정되는 컨텍스트**를 가리키는 키워드입니다.
일반 함수, 메서드, 생성자, 화살표 함수, 이벤트 핸들러, 클래스 등 다양한 상황에서 `this`의 동작이 다르므로 호출 방식을 정확히 이해하는 것이 중요합니다.
`this`를 올바르게 활용하면 객체 지향 프로그래밍, 이벤트 처리, 비동기 작업 등에서 강력한 기능을 발휘할 수 있습니다.
