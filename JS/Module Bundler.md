## webpack, rollup과 같은 번들러는 왜 필요한지 설명해주세요.
모듈 번들러는 JavaScript, CSS, 이미지 등 다양한 **파일을 하나의 번들로 묶어주는 도구**입니다.
이를 통해 웹 애플리케이션 성능을 최적화하고 개발 경험을 개선합니다.

### 네트워크 요청 최적화
여러 개의 파일을 개별적으로 요청하면 네트워크 부하가 증가합니다. 
번들러는 파일을 하나 또는 소수의 번들로 통합하여 HTTP 요청 수를 줄이고 로딩 속도를 향상시킵니다.

### 코드 최적화
* **압축과 난독화**: 불필요한 공백,주석을 제거하고 변수명을 짧게 변경(minifying)하여 파일 크기를 줄입니다.
또한, 난독화(mangling)를 통해 코드를 읽기 어렵게 만들어 리버스 엔지니어링을 방지합니다.
* **트리 쉐이킹**: import 되었지만 실제 코드에서 사용하지 않는 코드를 분석하여 최종 번들에서 제거합니다. ES 모듈 기반 코드에서 보다 효과적입니다.
* **스코프 호이스팅**: 여러 모듈의 코드를 단일 스코프로 통합하여 런타임 성능을 개선합니다.

### 호환성 제공
최신 JavaScript 문법을 사용해 코드를 작성한 경우 구형 브라우저를 사용자는 해당 문법을 해석할 수 없어 에러가 발생합니다.
번들러는 이러한 경우를 대비하여 Babel, SWC와 같은 트랜스파일러를 사용해 구형 브라우저에서도 동작할 수 있도록 코드를 변환합니다.

### 모듈 간 충돌 방지
여러 JavaScript 파일에서 동일한 변수명을 사용할 경우 충돌이 발생합니다.
번들러는 각 모듈을 고유한 스코프로 분리하거나 변수명을 자동으로 변환하여 이러한 문제를 해결합니다.

### 개발 경험 개선
번들러는 개발 서버와 HMR(Hot Module Replacement)을 제공하여 코드 변경 시 페이지를 새로고침하지 않고 실시간으로 반영합니다.
CommonJS, ESM 둘다 지원하며 개발자가 원하는 방식으로 코드를 작성할 수 있게 합니다.


모듈 번들러는 네트워크 성능 최적화, 코드 크기 축소, 호환성 보장, 개발 생산성 향상 등 다양한 이점을 제공합니다.
현대 웹 개발에서 사용자들에게도 개발자들에게도 이러한 이점으로 필수적인 요소로 자리잡았습니다.
