---
layout: post-write
title:  "Combination"
study: true
---


# Combination
  자바스크립트를 이용한 Combination 값이 필요할 경우 참고 하기 위해 작성한다.
 
# nCr
```javascript
function combination(n, r) {
  let top = 1;
  let bottom = 1;

  for(let i=0; i<r; i++) {
    top *= n-i;
    bottom *= r-i;
  }

  return top/bottom;
}
```



