# Typescript Quick Start

## 05 _ 연산자

### 5.1. 기본 연산자

#### 5.1.1. 산술 연산자

타입스크립트는 자바스크립트와 동일한 산술 연산자(arithmetic operator)를 지원
이에 더해 타입스크립트는 ES7의 지수 연산자(exponentiation operator)인 **를 지원 Math.pow를 대체해 사용할수 있다.
```typescript
// 산술 연산자 --- #1
console.log(10 ** 3); // 1000

=> ES6로 컴파일시 Math.pow(10, 3)

console.log(1 + false) // Error
console.log(false + false) // Error
```

타입스크립트에서는 숫자 값과 불리언 값은 연산할 수 없다.
자바스크립트 처럼 false값을 0으로 인식하지 않는다.

```typescript
console.log(10 / "2"); // Error
// 타입스크립트는 문자열 타입을 연산에 허용하지 않음 
```


#### 5.1.2. 비교, 논리, 조건 연산자

- 비교 연산자

- 논리 연산자

- 조건 연산자

| 연산자 기호 | 연산 예 | 의미                                                         |
| :---------: | ------- | ------------------------------------------------------------ |
|     ==      | a == b  | a와 b의 값이 같은지 비교 <br/>1 == true // true              |
|     ===     | a === b | a와 b의 값과 타입이 모두 같은지 비교 <br/>1 === true // false |
|     !=      | a != b  | a와 b의 값이 다른지 비교 <br/> 1 != true // false            |
|     !==     | a !== b | a와 b의 값과 타입이 다른지 비교<br />1 !== true // true      |
|      >      | a > b   |                                                              |
|      <      | a < b   |                                                              |
|     >=      | a >= b  |                                                              |
|     <=      | a <=    |                                                              |

타입의 안전성을 보장하기 위해서
== 대신 ===을 != 대신 !==을 사용하기를 권장


| 연산자 기호 | 연산 예 | 의미 |
| :---------: | ------- | ---- |
|     &&      |         | and  |
|    \|\|     |         | or   |
|      !      |         | not  |

삼항 연산자(tenary operator)

```typescript
[형식]
판별 조건 ? 표현식1 : 표현식2
```

##### 불리언 타입과 부정 연산자

0을 제외한 숫자나 문자열이 불리언 타입으로 바뀔때 true로 변환

```typescript
console.log(!"hello", !!"hello"); // false, true
console.log(!0, !!0, !1); // true, fasle, false
```


### 5.2. 디스트럭처링(destructuring)
ES6 디스트럭처링 지원

`de + structure` 객체의 구조(structure)를 제거(de)한다는 의미
객체의 구조를 분해 후 할당이나 확장과 같은 연산을 수행

- 객체 디스트럭처링

- 배열 디스트럭처링


#### 5.2.1. 객체 디스트럭처링

##### 객체 디스트럭처링 기본

```typescript
let {id, country} = {id: "happy", country: 88};

let {id, country = 88} = {id: "happy"};
```

객체의 속성값을 변수에 할당하는것 `디스트럭처링 할당(destructureing assignment)`

```typescript
let {id: newName1, country: newName2} = {id: "happy", country: 88};

console.log(newName1); // happy
console.log(newName2); // 88
```

`속성 재명명 (property renaming)` 할당 시 객체 속성에 새로운 이름 을 부여해 할당


##### 디스트럭처링 매개변수 선언

```typescript
function printProfile(obj){
    let name = "";
    let nationality = "";

    name = (obj.name == undefined) ? "anonymous" : obj.name;
    nationality = (obj.nationality == undefinded) ? "?" : obj.nationality;

    console.log(name); // happy
    console.log(nationality); // ?
}

printProfile({name:  "happy"});

// 위와 동일한 코드
function printProfile({name, nationality = "?"}){

    console.log(name); // happy
    console.log(nationality); // ?

}

printProfile({name: "happy"});
```

객체 전달을 생략하려면 우항에 할당식을 추가
```typescript
function printProfile({name, nationality = "none"} = {name: "anonymous"}){
    console.log(name, nationality);
}

printProfile(); // anonymous none
printProfile({name: "happy"}); // happy none
printProfile({name: "happy", nationality: "korea"}); // happy korea
```

##### 디스트럭처링 시 type 키워드 활용

```typescript
[형식]
type C = { a: string, b?: number } // 선택 연산자인 ?로 선언했으므로 생략가능

// 디스트럭처링 매개변수의 타입으로 활용

function fruit({a, b}: C): void {
    console.log(a, b);
}

fruit({a: "apple", b: 10}); // apple 10
fruit({a: "apple"}); // apple undefined
```


#### 5.2.2. 배열 디스트럭처링

```typescript
let numbers = ["one", "two", "three"];

let num1 = numbers[0]; // 인덱스를 이용해 얻은 값을 각 변수에 할당

let [num1, num2] = numbers; // 배열 디스트럭처링으로 간결하게 할당
let [,,num3, num4] = numbers; // 원하는 위치에 있는 요소만 변수로 할당

[num4, num3] = [num3, num4]; // num3, num4 변숫값을 서로 교환하고 싶다면 다음처럼 손쉽게 해결

let [color1, color2 = "blue"] = ["black"]; // 기본값 지정 가능
console.log(color1, color2); // black blue
```


##### 배열 요소를 함수의 디스트럭처링 매개변수로 전달

```typescript
function f([first, second]: [number, string]) { ... }
             ^       ^  배열의 요소를 전달함
function f([100, "hello"])

function f([first, second]: [number, string]) {
    console.log(first);
    console.log(second);
}
f([100, "hello"]); // 100 hello
```

### 5.3. 전개 연산자(spread operator)

- 나머지 매개변수를 선언할 때

- 배열 요소를 확장할 때

- 객체 요소를 확장할 때

#### 5.3.1. 전개 연산자를 이용한 배열 요소 확장

1. 배열 합치기
```typescript
let arr: number[] = [3,4,5];

let arr2: number[] = [1,2, ...arr];

console.log(arr2); // [1,2,3,4,5]
```

2. 배열 디스트럭처링
```typescript
let [first, ...rest] = [1,2,3];

console.log(first); // 1
console.log(rest); // [2,3]
console.log(rest[0]); // 2
```


#### 5.3.2. 전개 연산자를 이용한 객체 요소 확장

1. 객체 합치기
```typescript
let obj = {c: 3, d: 4, e: 5};

let obj2 = {a: 1, b: 2, ...obj};

console.log(obj2) // {a:1, b:2, c:3, d:4, e:5}
```
전개 연산자는 얕은 복사(shallow copy) 방식으로 값을 obj2 객체로 복사

2. 객체 디스트럭처링
```typescript
let numGroup = {n1: 1, n2: 2, n3: 3};

let {n2, ...n13} = numGroup;

console.log(n2, n13); // 2 {n1: 1, n3: 3};
```

