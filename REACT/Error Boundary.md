## Error Boundary가 무엇인가요?

Error Boundary는 React에서 제공하는 기능으로 컴포넌트에서 에러가 발생했을 때 Error Boundary에 정의된 항목으로 렌더링하여 애플리케이션이 다운 되는 것을 방지하는 매커니즘입니다.
Error Boundary는 반드시 클래스형 컴포넌트로 작성해야하고 아래 두 메소드 중 하나 이상을 정의해야 합니다.
`getDerivedStateFromError()`는 에러가 발생했을 때 호출되며 state를 업데이트하여 다음 렌더링에서 대체 UI를 보여주도록 합니다.
`componentDidCatch(e, info)`는 에러가 발생했을 때 호출되며 에러 로깅과 같은 부가 작업을 수행하기에 적합합니다.
Error Boundary는 클래스형 컴포넌트로만 작성이 가능하므로 함수형 컴포넌트에서 활요하려면 클래스형 컴포넌트로 Error Boundary를 정의한 후 감싸는 형태로 사용해야 합니다.

```
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.log("Error: ", error, errorInfo);
  }

  render() {
    if(this.state.hasError) {
      return <h1>에러 발생</h1>;
    }
    return this.props.children;
  }
}

<ErrorBoundary>
  <Component />
</ErrorBoundary>
```

컴포넌트에서 에러가 발생해도 애플리케이션에서 에러가 발생하여 다운되는 것이 아니라 `<h1>에러 발생</h1>`과 같이 미리 정의한 화면을 보여줄 수 있어 에러를 컨트롤 할 수 있게됩니다.
에러를 컨트롤하여 사용자 경험을 향상 시킬 수 있고 errorInfo를 확인하여 디버깅에 도움이 될 수 있습니다.

### 한계
Error Boundary는 렌더링 과정, 생명주기 메소드, 생성자 내에서 발생하는 에러만 포착한다는 한계가 있습니다.
**이벤트 핸들러, 비동기 코드(setTimeout, promise 등), 서버 사이드 렌더링, Error Boundary 자체의 에러**는 포착하지 못합니다.
이러한 에러에 대해서도 `try catch` 등 다양한 방법으로 에러에 대한 예외처리를 진행해야 합니다.
