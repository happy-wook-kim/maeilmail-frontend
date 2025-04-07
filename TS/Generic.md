## 타입스크립트에서 제네릭이란 무엇인가요?

제네릭은 타입스크립트에서 **타입을 마치 함수의 파라미터처럼 사용하는 기능**입니다. 
클래스, 함수, 인터페이스 등을 정의할 때 특정 타입을 미리 확정하지 않고 사용하는 시점에 타입을 지정할 수 있게 합니다.
이를 통해 **코드의 재사용성**을 극대화하면서 **타입 안정성**을 유지할 수 있습니다.

### 제네릭을 왜 사용하나요?

제네릭의 가장 큰 장점은 **코드 재사용성**을 높이면서 동시에 **타입 안정성**을 지킬 수 있다는 점입니다.
예를 들어, `any`타입을 사용하면 모든 타입을 허용하지만 타입스크립트의 타입 추론과 검사 기능이 무력화되어 컴파일 시점에 오류를 잡기 어렵습니다.
반면, 제네릭을 사용하면 다양한 타입을 유연하게 처리하면서 함수나 클래스 내부 로직과 실제 사용 시점의 타입 간 **일관성을 강제**합니다.
이를 통해 컴파일러가 잘못된 타입 사용을 사전에 감지하고 오류를 방지할 수 있습니다.

### 사용법
제네릭은 일반적으로 `<>`안에 타입 변수(보통 `T`로 표기)를 선언하여 사용합니다.
```
// 제네릭 함수
function identity<T>(arg: T): T {
  return arg;
}

// 제네릭 인터페이스
interface GenericInterface<T> {
  value: T;
}

// 제네릭 클래스
class GenericClass<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}
```

### 실제 사용 예시
```
interface User {
  id: string;
  name: string;
  email: string;
}

interface Product {
  id: string;
  name: string;
  made: string
}

type MarketResponseData<T> = {
  id: string;
  data: T; // T는 사용 시점에 지정되는 타입
  location: string;
}

// 1. User 데이터를 담는 응답
const userResponse: MarketResponseData<User> = {
  id: 'response-123',
  data: {
    id: 'user-abc',
    name: '홍길동',
    email: 'gildong@example.com'
  },
  location: '서울'
};

// 2. Product 데이터를 담는 응답
const productResponse: MarketResponseData<Product> = {
  id: 'response-456',
  data: {
    id: 'product-xyz',
    name: '최고급 키보드',
    made: '대한민국'
  },
  location: '부산'
};

// 잘못된 사용 예시 (컴파일 오류 발생)
const invalidResponse: MarketResponseData<User> = {
  id: 'response-789',
  data: {
    id: 'prod-error',
    name: '잘못된 상품',
    made: '중국' // Error: 'made'는 User 타입에 존재하지 않음
  },
  location: '인천'
};
```

위 예시처럼 제네릭을 사용해 서로 다른 타입을 처리하면서 타입 안정성을 가질 수 있습니다.
반면, `any`를 사용하면 이런 유연성은 얻을 수 있지만 타입 오류를 사전에 잡을 수 없어 런타임에 문제가 발생할 수 있습니다.
제네릭은 **예측 가능하고 안전한 코드**를 작성할 수 있게 도와줍니다.
