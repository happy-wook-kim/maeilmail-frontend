## HTML link 요소의 rel 속성 값 preconnect, preload, prefetch에 대해 설명해주세요.


HTML <link>는 현재 문서와 외부 리소스의 관계를 명시합니다. 스타일 시트 연결, 사이트 아이콘 연결 등 다양한 용도로 사용되며, 웹 페이지 성능 최적화에 중요한 역할을 합니다.

<link>의 rel 속성은 모두 성능 최적화를 위한 속성 값은 아래와 같습니다.

* * *

**preconnect** 

브라우저가 외부 도메인과 연결을 미리 설정하여 리소스 요청 시간을 단축합니다.
DNS 조회, TCP 핸드셰이크, TLS 협상 등 연결 설정 과정을 미리 수행합니다. 
CDN과 같이 많은 리소스를 외부에서 가져올 때 유용합니다.

```
<link rel="preconnect" href="https://cdn.example.com" />
```

**preload**

현재 페이지에서 즉시 사용도는 중요한 리소스를 우선적으로 다운로드하도록 브라우저에 지시합니다.
`as`속성을 사용하여 리소스 종류를 명시해야 합니다. (Javascript, CSS, 웹폰트 등)페이지 로딩 속도를 향상시키고자 사용합니다.
```
<link rel="preload" href="/utils/common.ts" as="script" />
```

**prefetch**

다음 방문할 수 있는 페이지에서 사용될 리소스를 백그라운드에서 미리 다운로드하여 캐시에 저장합니다.
페이지 간 이동 속도를 향상시켜 사용자 경험을 개선합니다.
```
<link rel="prefetch" href="/next-page.html" />
```


### 참고
[`MDN <link>`](https://developer.mozilla.org/ko/docs/Web/HTML/Element/link)
