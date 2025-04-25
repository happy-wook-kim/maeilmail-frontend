## never와 unknown 타입에 대해서 설명해주세요.

### never
`never`타입은 **절대로 발생할 수 없는 값**의 타입입니다. 이름처럼 어떤 값도 `never`타입 변수에는 할당될 수 없습니다.

#### 특징
* **값 없음**: `never`타입 변수에는 어떠한 값도 할당할 수 없습니다.
* **모든 타입의 서브타입(Bottom Type)**: 
`never`는 `number` `string` `object` 등 모든 타입의 서브타입입니다. 따라서 `never`타입 값은 어떤 타입의 변수에도 할당 가능합니다.
이는 논리적으로 `never`타입 값이 존재할 수 없기 때문에 타입 제약을 위반하지 않기 때문입니다.

#### 사용사례
**1. 항상 오류를 발생시키는 함수**
```
function throwError(message: string): never {
  throw new Error(message);
}
```
* 함수가 값을 반환하지 않고 항상 예외를 던지는 경우, 반환 타입으로 `never`를 사용합니다.
**2. 무한 루프 함수**
```
function infiniteLoop(): never {
  while(true){
    console.log('Loop ...');
  }
}
```
* 함수가 무한 루프에 빠져 절대 반환되지 않을 때 `never`를 사용합니다.

### unknown
`unknown`타입은 **알 수 없는 타입**을 나타냅니다. `any`타입과 비슷하게 모든 타입의 값을 `unknown`타입 변수에 할당할 수 있는 **최상위 타입(Top Type)**입니다.
모든 타입은 `unknown`타입의 서브타입입니다.

#### `any`와의 주요 차이점 및 특징
* **타입 안정성**: `unknown`타입 값은 직접적으로 어떤 작업도 수행할 수 없습니다. 속성 접근, 메소드 호출, 연산(+, -, * 등)이 불가능합니다.
* **할당 제한**: `unknown`타입 값은 `unknown`또는 `any`를 제외한 다른 타입 변수에 직접 할당할 수 없습니다.
* **타입 검사 필요**: `unknown`타입 값을 사용하려면 타입가드(`typeof`, `instanceof`) 또는 타입 단언(`as`)을 통해 구체적인 타입을 알려주어야 합니다.

이러한 특징 덕분에 `unknown`은 `any`에 비해 훨씬 안전한 타입입니다. `unknown`은 개발자가 명시적으로 타입을 확인하고 처리하도록 강제합니다.

#### 사용 사례
외부 API 호출 결과
```
async functoiin fetchData(): Promise<unknown> {
  const response = await fetch(url);
  return await response.json();
}

async function processData() {
  const data: unknown = await fetchData();

  // 타입 가드로 안전하게 처리
  if(typeof data === 'object' && data !== null && 'name' in data) {
    console.log((data as { name: string }).name);
  }else if(Array.isArray(data)) {
    console.log(`Length: ${data.length}`);
  }
}
```
* 외부 시스템에서 받아온 데이터의 타입이 불확실할 때 `unknown`을 사용해 안전하게 처리합니다.
