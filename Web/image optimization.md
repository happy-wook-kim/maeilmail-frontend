## 이미지 크기가 클 경우 렌더링 속도가 느려질 텐데, 이를 개선하기 위한 방법들을 설명해주세요.

웹 성능 최적화에서 이미지, 비디오와 같은 미디어 리소스 최적화는 매우 중요한 부분입니다.
특히 이미지는 일반적으로 큰 용량을 차지하기 때문에 웹페이지 로딩 속도에 큰 영향을 미칩니다.
이미지 용량을 줄이면 다운로드 속도가 빨라지고, 이는 곧 브라우저의 렌더링 속도 향상으로 이어져 사용자 경험을 개선합니다.
특히, FCP(First Contentful Paint)와 LCP(Largest Contetnful Patin) 같은 핵심적인 웹 성능 지표를 개선하는 데 이미지 최적화가 큰 역할을 합니다.

### 이미지 최적화 방법

#### 이미지 압축
원본 이미지의 해상도가 높으면 불필요하게 파일 크기가 커집니다. 웹페이지에서 실제로 필요한 해상도로 조절하는 것이 좋습니다.
2456x1680의 이미지를 1228x840으로 압축한다면 **2.1MB였던 이미지가 70KB로** 압축됩니다.
웹페이지 사용자는 2.1MB를 다운받는 시간에서 70KB를 다운받는 시간으로 이미지 로드가 빠른 경험을 할 수 있습니다. 
웹페이지에는 수많은 이미지들이 있으니 모든 이미지들을 압축한다면 **엄청난 성능 최적화**를 할 수 있습니다.

#### 이미지 형식
이미지 압축을 했다면 이미지 형식을 고려하는 것도 좋은 방법입니다.
`WebP`형식은 **압축률이 뛰어나고 투명도와 애니메이션까지 지원**하는 이미지 형식입니다. 
크롬에서 만든 형식이다보니 크롬은 2014년부터 지원을 시작했고 다른 브라우저에서는 2020년 이후 업데이트한 브라우저라면 대부분 지원하고 있습니다.
1MB였던 `jpg`파일을 `WebP`로 변경했을 때 **422KB로 용량을 낮출 수** 있습니다.

#### 반응형 이미지
`<picture>`를 사용하면 브라우저의 뷰포트 크기, 해상도, 이미지 형식 지원 여부에 따라 가장 적합한 이미지를 선택하여 로드할 수 있습니다.
```
<picture>
  <source srcset="image-large.webp" type="image/webp" media="(min-width: 1024px)">
  <source srcset="image-medium.webp" type="image/webp" media="(min-width: 768px)">
  <source srcset="image-small.webp" type="image/webp">
  <source srcset="image-large.jpg" type="image/jpeg" media="(min-width: 1024px)">
  <source srcset="image-medium.jpg" type="image/jpeg" media="(min-width: 768px)">
  <img src="image-small.jpg" alt="구형 이미지 포맷을 마지막에 꼭 배치해야합니다.">
</picture>
```

#### lazy load
사용자가 스크롤하여 이미지가 뷰포트에 들어올 때 이미지를 로드하는 기법입니다. 페이지의 초기 로딩 속도를 크게 향상시킬 수 있습니다.
```
// loading 속성으로 구현
<img src="image.jpg" loading="lazy" alt="descrption" />

// IntersectionObserver로 구현
const observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.src = entry.target.dataset.src;
      observer.unobserve(entry.target);
    }
  });
});

images.forEach(image => {
  observer.observe(image);
});
```
두 가지 방법으로 구현이 가능합니다.
