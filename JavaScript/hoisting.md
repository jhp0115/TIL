# Hoisting

- `함수 선언`은, 함수 문 전체가 호이스팅된다.
- `변수`(let, const)와 `클래스`는 식별자 선언만 호이스팅되고, 초기화는 되지 않는다.



## 블럭 내부에서의 호이스팅
```js
let a = 1;
{
    console.log(a);  // 오류가 난다.
    let a = 2;
}
```
위의 코드는 왜 오류가 날까?  
스코프 단위로도 호이스팅이 일어나기 때문에, JS 엔진이 중괄호 내부에 있는 코드를 실행할 때 해당 스코프에도 a라는 이름의 변수의 존재를 알고 있기 때문에 전역 범위에 있는 a 변수를 갖고 오려하지 않고 중괄호 내부의 a를 가져다 쓰려고 했는데, 그 변수의 값이 아직 아무 값으로도 초기화가 되어 있지 않아서 오류가 난 것이다.