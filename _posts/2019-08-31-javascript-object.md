---
layout: post
title:  [자바스크립트] 객체
date: 2019-08-31
tags: javascript
authors: eojinlee
---


자바스크립트의 객체(object)는 마치 컨테이너 박스처럼 한꺼번에 여러 값을 담을 수 있는 데이터 타입이다.숫자, 문자열, 불리언 등 다른 원시타입(Primitive data type)과 달리 객체는 객체 타입(Object data type)이라는 데이터타입으로 따로 분류된다.

객체는 데이터값을 지닌 **프로퍼티**와 액션을 수행할 수 있는 **메소드**를 모두 한 컨테이너에 담을 수 있기 때문에 여러 데이터와 액션들을 하나의 단위로 구조화해서 변수나 모듈처럼 사용할 수 있다.

### 객체 리터럴 표기법

자바스크립트에서 객체 리터럴은 기본적으로 아래와 같이 표현된다.파라미터 없이 바로 선언하기, 끝에 세미콜론(;) 붙이기는 함수보다 변수 정의에 가깝다.

```javascript
// 객체 리터럴 표기법
const object1 = {
    'Key Name': 'Value1',
    keyNum: 1,
    keyArray: ['Happy', 'Sad', 'Angry'],
    keyfunc() {
        console.log('Hello!');
    }
};

// 객체 내 프로퍼티는 key: value 로 이루어진다.
// 메소드는 객체 내 함수로, 프로퍼티 중 하나다.
```

### 키+밸류 = 프로퍼티

**키(key, name)**와 **밸류(value)**는 변수에 데이터를 선언하는 역할로, 이 모음을 통틀어서 **프로퍼티(property)**라 부른다.하나의 객체에 여러 개 프로퍼티가 들어갈 수 있다.

- **키**는 문자열 또는 변수와 같은 식별자(no 문자열)로 쓸 수 있다.자바스크립트는 키를 문자열로 인식하기 때문에, 공백처럼 식별자로 쓸 수 없는 문자가 포함되어있다면 문자열로만 써야한다.
- **밸류**는 키와 달리 모든 데이터타입을 쓸 수 있다.

### 객체 내 함수 = 메소드

객체 안의 함수 데이터를 메소드라고 부른다.프로퍼티는 객체 안의 데이터가 무엇인지, 메소드는 객체 안의 데이터가 무엇을 실행하는지를 뜻한다. `console.log()`와 같은 글로벌 자바스크립트 객체.메소드가 대표적인 예시다.

-------

## 객체 프로퍼티 접근하기

객체의 프로퍼티에 접근하려면 `'hello'.length`처럼 **점 연산자 .** 를 쓰거나 `object1['keyNum']`처럼 **대괄호 연산자 []** 를 쓴다.

- 점 연산자는 키가 식별자일 경우 쓴다.
- 대괄호 연산자는 키가 문자열일 경우 쓴다.다만 자바스크립트는 모든 키를 문자열로 인식하므로 대괄호 연산자 안의 키가 식별자로 쓰였더라도 '문자열'로 적어야한다.

객체 프로퍼티는 객체 밖에서 재선언할 수 있다.

```javascript
// 객체 프로퍼티를 객체 밖 함수에서 접근하기
const object2 = {
    'Key Name': 'Hanna',
    keyNum: 2,
    keyArray: ['Happy', 'Sad', 'Angry'] 
};

let status = (howRU, feeling) => {
    howRU = object2.keyArray;
    feeling = object2.keyNum;
    console.log(`${object2['Key Name']}: ${howRU[feeling]}`);
}
status(); // Hanna: Angry

// 객체 프로퍼티 재선언하기
object2['Key Name'] = 'Namyoon';
object2.keyNum = 0;
status(); // Namyoon: Happy
```

### this.키워드

객체 안의 메소드에서 다른 객체 프로퍼티에 접근할 때는 **[this.키워드](https://poiemaweb.com/js-this)**를 쓴다.이 키워드를 쓰지 않고 바로 키값을 불러오면 객체가 아닌 함수 내 변수로 인식되기 때문에 레퍼런스 에러가 난다.

```javascript
// 객체 프로퍼티를 객체 내 메소드에서 접근하기
const object3 = {
    id: ['Yura', 'Haul'],
    applaud(dm) {
        dm = 'Your work is soooo nice!!! :D';
        console.log(`${this.id[0]}, ${dm}`);
    console.log(`${id}, ${dm}`); // Uncaught ReferenceError: id is not defined
    },
    question(dm) {
        dm = 'I wonder how you did it! ;)';
        console.log(`${this.id[1]}, ${dm}`);
    }
};
object3.applaud(); // Yura, Your work is soooo nice!!! :D
object3.question(); // Haul, I wonder how you did it! ;)
```

Java에서 `this`는 자기 자신을 가리키는 참조변수이지만, 자바스크립트의 `this`는 호출 방식에 따라 불러오는 객체가 달라진다.예외로 메소드에서 호출되는 `this` 에서만 메소드가 소속된 객체를 나타낸다.객체의 메소드인 경우를 제외하고 일반 함수, 콜백함수, 내부함수에서의 `this` 모두 메소드가 속한 객체가 아닌, 일반 함수와 동일하게 기본적으로 전역 객체(window)를 가리킨다.

메소드 내부에서도 `this`를 쓰지 못 할 때가 있다.ES6의 화살표함수(arrow function)가 메소드로 쓰일 때는 `this` 가 작동하지 않는다.

```javascript
// 일반함수에서 this는 전역객체(window)를 가리킨다.
function funcNormal() {
    console.log(this);
}
const funcArrow = () => {
    console.log(this);
}
funcNormal(); // window
funcArrow(); // window


// 내부함수, 콜백함수에서 this는 전역객체(window)를 가리킨다.
let num = 1;

const newObj = {
    num: 10,
    func2() {
        console.log(this.num); // 10
        func3() {
            console.log(this.num); // 1
        }
        func3();
    }
};
newObj.func2();
```

-------

### 내부 객체

객체 내부의 프로퍼티로 객체를 쓸 수 있다.(nested objects) 객체의 내부 객체 프로퍼티에 접근하려면 연산자를 이어붙여서 표현하면 된다.

```javascript
// 객체 내 객체 프로퍼티에 접근하기
const object4 = {
    missions: null,
    team: {
        leader: {
            name: 'Jinu',
            degree: 'Visual Design',
            'favorite foods': ['maratang', 'cake', 'coffee']
        },
        partner: {
            name: 'Seoyoung',
            degree: 'Computer Engineering'
        }
    }
};
const leaderFave = object4.team.leader['favorite foods'];

// 배열 형태로 객체 프로퍼티 추가하기
object4.missions = [{
    platform: 'car display',
    target: 'senior women'
}, {
    platform: 'mobile AR',
    target: 'teenage'
}];
const firstMission = object4.missions[0]; 
```

## 객체타입의 참조방식

원시타입이 메모리에 저장된 데이터값 자체를 참조한다면, 객체타입은 메모리에 저장된 데이터값을 '가리키는'(pointing) 화살표처럼 데이터값의 위치를 참조한다.

```javascript
// 원시타입은 데이터값 자체를 참조한다.
let num1 = null; // 빈값이 할당된다.
let num2 = num1; // num2가 참조하는 '빈값'이 할당된다.
num2 = 100;
console.log(num1, num2); // null 100

// 객체타입은 데이터값의 위치를 참조한다.
let str1 = {}; // 빈 객체가 할당된다.
let str2 = str1; // 'str1'이 참조하는 빈 객체가 할당된다.
str2.value = 'hi!';
console.log(str1, str2); // { value: 'hi!' } { value: 'hi!' }

// 객체타입에 서로 다른 데이터값 위치를 참조하도록 할 수 있다.
let str3 = {}; // 빈 객체가 할당된다.
let str4 = {}; // str3과 '다른' 빈 객체가 할당된다.
str4.value = 'hello!'
console.log(str3, str4); // {} { value: 'hello!' }
```

객체 밖 함수 안에서 객체를 참조할 때는 일반 변수처럼 가상의 파라미터를 두면 글로벌 영역의 객체를 참조할 수 있다.

```javascript
let object6 = {
    num: 10,
    'Our Message': 'Welcome!'
};

const changeValue = (obj) => {
    obj.num = 12;
    obj['Our Message'] = 'We made it!';
}
changeValue(object6);
console.log(object6); // { num: 12, Our Message: 'We made it!' }
```

## 객체와 반복문 for...in

for...in 반복문으로 객체 내 프로퍼티를 반복해서 불러올 수 있다.다른 반복문처럼 for문 안에 파라미터로 쓸 변수를 선언하고, 대괄호로 감싸서 사용하면 된다.

```javascript
const object7 = {
    team: {
        leader: {
            name: 'Jinu',
            degree: 'Visual Design',
        },
        crew1: {
            name: 'Seoyoung',
            degree: 'Computer Engineering'
        },
        crew2: {
            name: 'Lay',
            degree: 'Physics'
        }
    }
};

for (let teamCrew in object7.team) {
    console.log(`${object7.team[teamCrew].name}: ${object7.team[teamCrew].degree}`);
};
// 변수 teamCrew는 key가 아닌 index 역할이기 때문에 점 연산자를 쓰지 않고, 문자열로 표기하지 않는다.
```
