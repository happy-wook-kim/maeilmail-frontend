## SSR(Server Side Rendering)에 대해 설명해주세요.

전통적인 웹은 JSP, PHP, Spring같은 기술로 개발되었으며 멀티페이지 구조를 기반으로 각 HTML파일에 JS,CSS를 불러오는 형태입니다.

2010년 Angular가 발표되고 2013년 React가 등장하면서 웹 개발은 프론트엔드와 백엔드로 분리되기 시작했습니다.
이 시기에 프론트엔드에서는 SPA(Single Page Application)가 유행하기 시작했으며 SPA는 CSR(Client Side Rendering)방식으로 구현됐습니다. 

CSR에서는 `index.html`이라는 빈 파일에 JS를 통해 DOM을 조작하고 컴포넌트를 불러와 사용자 인터랙션에 반응하는 웹 애플리케이션 만들었습니다.

### CSR의 문제점
CSR을 사용하다보니 몇가지 문제점이 드러났습니다.
* **SEO 문제**: `index.html`이 비어 있어 크롤링 봇이 콘텐츠를 제대로 인덱싱하지 못해 SEO에 불리했습니다. 
이를 해결하기 위해 Prerender나 SSG(Static Site Generation) 같은 방식이 발전했고, 크롤링 봇이 JS 렌더링을 기다린 후 크롤링하는 방향으로 개선되기 했지만 여전히 초기 빈 HTML 구조로 인해 한계가 남아 있었습니다.
* **초기 로딩 속도**: JS로 DOM을 그리다보니 JS 파일 크기가 커졌습니다.
번들링, JS minification, JS mangling, 정적파일 Brotli 압축, 코드 스플리팅, Tailwind CSS 같은 최적화 기술이 등장했지만 여전히 초기 로딩 속도가 느려졌습니다.
이는 사용자가 웹 사이트에 처음 진입할 때 시간이 더 걸리고 첫 인터랙션도 지연되는 문제를 낳았습니다.

### SSR의 등장과 장점
이러한 문제들로 인해 SSR이 나타났습니다.

SSR은 서버에서 웹 사이트를 렌더링한 후 완성된 HTML을 클라이언트에 제공합니다. 이를 통해 사용자는 JS를 다운받고 화면을 그리는 과정을 기다릴 필요가 없어졌습니다.
인터렉션을 위한 JS는 여전히 필요했으나 레이아웃과 페인팅 과정이 서버에서 처리되어 초기 로딩 속도가 개선됐습니다.
또한 서버에서 미리 렌더링된 완성된 페이지를 크롤링 봇이 볼 수 있어 SEO에 유리합니다.

### SSR의 문제점
하지만 SSR에도 몇 가지 문제점이 있습니다.
* **페이지 전환 속도**: 페이지 전환이 발생하면 브라우저가 서버에 새 요청을 보내고 서버가 다시 HTML을 렌더링해 응답합니다.
이는 **필요한 데이터만 비동기로 가져와 DOM을 부분 업데이트**하는 CSR의 클라이언트 측 라우팅보다 **네트워크 요청과 서버 처리 시간이 추가**로 소요되어 느릴 수 있습니다.
* **서버 리소스 사용 증가**: 렌더링 작업이 클라이언트에서 서버로 옮겨감에 따라 서버 부하가 증가하고, 이는 서버 비용 상승으로 이어집니다.
* **개발 복잡성 증가**: React를 기준으로 서버 컴포넌트와 클라이언트 컴포넌트를 구분해야 하며 데이터 fetching 방식과 라이프사이클 관리를 서버와 클라이언트 환경 모두에서 고려해야 합니다.
이로 인해 개발자의 러닝 커브가 발생합니다.

### 결론
CSR이 더 좋다 SSR이 더 좋다라고 단정할 수는 없습니다. 서비스의 특성에 따라 적합한 방식을 선택해야합니다.
서비스의 특성, SEO 요구 사항, 초기 로딩 성능 요구 사항, 사용자 인터랙션의 중요도, 개발팀의 숙련도 등을 종합적으로 고려하여 결정해야 합니다.
