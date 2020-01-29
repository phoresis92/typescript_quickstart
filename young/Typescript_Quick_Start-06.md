# Typescript Quick Start

## 06 _ 함수

---

### 6.1. 타입스크립트 함수 사용

#### 6.1.1. 자바스크립트 함수

##### 기명 함수와 익명 함수의 선언

1. named function

    function myFunc(a,b){ ... }
    시그니처 함수명 매개변수

- 기명 함수는 호출될 때 호이스팅이 발생

2. anonymous function

    let resultMultiplication = function(a,b) { return a * b};

- 익명 함수를 할당한 뒤에만 함수를 호출 할 수 있도록 호출 시점을 제한


##### 자바스크립트의 함수의 불안전성

- 자바스크립트 => 느슨한 타입 언어(loosely typed language)
- 프로그램이 실행될 때 동적으로 타입을 할당해 추론된 타입(inferred type)이 지정
- 타입이 없기 때문에 런타임 때 의도하지 않은 타입 변환이 일어날 수 있다.
- 위와 같은 문제를 방지하기 위해 불필요한 매개변수의 검증이나 타입 캐스팅을 수행

1. 함수의 매개변수에 대한 타입 검증(타입이 일치하지 않으면 타입 캐스팅)

2. 함수의 연산을 수행한 후에 반환값에 대한 타입 캐스팅

3. 함수 호출 결과에 대한 타입 캐스팅

#### 6.1.2. 타입 안전성을 갖춘 타입스크립트 함수

```typescript
function max(x: number, y: number):number{
    if(x > y){
        return x;
    } else {
        return y;
    }
}

let a = max(1,2);
// let b = max("abc", "ABC");

console.log(`a=${a}`); // a=2
```

---

### 6.2. 매개변수의 활용

#### 6.2.1. 기본 초기화 매개변수

ES6에서 추가된 기본 초기화 매개변수(default-initialized parameter)
매개변수에 인수가 전달되지 않으면 매개변수에 설정된 초깃값으로 값을 초기화

```typescript
function pow( x: number, y: number = 2): number
          ^                     ^          ^
       (함수명)   (기본 초기화 매개변수) (함수의 반환 타입)

function pow( x: number, y: number = 2): number{
    return x ** n;
}

console.log(pow(10)); // 100
console.log(pow(10, 3)); // 1000
```


#### 6.2.2. 나머지 매개변수(rest parameter)

ES6에서 제안된 특징 => 개수가 정해지지 않은 인수를 배열로 받을 수 있는 기능

함수에서 첫 번째, 두 번째 매개변수의 순서는 중요하고,
세 번째 이후에 있는 매개변수는 순서가 중요하지 않으며, 
0개 이상의 매개변수를 전달하고 싶다
```typescript
function concat(a, b, ...restParameter){
    return a+b+restParameter.join("");
}

// 최소 (a,b) 2개 이상의 인수를 전달
// 3개 이상의 인수를 전달하면 ...restParameter가 배열로 받음
```

```typescript
function concat(...restParameter: string[]){ ... }

// 나머지 매개변수 타입을 지정

function color(a: string, ...rest: string[]){
    return a + " " + rest.join(" ");
}

// colors();
let color1 = colors("red"); // red
let color2 = colors("red", "orange"); // red orange

function colors(a){
    var rest = [];
    for (var _i = 1; _i < arguments.length; i++){
        rest[_i - 1] = arguments[_i];
    }

    return a + " " + rest.join(" ");
}
// arguments 배열 형태로 전달받은 인수를 저장, length 프로퍼티를 제공
// but 배열은 아니다.
// 본래 배열 객체에서 포함하고 있어야 할 pop(), push()와 같은 메서드 없다
```

#### 6.2.3. 선택 매개변수(optional parameter)

전달 인수의 개수를 0개 이상 1개 미만으로 제한하려면 '선택 매개변수'를 이용

```typescript
[형식]
function sum(a: number, b?: number):number {
    return a + b;
}

console.log(sum(1)); // NaN
```

- **선택 매개변수와 초깃값 설정은 함께 사용할 수 없음**
```typescript
function sum(a: number, b?: number = 2):number { ... }
                               ^
                            Error

function sum2(a: number, b?: number){
    if(b === undefined){
        b = 0;
    }
    return a + b;
}

console.log(sum2(1)); // 1
```

#### 6.2.4. 함수 오버로드(function overloads)

매개변수와 반환 타입이 다른 함수를 여러개 선언할 수 있는 특징
컴파일 시간에 가장 적합한 오버로드를 선택해 컴파일하므로 자바스크립트 실행시에는 런타임 비용이 발생하지 않는다

- **가장 일반적인(general) 함수(매개변수를 any 타입으로 선언)의 시그니처를 가장 아래에 선언**
- **그 위로 구체적인(specific) 타입을 명시한 함수의 시그니처를 쌓는 방식으로 선언한다!**
```typescript
function add(a: number): number;
function add(a: number, c: number): number;
function add(a: any, b?: any): any {
    if(b === undefined){
        return a;
    } else {
        return a + b;
    }
}

console.log(add(1,2)); // 3
console.log(add(1)); // 1
```

- **다음과 같이 독립된 블록으로 선언하면 안된다!**
- **함수의 바디는 하나만 선언되어야 한다!**
```typescript
function add(a: string, b: string):string { ... }
function add(a: number, b: number): number { ... }
function add(a: any, b:any): any { ... }
```

Example
```typescript
let constellations = [
{name: "Capricorn", month: 2},{name: "Aquarius", month: 2},{name: "Pisces", month: 3}
]

// function pick(x: number | {month: number} | {name: string}): string | number;
// 유니언 타입 형태로 표현 가능
function pick(x: {name: string}): number;
function pick(x: {month: number}): string;
function pick(x: number): number;
function pick(x: any): any {
    if(typeof x === "object"){
        ...
    } else if (typeof x === "number"){
        ...
    }
}

console.log(pick({month: 1})); // Capricorn
console.log(pick({name: "Auarius"})); // 2
console.log(constellations[pick(3)]); // {name: "Pisces", month: 3}
```

---

### 6.3. 익명 함수의 이해와 활용

#### 6.3.1. 익명 함수와 화살표 함수

화살표 함수(arrow function) => ES6 익명 함수를 좀 더 간략하게 표현할 수 있는 방법
람다 함수(lambda function)

```typescript
      ()            =>           {};
매개변수 목록, 뚱뚱한 화살표, 함수 블록
```
'->' 에 비해 뚱뚱한 모양이어서 '뚱뚱한 화살표(fat arrow)'라는 별명이 있다

```typescript
let y3 = x => x;
=
let y3 = (x)=>{return x;};

// 위와 같이 매개변수가 1개일 때 소괄호를 사용하는 선언 형태는 타입스크립트 스타일 가이드에서 추천하지 않는 스타일 이므로 (x) 대신 소괄호를 생략한 x 형태로 사용할 것
```

```typescript
let z1 = (x, y)=> {return x + y;};
let z2 = ((x, y)=> x + y);

// 중괄호가 있을 때는 반드시 return 값을 포함해야 반환값을 얻을 수 있다.
```

```typescript
(x => { x; })(3);

// 변수를 선언하지 않고 즉시 호출
// 즉시호출 함수 (IIF: Immediately Invoked Function)
```

##### 필터 메서드 적용(filter method)

```typescript
let numberList = [1,2,3,4,5];
numberList = numberList.filter(n => {
    return n % 2 === 0;
});

console.log(numberList); // [2, 4]
```

##### 리듀스 메서드 적용(reduce method)

```typescript
[1,2,3,4,5].reduce((a,b)=>{ return a + b; })
     ^              ^ ^
순회할 배열    누적값 카운터 값
```

| 호출 순서 | 이전 누적값(a) | 현재 값(b) | 반환값 |
| :-------: | -------------- | ---------- | ------ |
|   1번째   | 1              | 2          | 3      |
|   2번째   | 3              | 3          | 6      |
|   3번째   | 6              | 4          | 10     |
|   4번째   | 10             | 5          | 15     |

```typescript
function getSum(nums: number[]): number {
    let sum: number = nums.reduce((a, b)=>{ return a + b; });
    return sum;
}

let numSum = getSum([1,2,3,4,5]);
console.log(`numSum=${numSum}`); // numSum=15
```

##### 객체 리터럴의 선언과 객체 리터럴 타입의 선언

