## localStorage와 sessionStorage에 대해 설명해주세요.

localStorage와 sessionStorage는 브라우저에서 제공하는 웹 스토리지 API로 데이터를 키-값 형태로 저장합니다.

### LocalStorage
localStorage는 데이터를 영구적으로 저장합니다.
사용자가 직접 데이터를 삭제하거나 웹 애플리케이션 코드에서 삭제하지 않는 한, 브라우저를 닫거나 컴퓨터를 재시작하더라도 데이터가 유지됩니다.
또한, 동일한 Origin 내 모든 탭에 데이터를 공유합니다.

다크모드 같은 테마 설정이나 사용자 설정 데이터 같이 장기적으로 유지해야 하는 데이터 저장에 적합합니다.

```
// 데이터 저장
localStorage.setItem('theme', 'dark');
localStorage.setItem('userPreferences', JSON.stringify({ notifications: true, language: 'ko' }));

// 데이터 읽기
const currentTheme = localStorage.getItem('theme'); // 'dark'
const userPrefs = JSON.parse(localStorage.getItem('userPreferences')); // { notifications: true, language: 'ko' }

// 데이터 삭제
localStorage.removeItem('theme');

// 모든 데이터 삭제
localStorage.clear();
```

### SessionStorage
sessionStorage는 현재 열려 있는 브라우저 탭의 세션 동안에만 유지됩니다.
해당 탭을 닫거나 브라우저를 닫으면 데이터는 자동으로 삭제됩니다. 
같은 Origin 이더라도 탭 간 데이터를 공유하지 않습니다.

로그인 후 데이터를 일시적으로 저장하거나 특정 탭에서만 사용하는 데이터를 관리하는데 적합합니다.

```
// 데이터 저장
sessionStorage.setItem('currentStep', '2');
sessionStorage.setItem('formData', JSON.stringify({ name: '홍길동', email: 'gildong@example.com' }));

// 데이터 읽기
const step = sessionStorage.getItem('currentStep'); // '2'
const formData = JSON.parse(sessionStorage.getItem('formData'));
```

### 주의점
localStorage나 sessionStorage는 브라우저 개발자 도구를 통해 쉽게 접근하고 내용을 확인하거나 수정할 수 있습니다.
데이터는 평문으로 저장되므로 보안상 주의가 필요합니다.
* **민감 정보 저장 금지** : 비밀번호, 개인 식별 정보, 신용카드 정보, API 키, 세션 토큰등 민감하거나 중요한 데이터는 웹 스토리지에 저장하지 않아야 합니다.
* **XSS 공격 취약성** : 악의적인 스크립트가 웹 페이지에 삽입될 경우, 웹 스토리지에 저장된 데이터가 탈취될 위험이 있습니다. localStorage는 영구적이고 모든 탭에서 공유되므로 특히 더 주의해야 합니다.

민감한 데이터는 서버 측 세션 관리를 통해 보관해야합니다.
HttpOnly, Secure 플래그가 설정된 쿠키를 통해 저장하고 서버 측에서만 해독하여 사용하도록 구현해야 합니다.
