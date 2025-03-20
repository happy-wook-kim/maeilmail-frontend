## useEffect

useEffect란 컴포넌트가 렌더링 된 후 외부 API 호출, 이벤트 리스너 등록, DOM 조작 등을 처리하기 위한 리액트 훅입니다.

```
// 아래와 같은 구조를 가지고 있습니다.
useEffect(setup, dependencies)

// 사용 예시: 채팅 서버 연결 및 연결 해제
useEffect(() => {
  const connection = createConnection(serverUrl, roomId);
  connection.connect();

  // 컴포넌트 언마운트 시 실행되는 cleanup 함수
  return () => {
    connection.disconnect(); 
  };
}, [serverUrl, roomId])

```

useEffect는 컴포넌트가 초기 렌더링 된 후 실행되며, dependencies 배열의 값이 이전 렌더링 값과 얕은 비교를 통해 변경될 때 마다 setup 함수가 실행됩니다.
컴포넌트가 언마운트 될 때 cleanup 함수가 실행됩니다.

위 예시에서 dependencies 배열[] 안에 들어가는 dependencies의 값이 변경됨이 감지되면 setup 함수가 실행됩니다.

dependencies 배열에는 주로 useState, useRef.current, 컴포넌트의 props, useCallback, useMemo 등의 값을 사용합니다. 
또는, 빈 배열[]을 넣어 컴포넌트가 초기 렌더링 순간에만 setup 함수를 실행하기도 합니다.
