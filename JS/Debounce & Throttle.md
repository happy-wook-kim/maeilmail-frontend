## 디바운스와 쓰로틀에 대해서 설명해주세요.

디바운스와 쓰로틀은 짧은 시간 내에 연속적으로 발생하는 이벤트를 제어하여 **불필요한 함수 호출을 줄이는 기법**입니다.
이를 통해 **웹 성능을 최적화**하고 **리소스 낭비를 방지**하며 **사용자 경험을 개선**할 수 있습니다.
두 기법은 이벤트 처리 빈도를 조절하지만 실행 방식과 사용 목적에 차이가 있습니다.

### 디바운스
디바운스는 동일한 이벤트가 여러번 호출되는 경우 **마지막으로 호출된 이벤트만 실행하는 방식**입니다.
이벤트가 발생하면 지정된 시간동안 기다립니다.
이 시간 내에 동일한 이벤트가 추가로 발생하면 타이머를 초기화하고 마지막 이벤트부터 다시 지정된 시간 동안 기다립니다.
최종적으로 추가 이벤트가 발생하지 않으면 마지막 이벤트를 실행합니다.

이를 통해 실행될 함수의 호출 횟수를 줄입니다.
검색어를 입력했을때 검색 리스트를 하단에 보여주는 동작이나 입력 유효성을 검사하는데 사용됩니다.

```
function debounce(func, wati) {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// 사용 예: 검색 입력 처리
const search = debounce((query) => {
  console.log(`검색어: ${query}`);
}, 300);

document.querySelector('input').addEventListener('input', (e) => {
  search(e.target.value);
});
```

### 쓰로틀
쓰로틀은 동일한 이벤트가 여러번 호출되는 경우 **일정 주기마다 한 번만 이벤트를 실행**하도록 제한하는 방식입니다.
이벤트가 발생하면 즉시 함수를 실행하고 지정된 시간동안 추가 이벤트를 무시합니다.
지정된 시간이 지나면 다시 새로운 이벤트를 처리할 수 있습니다.

이를 통해 함수의 실행 빈도 자체를 일정 주기마다 한 번으로 제한할 수 있습니다.
스크롤 이벤트 처리나 API 요청을 제한할 때 사용됩니다.

```
function throttle(func, wait) {
  let isThrottled = false;
  return function (...args) {
    if (!isThrottled) {
      func.apply(this, args);
      isThrottled = true;
      setTimeout(() = {
        isThrottled = false;
      }, wait);
    }
  };
}

// 사용 예: 스크롤 이벤트 처리
const logScroll = throttle(() => {
  console.log('스크롤 위치:', window.scrollY);
}, 100);

window.addEventListener('scroll', logScroll);
```

### 결론
디바운스는 이벤트가 연속적으로 발생한 뒤 최종 상태만 처리하고 싶을 때 적합합니다. 예를 들어 사용자가 입력을 멈춘 후 서버 요청을 보내는 경우에 적합합니다.
쓰로틀은 이벤트를 즉각 실행해야하며 일정한 주기로 처리하고 싶을 때 적합합니다. 예를 들어 스크롤 중 실시간으로 UI를 업데이트하는 경우에 적합합니다.
