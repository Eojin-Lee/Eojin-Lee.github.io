---
layout: post
title:  알고리즘 문제풀이 - 완주하지 못한 선수
date: 2019-07-15
tags: javascript programmers algorithm
authors: eojinlee
cover: /assets/post_image/20190628/test4.jpg
---


## [완주하지 못한 선수](https://programmers.co.kr/learn/courses/30/lessons/42576)

총 풀이 시간: (개념 파악한 이후) 1hr 40min

---------------------

### 문제설명

마라톤에 참여한 선수들의 이름이 담긴 배열 participant, 완주한 선수들의 이름이 담긴 배열 completion. 두 배열을 비교해서 완주하지 못한 선수 단 1명의 이름을 return하도록 Solution 함수를 작성해주세요.

-------------------


### 문제풀이과정

**1. participant 와 completion 의 element 값은 따로 주어지지 않았으므로, function parameter 안에만 넣고 따로 정의내리지 않아도 된다. (undefined 처리)**
```
function solution(participant, completion)
```

**2. participant 에서 element 1개가 빠지면 completion 이다.**
```
completion.length = participant.length - 1;
```

**3. 두 arr를 순차적으로 비교하고 participant 에서 빠진 값 1개만 구하는 것이므로, 똑같이 나열된 두 arr에서 i번째 element가 다르면 그 element를 return 하게 하면 된다.**
```
if((participant[i] !== completion[i])) {
	answer = participant[i];
}
```
> 왜 if문 parameter bracket()을 두 개나(()) 씌우지?
*[**if-Assignment within the conditional expression** | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else#Assignment_within_the_conditional_expression)*

**4. 두 arr 비교는 원하는 값이 나올때 까지만 한다. 원하는 값이 안나오면 마지막 값이 정답일 것. 이 정답은 default로 미리 정해준다.**
```
let answer = participant[participant.length - 1];
for(let i == 0; i < participant.length; i++) {
    if((participant[i] !== completion[i])) {
	  answer = participant[i];
	  break;
}
 return answer;
}
```
> 왜 answer = participant[participant.length]가 아닌거야?
*[**Array-Accessing array elements** | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Accessing_array_elements)*

> for(i == 0, ...)도 맞는 표현 아니야? 왜 i에 정의키워드를 붙이지? 그리고 그게 왜 var가 아니라 let이야?
*[**for-Syntax** | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for#Syntax)*

> break는 언제, 왜 걸어야 하는거야?
*[**break-Description** | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/break)*

**5. 아 맞다! 배열을 비교하기 전에 두 배열을 정렬해야한다. 이것도 default로 놓아야 한다.**
```
let answer = participant[participant.length - 1];
participant.sort();
completion.sort();
```

**6. 최종 1차 코드**
```
function solution(participant, completion) {

    let answer = participant[participant.length - 1];

    participant.sort();
    completion.sort();

    for(let i == 0, i < participant.length, i++) {
	if((participant[i] !== completion[i])) {
	  answer = participant[i];
	  break;
         }
         return answer;
    }
}
```

**7. 완벽해보였지만 타입 에러를 내며 작동하지 않는다.** 

다시보니 쓰지않은 코드들, 잘못 쓴 연산자, 신택스 에러들이 보인다. 오타내지않게 집중하자. (일러스트레이터로 처음 벡터 도형을 그릴 때 이리저리 삽질하던 경험과 비슷한 느낌이다.) 잘 모르는 것들부터 고쳐본다. if param bracket을 다시 1개로 줄이고, participant.length 최대값을 default로 정한다. 필요없는 코드를 뺀다. for문 param을 다시 쓴다. return을 제대로 고쳐쓴다. let answer 를 sort 이후에 배치한다. 최종 2차 코드.
```
function solution(participant, completion) {

    participant.length <= 100000;
    completion.length == participant.length - 1;

    participant.sort();
    completion.sort();

    let answer = participant[participant.length - 1];

    for(let i == 0, i < participant.length, i++) {
	if((participant[i] !== completion[i])) {
	  answer = participant[i];
          return answer;
	  break;
         } else {
         return answer;
         break;
         }
    }
}
```
> function for if 를 조합하는 syntax 예시
*[**Loops and iteration-Example** | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration#Example)*

> return은 어디에 적는거야?
*[**return-Description** | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/return#Description)*

**8. 최종 3차 코드. let answer를 sort 뒤에 배치하니 2개의 예제조건을 통과했다!** 

하지만 마지막 조건, 동명이인을 풀지 못했다. 아마 else문이나 끝부분에 문제가 있는 것 같다. else를 지우고 return을 함수 끝에 넣었다.
```
function solution(participant, completion) {

    participant.length <= 100000;
    completion.length == participant.length - 1;

    participant.sort();
    completion.sort();

    let answer = participant[participant.length - 1];

    for(let i == 0, i < participant.length, i++) {
	if((participant[i] !== completion[i])) {
	  answer = participant[i];
          return answer;
	  break;
         } 
    } 
    return answer;
}
```

**9. 최종 3차코드로 성공!!** 

syntax 때문에 시간을 많이 허비했다는 생각이 든다... syntax 진행 순서같은 원리를 더 알고싶다... 헷갈리지 않게ㅠㅠ. 다른사람들 풀이를 보니까 hash를 쓰거나, 완전 처음보는 개념들로 풀어냈다. 내가 정확성 테스트와 효율성 테스트에 간신히 통과한 이유가 역시 hash를 안 써서 그런가보다. hash 개념을 배운 뒤 다시 풀어보면 여기 올라온 답 만큼 다양하게 풀 수 있을 것 같다. participant 등의 변수들을 다시 변수처리해서 깔끔하게 정리한 코드도 보인다. 이건 지금 고쳐볼 수 있겠다.
```
 function solution(participant, completion) {

 let p = participant.sort();
 let c = completion.sort();
 let answer = p[p.length - 1];

 for(let i = 0; i < p.length; i++) {
	if(p[i] != c[i]) {
	  answer = p[i];
	  break;
	  }
	} 
 	return answer;
}
```

-------------


### 후기

해결 방법과 키워드를 알고있는데도 신택스 틀려서 삽질한 건 수학풀이때 개념 다 알고도 계산 잘못해서 틀리는 것과 매한가지다. 내가 그래서 수학을 포기했던건데... 이젠 도망칠 수도 없다. 한 번 코드 짤 때 집중해서 올바르게 짜는 수밖에.

-----------



