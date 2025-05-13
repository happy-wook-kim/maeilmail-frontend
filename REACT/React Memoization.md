## React의 Memoization

Memoization은 복잡하거나 비용이 큰 함수 호출의 결과값을 캐시해두고, 동일한 입력값이 다시 들어올 경우 캐시된 값을 반환하여 함수의 실행을 피하고 성능을 최적화하는 기법입니다.
React에서는 이 원리를 활용하여 불필요한 리렌더링을 방지하고 연산 결과를 재사용함으로써 애플리케이션의 반응성과 효율성을 크게 향상시킬 수 있습니다.

### memo
`React.memo`는 React의 고차 컴포넌트로 컴포넌트의 Props가 변경되지 않은 경우 리렌더링을 건너뛰는 기능입니다.
이는 컴포넌트를 래핑하여 Props가 변경되지 않으면 이전 렌더링 결과를 재사용하는 방식으로 동작합니다.
아래의 예시에서 name이 변경되지 않는다면 Greeting 컴포넌트는 리렌더링되지 않습니다.
```
import { memo } from 'react';

const Greeting = memo(function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>
});

function App() {
  const [name, setName] = useState("Selly");

  return (
    <Greeting name={name} />
  );
}
```
`memo`는 Props의 얕은 비교만 수행하므로 객체 Props가 내부 값은 같아도 참조가 달라지면 리렌더링이 발생할 수 있습니다.

### useMemo
`useMemo`는 계산 결과를 캐싱하는 리액트 훅입니다.
복잡한 연산이나 리소스 사용이 큰 연산을 렌더링마다 반복 계산하는 것이 부담스러운 경우 `useMemo`를 통해 계산값을 캐싱하여 성능을 최적화 합니다.
`useMemo`는 의존성 배열에 있는 변수가 변경될 때만 계산을 다시 수행합니다.
```
import { useMemo } from 'react';

function filterTodos(todos, tab) {
  let startTime = performance.now();
  while (performance.now() - startTime < 500) {
    // 매우 느린 코드를 시뮬레이션하기 위해 500ms 지연
  }

  return todos.filter(todo => {
    if (tab === 'all') {
      return true;
    } else if (tab === 'active') {
      return !todo.completed;
    } else if (tab === 'completed') {
      return todo.completed;
    }
  });
}

function TodoList({ todos, tab }) {
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab]
  );

  return (
    <ul>
      {visibleTodos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

### useCallback
`useCallback`은 함수를 캐싱하는 리액트 훅입니다.
리렌더링마다 함수를 새로 생성하는 것이 부담스러운 경우 `useCallback`을 사용하여 동일한 함수 인스턴스를 재사용할 수 있습니다.
`useMemo`와 마찬가지로 의존성 배열에 있는 변수가 변경될 때만 함수가 새로 생성됩니다.
특히, 자식 컴포넌트에 함수를 Props로 전달할 때 유용합니다.

아래의 예시에서 `handleClick`이 `useCallback`으로 캐싱되지 않으면 `count`가 변경될 때마다 `<Button>`이 불필요하게 리렌더링 됩니다.
```
import { useCallback } from 'react';

const Button = memo(({ onClick }) => {
  console.log('Button 렌더링');
  return <button onClick={onClick}>클릭</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('버튼 클릭됨');
  }, []);

  return (
    <>
      <Button onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>카운트: {count}</button>
    </>
  );
}
```
