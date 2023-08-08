

# 노출모듈 패턴(revealing module pattern)

즉시 실행 함수를 통해 private, public 같은 접근제어자를 만드는 패턴

- 자바스크립트는 private나 public 같은 접근제어자가 존재하지 않고 전역 범위에서 스크립트가 실행됨
    - 노출모듈 패턴을 통해 private와 public 접근제어자 구현

## 자바스크립트

```jsx
const pukuba = (() => {
    const a = 1
    const b = () => 2
    const public = {
        c : 2, 
        d : () => 3
    }
    return public 
})() 
console.log(pukuba)
console.log(pukuba.a)
// { c: 2, d: [Function: d] }
// undefined
```
