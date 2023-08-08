# 6.이터레이터 패턴

생성일: August 8, 2023 10:12 AM

# 이터레이터 패턴(iterator pattern)

이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴

- 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회 가능

## 자바스크립트에서 이터레이터 패턴

```jsx
const mp = new Map() 
mp.set('a', 1)
mp.set('b', 2)
mp.set('cccc', 3) 
const st = new Set() 
st.add(1)
st.add(2)
st.add(3) 
const a = []
for(let i = 0; i < 10; i++)a.push(i)

for(let aa of a) console.log(aa)
for(let a of mp) console.log(a)
for(let a of st) console.log(a)
 
/* 
a, b, c 
[ 'a', 1 ]
[ 'b', 2 ]
[ 'c', 3 ]
1
2
3
*/
```

- 다른 자료 구조인 set과 map임에도 똑같은 `for a of b`라는 이터레이터 프로토콜을 통해 순회


> 💡 이터레이터 프로토콜
>
> - 이터러블한 객체를 순회할 때 쓰이는 규칙
>
> 이터러블한 객체
>
> - 반복 가능한 객체로 배열을 일반화한 객체
