---
layout: post-write
title:  "자바스크립트"
study: true
---


# 숫자 -> 문자
 1. 값을 String으로 변환.
  
  ```javascript
  var numValue = 1;
  String(numValue);
  ```

 2. 값에 ''를 추가
 ```javascript
  var numValue = 1;
  numValue = numValue + '';
 ```

# 배열에 객체 넣기
 ```javascript
  var data = [];
  var key = "key" //이때 key값의 숫자로 구성되어 있을경우 data는 숫자로 판단한다.
  data[key] = anyValue;
 ```

# prototype.sort();
 매개변수가 없으면 문자열로 정렬.
 ```javascript
  prototype.sort( (a, b) => a-b );
 ```

 객체를 이용할 경우
 ```javascript
  var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic', value: 13 },
  { name: 'Zeros', value: 37 }
 ];

// value 기준으로 정렬
items.sort(function (a, b) {
  if (a.value > b.value) {
    return 1;
  }
  if (a.value < b.value) {
    return -1;
  }
  // a must be equal to b
  return 0;
});
 ```
   sort의 return 값이 1이면 바꿈, -1이면 바꾸지 않는다.
  

 
 
  
  