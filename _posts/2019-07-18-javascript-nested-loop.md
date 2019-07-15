---
layout: post
title:  자바스크립트 - 반복문의 진행과정
date: 2019-07-18
tags: javascript
authors: eojinlee
cover: /assets/post_image/20190628/test4.jpg
---

## Nested Loop(for문)의 진행순서

Codecademy 에서 nested loop for문을 공부하다가 궁금한 게 생겼다.
코드 예제를 짜다가 nested loop의 진행과정이 궁금해서 각 블록문마다 console.log를 넣어봤는데, log가 어떤 패턴으로 나오는건지 잘 이해가지 않는다.


```
const bobsFollowers = ["Ari", "Bunny", "Carol", "Dunkun"];
const tinasFollowers = ["Ari", "Bunny", "Conan"];
const mutualFollowers = [];
for(let i = 0; i < bobsFollowers.length; i++) {
  for(let j = 0; j < tinasFollowers.length; j++) {
    if(bobsFollowers[i] === tinasFollowers[j]) {
      mutualFollowers.push(bobsFollowers[i]);
      console.log("if: " + mutualFollowers);
    }
    console.log("for2: " + mutualFollowers);
  }
  console.log("for: " + mutualFollowers);
}

/*
if: Ari // if 1번째 반복
for2: Ari
for2: Ari
for2: Ari // for2 tinasFollowers.length( == 3 )까지 반복
for: Ari // for 1번째 반복
for2: Ari // ? 왜 if로 넘어가지 않고 for2에서 console.log로 넘어가지?
if: Ari,Bunny
for2: Ari,Bunny
for2: Ari,Bunny
for: Ari,Bunny
for2: Ari,Bunny
for2: Ari,Bunny
for2: Ari,Bunny
for: Ari,Bunny
for2: Ari,Bunny
for2: Ari,Bunny
for2: Ari,Bunny
for: Ari,Bunny
*/
```

## 실험해보기

그래서 각 console.log마다 `index` 값을 확인해야겠다고 생각했다. console.log에 `index`를 추가했다. `index`들은 블록문 안에 let으로 정의되어있으니까 각 블록문을 넘지는 못한다는걸 염두해서 각 블록문 조건에 쓰인 `index` 변수만 콘솔에 넣는다. 결과는 이렇다.

```
const bobsFollowers = ["Ari", "Bunny", "Carol", "Dunkun"];
const tinasFollowers = ["Ari", "Bunny", "Conan"];
const mutualFollowers = [];
for(let i = 0; i < bobsFollowers.length; i++) {
  for(let j = 0; j < tinasFollowers.length; j++) {
    if(bobsFollowers[i] === tinasFollowers[j]) {
      mutualFollowers.push(bobsFollowers[i]);
      console.log("if:   " + "i = " + i + ", j = " + j + ", " + mutualFollowers);
    }
    console.log("for2: " + "j = " + j + ", " +  + mutualFollowers);
  }
  console.log("for1: " + "i = " + i + ", " +  + mutualFollowers);
}

/*
if:   i = 0, j = 0, Ari // if 첫 번째 true
for2: j = 0, NaN // 전역변수 mutualFollowers가 참조됨. push된 mutualFollowers는 if 블록문을 넘지 못함.
for2: j = 1, NaN
for2: j = 2, NaN // for2에서 j = 0부터 tinasFollowers.length ( = 3) 만큼 반복
for1: i = 0, NaN // for2 반복 끝나고 for1 첫 번째 반복

// 여기까지가 1 페이즈. 페이즈는 i값의 변화를 기준으로 정했다.


for2: j = 0, NaN // j는 다시 0으로 초기화됨.
if:   i = 1, j = 1, Ari,Bunny // if 두 번째 true
for2: j = 1, NaN
for2: j = 2, NaN // for2에서 j = 1부터 tinasFollowers.length ( = 3) 만큼 반복
for1: i = 1, NaN // for2 반복 끝나고 for1 두 번째 반복

// 여기까지가 2 페이즈


for2: j = 0, NaN
for2: j = 1, NaN
for2: j = 2, NaN
for1: i = 2, NaN

// 여기까지가 3 페이즈


for2: j = 0, NaN
for2: j = 1, NaN
for2: j = 2, NaN
for1: i = 3, NaN

// 여기까지가 4 페이즈
*/
```

## 실험결과로 가설 추측하기 + 더 궁금한 점

**1. let으로 선언한 변수는 블록변수가 되어서 블록문을 넘지 못한다.** 

위의 예제에서는 각 `index` 값들이 var이 아닌 let으로 선언되어서 블록문을 못 넘는다. `index` 값 뿐만아니라 새로 생성되는 mutualFollowers도 push되는 if문 말고는 다른 for문에서 참조하지 못하는 것 같다. 

**2. 중첩된 블록문은 처음 실행될 때 가장 안쪽에 있는 블록문부터 실행된다.**

그래서 처음에 for1 > for2 > if 로 시작하지 않고 if부터 바로 들어간 것이다. 그 다음부터는 위에 쓴 순서대로 진행되는 것 같다.

**3. 중첩된 for문으로 행렬을 만들 수 있다.**

배열 속 배열(nested array)을 어디에 쓸 데가 있는지 몰랐는데, 이렇게 중첩된 반복문을 사용하면 가로세로 2차원 배열을 만들 수 있겠다고 생각했다. 음... 그럼 for문이 3번 중첩되면 3차원 배열도 만들 수 있나?

**4. 배열 요소가 null인 배열을 console.log로 불러오면 null > NaN 으로 불려온다.**

값이 없으니까 막연히 undefined일거라 추측했는데, 생각해보니 배열은 객체라서 null이 맞다. 그런데 왜 console.log로 불러왔을 때 null 이 아니라 NaN으로 불려왔을까?

