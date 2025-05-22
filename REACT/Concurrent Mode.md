## 리액트 동시성 모드(Concurrent Mode)에 관해서 설명해주세요.

React 18 버전부터 본격적으로 도입된 **동시성**은 리액트가 여러 작업을 동시에 처리하고 우선순위에 따라 작업을 관리할 수 있게 하는 핵심적인 내부 메커니즘입니다.
동시성을 통해 복잡한 UI 환경에서도 애플리케이션의 반응성을 향상시켜 사용자 경험을 극대화할 수 있습니다.

과거 리액트에서는 하나의 렌더링 작업이 시작되면 완료될 때까지 다른 작업을 막는 동기적 방식으로 동작했습니다.
이로 인해 애플리케이션이 복잡해지고 UI 업데이트에 많은 연산이 필요한 상황에서 렌더링 도중에 사용자 입력에 대한 반응이 지연되어 사용자가 앱이 멈췄다고 느끼는 상황에 발생할 수 있습니다.

동시성 렌더링 환경에서는 리액트가 렌더링 작업을 작은 단위로 나누어 처리하며 필요한 경우 진행 중인 렌더링을 일시 중단하고 사용자의 입력과 같이 더 시급한 업데이트를 먼저 처리한 후 나중에 중단했던 작업을 이어서 진행할 수 있습니다.
여러 작업 흐름을 동시에 관리하면서 가장 중요한 흐름을 먼저 처리하는 것입니다.

### useTransition
`useTransition`은 **UI 변경을 긴급하지 않은 전환으로 표시하여 리액트가 해당 업데이트로 인해 다른 긴급한 업데이트가 느려지는 것을 방지**하는 훅입니다.

#### 주요 목적
긴급하지 않은 상태 업데이트를 표시하여 UI 반응성을 유지합니다.
전환이 진행 중인 동안 로딩 상태를 UI에 표시할 수 있게 해줍니다.

#### 사용 방법
`useTranstion`은 두개의 값을 배열 형태로 반환합니다.
1. `isPending` : 전환 작업이 진행 중인지 여부를 나타냅니다. `true`이면 전환이 진행 중임을 의미하며 이를 사용하여 로딩 UI를 표시할 수 있습니다.
2. `startTransition` : 긴급하지 않은 상태 업데이틀 이 함수로 감싸줍니다. `startTransition`으로 감싸진 업데이트는 리액트에 의해 우선순위가 낮게 처리됩니다.

```
import React, { useState, useTransition } from 'react';

// 가상의 큰 데이터 목록
const allItems = Array.from({ length: 10000 }, (_, i) => `아이템 ${i + 1}`);

function App() {
  const [searchTerm, setSearchTerm] = useState('');
  const [filteredItems, setFilteredItems] = useState(allItems);
  const [isPending, startTransition] = useTransition();

  const handleInputChange = (event) => {
    const newSearchTerm = event.target.value;
    setSearchTerm(newSearchTerm); // 입력값 업데이트는 즉시 (긴급)

    // 검색 결과 필터링은 시간이 걸릴 수 있으므로 transition으로 처리
    startTransition(() => {
      setFilteredItems(
        allItems.filter(item => item.includes(newSearchTerm))
      );
    });
  };

  return(
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={handleInputChange}
        placeholder="검색..."
      />
      {isPending && <p>결과 로딩 중...</p>}
      <ul>
        {filteredItems.map(item => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </div>
  );
}
```

### useDeferredValue
`userDeferredValue`는 **특정 값의 업데이트를 지연시켜 UI의 다른 부분들이 먼저 업데이트**될 수 있도록 하는 훅입니다.

#### 주요 목적
UI의 렌더링 비용이 큰 특정 부분이 새로운 데이터로 업데이트되는 것을 지연시켜 다른 중요한 부분의 반응성을 유지합니다.
리액트가 이전 값을 잠시 보여주다가 새로운 값에 대한 렌더링이 준비되면 부드럽게 업데이트할 수 있도록 합니다.

#### 사용 방법
`useDeferredValue`는 하나의 필수 인자와 선택적인 두 번째 인자를 받습니다.

```
import React, { useState, useDeferredValue } from 'react';

// 가상의 큰 데이터 목록을 필터링하는 함수 (비용이 큰 작업 가정)
const filterItems = (items, term) => {
  if (!term) return items;
  // 실제로는 복잡한 필터링 로직
  return items.filter(item => item.includes(term));
};

const allItems = Array.from({ length: 10000 }, (_, i) => `항목 ${i + 1}`);

function App() {
  const [searchTerm, setSearchTerm] = useState('');
  const deferredSearchTerm = useDeferredValue(searchTerm, { timeoutMs: 500 }); // 검색어 업데이트 지연

  // 지연된 검색어로 목록 필터링
  const filteredItems = filterItems(allItems, deferredSearchTerm);

  const handleInputChange = (event) => {
    setSearchTerm(event.target.value); // 입력 필드는 즉시 업데이트
  };

  return (
    <div>
      <input
        type="text"
        value={searchTerm} // 입력창은 실제 searchTerm을 사용
        onChange={handleInputChange}
        placeholder="검색..."
      />
      {/* 목록은 deferredSearchTerm을 기반으로 업데이트되므로 약간의 지연이 있을 수 있음 */}
      {/* searchTerm과 deferredSearchTerm이 다르면, "이전 결과"를 보여주고 있음을 알 수 있음 */}
      {searchTerm !== deferredSearchTerm && <p>결과 업데이트 중...</p>}
      <ItemList items={filteredItems} />
    </div>
  );
}
```

### 동시성의 장점 및 영향
* **향상된 사용자 경험** : UI가 멈추거나 버벅거리는 현상이 줄어들어 사용자는 애플리케이션이 항상 빠르고 부드럽게 반응한다고 느낍니다.
* **개선된 인식 성능** : 실제 연산 시간이 줄어들지 않더라도 중요한 업데이트를 먼저 처리하고 나머지를 백그라운드에서 진행함으로써 사용자가 느끼는 성능이 개선됩니다.
* **복잡한 UI 처리 용이** : 데이터 시각화, 애니메이션, 대규모 데이터 목록 등 복잡한 UI 요소들도 다른 부분의 반응성을 해치지 않으면서 처리할 수 있습니다.
* **새로운 UI 패턴 가능** : `Suspense`와 결합하여 데이터 패칭, 코드 스플리팅 등에서 더 정교한 로딩 상태 관리 및 UI 패턴 구현이 가능해집니다.
