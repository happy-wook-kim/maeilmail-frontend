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

**a: a unchanged**

**b: { b: 'unchanged' }**

**c: { c: 'changed' }**

***

a는 call by value 로 값의 복사본이 생긴 케이스이므로 함수 내부 a가 변경되어도 함수 외부 a에 영향을 주지 않습니다.

b는 객체이므로 change 함수의 파라미터로 전달할 때 b의 값을 전달하는게 아닌 b의 값이 있는 주소를 복사하여 전달합니다. 이후 함수 내부의 b에 새로운 객체를 할당하면서 새로운 객체의 주소를 생성하고 할당합니다. 함수 외부 b의 참조 값(주소)는 변경되지 않습니다.

c도 객체이므로 change 함수 파라미터로 전달할 때 주소를 복사하여 전달하는 것은 동일하지만, 참조하는 주소에서 속성을 변경하는 것이므로 새로운 객체를 생성하고 새로운 주소를 할당하는 것이 아닌 복사한 주소의 속성을 변경하는 것입니다. 함수 외부의 c도 동일한 주소를 참조하므로 함수 외부 c의 속성 값이 변경됩니다.
