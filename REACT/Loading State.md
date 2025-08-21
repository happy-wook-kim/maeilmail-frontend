## useEffect를 이용하여 로딩 상태 관리하는 방법과 Suspense를 활용하는 방법에 대한 차이점을 설명해주세요.

React에서 비동기 데이터 로딩 상태를 관리하는 두가지 주요 방법인 `useEffect`와 `Suspense`는 각기 **명령형**과 **선언형**이라는 차이를 가집니다.

`useEffect`는 로딩을 어떻게 처리할지 개발자가 직접 모든 단계를 코드로 작성하는 명령형 방식이며,

`Suspense`는 무엇을 보여줄지만 선언하면 React가 나머지 로딩 과정을 알아서 처리하는 선언형 방식입니다.


### useEffect를 이용한 로딩 관리
컴포넌트 내부에서 `isLoading` `data` `error`와 같은 상태 변수를 직접 선언하고 `useEffect``훅 안에서 데이터 fetching의 전 과정을 수동으로 제어하는 방법입니다.

#### 동작 방식
isLoading, data, error를 관리할 세가지 state를 선언합니다.

useEffect 훅 내부에서 비동기 함수를 호출하여 데이터 요청을 시작합니다. 비동기 함수 호출 직전 loading 상태를 `false`로 변경합니다,

데이터를 성공적으로 받아오면 응답값에서 data를 설정하고 loading 상태를 `true`로 변경합니다. 요청 중 에러가 발생하면 error 상태를 `true`로 설정합니다.

컴포넌트의 isLoading, error, data 상태에 따라 로딩 UI, 에러 UI, 성공 UI를 **조건부로 렌더링**합니다.

#### 장단점
데이터 fetching 모든 단게를 개발자가 직접 제어할 수 있다는 장점이 있지만
데이터를 가져오는 컴포넌트마다 isLoading, data, error 상태 관련 로직을 반복적으로 작성해야 합니다.

또한, 여러 개의 비동기 데이터를 한 컴포넌트에서 다룰 경우 로직이 복잡해 질 수 있습니다.

### Suspense를 활용한 로딩 관리
React 18부터 지원되는 `Suspense`는 로딩 상태 관리를 React에 위임하는 선언적인 방식입니다.
개발자는 로딩 UI와 최종 UI만 정의하고 언제 로딩 UI를 보여줄지는 React가 알아서 결정합니다.

#### 동작 방식
로딩 상태를 관리하고 싶은 컴포넌트를 `<suspense>`컴포넌트로 감싸고, `fallback` prop으로 로딩 중에 보여줄 UI를 지정합니다.

`Suspense`내부의 자식 컴포넌트는 `Suspense`를 지원하는 방식으로 데이터를 요청합니다. 데이터가 준비되지 않았다면 컴포넌트는 렌더링을 중단하고 이 상태를 React에 알립니다.
* 이때, fetch 함수는 Suspense와 호환되지 않습니다. Suspense를 지원하는 fetching 라이브러리(Tanstack Query, SWR)이나 React의 `use`훅을 사용해야 합니다.

데이터 로딩이 완료되면 React는 렌더링을 중단했던 컴포넌트를 다시 렌더링합니다.

#### 장단점
컴포넌트는 성공 상태의 UI 렌더링만 고려하면 되고 로딩과 에러 처리는 `Suspense`와 `ErrorBoundary`에 위임되므로 코드가 간결해집니다.

하지만 Suspense와 호환되는 데이터 fetching 라이브러리를 사용하거나 React의 최신 API를 도입해야 합니다.
