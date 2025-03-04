```
function change(a, b, c) {
    a = 'a changed'
    b = { b: 'changed' };
    c.c = 'changed';
}

let a = 'a unchanged';
let b = { b: 'unchanged' };
let c = { c: 'unchanged' };

change(a, b, c);

console.log(a, b, c); // ?
```







***

정답은 

a: a unchanged

b: { b: 'unchanged' }

c: { c: 'changed' }

a는 call by value 로 복사본이 생긴 케이스이므로 기존 a에 영향을 주지 않습니다.
b는 change 함수 내부에서 b를 재할당하여 { b: 'changed' }의 참조 값을 가리키게 되지만, 함수 외부의 b는 변경되지 않습니다.
c는 함수 내부와 외부의 변수 모두 동일한 참조 값을 가르키고 있습니다. 함수 내부 변수의 속성을 변경하면 함수 외부의 c의 속성도 변경됩니다.
