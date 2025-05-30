## script 태그에서 async와 defer의 차이점에 대해서 설명해주세요.

`<script>`는 HTML문서 내에서 자바스크립트 코드를 작성하거나 외부 `.js`파일을 불러오는데 사용됩니다.
하지만 최근에는 Webpack, Vite, esbuild와 같은 모듈 번들러의 등장으로 `<script>`를 직접 사용하는 대신 번들러를 통해 자바스크립트 파일들을 묶어서 사용합니다.
외부 스크립트를 불러올 때는 여전히 `<script>`를 유용하게 사용합니다.

`<script>`에 `async`나 `defer`와 같은 속성을 추가하여 스크립트 파일을 불러오고 실행하는 방식을 제어할 수 있습니다. 

### async
* HTML 파싱과 **병렬적으로** 스크립트 파일을 백그라운드에서 다운로드합니다.
* 다운로드가 완료되면 **즉시** 스크립트를 실행합니다. 이때 HTML 파싱이 완료되지 않았다면, 파싱을 일시중단하고 스크립트를 실행합니다.
* 여러 개의 `async` 스크립트가 있는 경우 다운로드가 완료되는 순서대로 실행됩니다.

### defer
* HTML 파싱과 **병렬적으로** 스크립트 파일을 백그라운드에서 다운로드합니다.
* 다운로드가 완료되어도 즉시 실행하지 않고, HTML 파싱이 완전히 끝난 후 스크립트를 실행합니다.
* 여러 개의 `defer` 스크립트가 있는 경우 HTML 문서에 등장하는 순서대로 실행됩니다.

어떤 속성이 좋다기보다는 **상황에 맞게** 속성을 선택하여 추가해야합니다.
다른 스크립트나 DOM에 **의존성이 없는 경우** `async`를 다른 스크립트나 DOM에 의존성이 있고 주요 콘텐츠를 렌더링하는 데 필요한 스크립트라면 `defer`를 선택하는 것이 좋습니다.
