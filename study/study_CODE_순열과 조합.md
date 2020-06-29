---
layout: post-write
title:  "순열과 조합"
study: true
---


# nPr, nCr 값
  
  수학이다.
  nPr => n개의 경우를 r번 사용할경우.
  nCr => nPr에서 중복 제외

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

function permutation(n, r) {
  let value = 1;

  for(let i=0; i<r; i++) {
    value *= n-i;
  }

  return value;
}
```


# 배열의 원소로 만들수 있는 가능한 모든 조합

  자주 나오는 문제이다. 기능을 추가해서 해결해야할 문제들이니 잘 알아두자. 
```javascript
//list 값은 문자열!
function findAllPermutation(stringList, prevValue) {
  const frontPaddingValue = prevValue || '';
  return stringList.reduce((listNumbers, value, index) => {
    listNumbers.push(frontPaddingValue+value);

    const nextNumberList = [...stringList];
    nextNumberList.splice(index, 1);

    const result = findAllPermutation( 
    nextNumberList, frontPaddingValue + value
    );
    listNumbers.push(...result);

    return listNumbers;
  }, []);
}
```
 잘 생각해 보자. 배열 내 각각의 요소를 저장한 그 전값과 합쳐주면 된다. 단 각각의 요소를 연선할 때 동일한 조건을 가지도록 해야 한다. (reduce의 강점.)
  ex [1, 2, 3]
   1을 가지고 들어가서 [2, 3]의 요소와 합쳐준다. 그런 행위를 할때마다 push



# Permutation

```javascript
function findPermutation(stringList, rValue) {
    const len = stringList.length;
    const startCount = 1;

    function makePermutation(stringList, count, rValue, prevValue) {
        const frontPaddingValue = prevValue || '';
        let pivotCount = count;
        return stringList.reduce( (listNumbers, value, idx) => {
            count = pivotCount;
            if(count === rValue) {
                listNumbers.push(frontPaddingValue+value);
                return listNumbers;
            }
            const nextNumberList = [...stringList];
            nextNumberList.splice(idx, 1);
            count = 1+len-nextNumberList.length;
            const result = makePermutation(nextNumberList, count, rValue, frontPaddingValue + value);
            listNumbers.push(...result);

            return listNumbers;
        }, []);
    }
    return makePermutation(stringList, startCount, rValue);
}
```
  findAllPermutation에 pivotCount를 지정하여 해당 Count일때의 집합을 저장.


# Combination

 ## 하나의 집합 내에서 조합
```javascript
const getCombinations = (arr, m) => {
    const combinations = [];
    const picked = [];
    const used = new Array(arr.length).fill(false);

    function find(picked) {
        if (picked.length === m) {
            const rst = [];
            for (let i of picked) {
                rst.push(arr[i]);
            }
            combinations.push(rst);
            return;
        } else {
            let start = picked.length ? picked[picked.length - 1] + 1 : 0;
            for (let i = start; i < arr.length; i++) {
                picked.push(i);
                used[i] = 1;
                find(picked);
                picked.pop();
                used[i] = 0;
            }
        }
    }
    find(picked);
    return combinations;
}
```


 하나의 집합 중 같은 값이 있을 경우
  ([1, 2, 2, 3] 과 같은 숫자에서 combination을 뽑고자 할떄)
```javascript
const getCombinations = (arr, m) => {
  arr.sort(); //같은 값이 연속으로 들어오도록 강제
  const combinations = [];
  const picked = [];
  const used = new Array(arr.length).fill(false);
  
  function find(picked) {
    if (picked.length === m) {
      const rst = [];
      for (let i of picked) {
        rst.push(arr[i]);
      }
      combinations.push(rst);
      return;
    } else {
      let start = picked.length ? picked[picked.length-1] + 1 : 0;
      for (let i = start; i < arr.length; i++) {
        if (i === 0 || arr[i] !== arr[i-1] || used[i-1]) {
          picked.push(i);
          used[i] = 1;
          find(picked);
          picked.pop();
          used[i] = 0;
        }
      }
    }
  }
  find(picked);
  return combinations;
}
```

 ## 두개의 집합간 조합
  ### 독립된 두개의 집합
```javascript
   function getCombination(arr_a, arr_b) {
    var answer = 0;
    const combination = [];
    arr_a.forEach( a => {
        arr_b.forEach( b => {
            combination.push([a, b]);
        })
    })
    return combination;
    } 
```

  ### 종속적인 두개의 집합
   아래의 문제해결은 하나의 배열안에 2개배열을 넣을 경우에 해당하고, 
   만일 특정 조건으로 2개 배열을 구성할수 있다면 기존 하나의 배열에서의 조합 문제 해결방식을 따른다.(조합문제_1 참고)
  ```javascript
   // parentSet = [0, 1, 2, 3, 4]
    var arraySet = [[0, 2], [1, 2], [3, 4]];
  ```
  ```javascript

function getCombination(arrays) {
    var answer = {};
    
    debugger;
    function makeCombination(i = 0, selected = []) {
        if (!arrays[i]) {
            selected.sort(); //당연히 조합이기 때문에 순서가 다른 같은 값은 중복처리해줘야 한다.
            answer[selected.join('')] = true;
            return;
        }
        arrays[i].filter(e => !selected.includes(e)).forEach(e => {
            makeCombination(i + 1, selected.concat([e]));
        });
    }
    
    makeCombination();
    return answer;
}
/** 
순서가 같은 다른 값 
 ex) arraySet = [[0, 2], [1, 2], [3, 4], [3, 4]]; 일 경우
      [0, 1, 4, 3] , [0, 1, 3, 4] 
 를 튜플로서 처리하자면 아래와 같다.
*/

  function getCombination(arrays) {
    var answer = [];
    
    function makeCombination(i = 0, selected = []) {
        if (!arrays[i]) {
            answer.push(selected);
            return;
        }
        arrays[i].filter(e => !selected.includes(e)).forEach(e => {
            makeCombination(i + 1, selected.concat([e]));
        });
    }
    
    makeCombination();
    return answer;
}
```