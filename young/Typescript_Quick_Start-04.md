# Typescript Quick Start

## 04 _ 제어문

### 4.1. 조건문의 타입 제약과 폴스루

#### 4.1.1. if 문 사용 시 타입 제약

```typescript
let text: string = "";
let statusActive: number = 0;
let isEnabled: boolean = true;

// string '' === false
// number 0 === false
if(statusActive || text){
    console.log(1);
}

if(isEnabled && 2 > 1){
    console.log(2);
}
```



#### 4.1.2. switch 문과 폴스루

```typescript
let command: number = 0;
// 사용할 타입을 지정하여 타입체크할 필요가 없어짐

switch (command){
    case 0:
        // do something
        break;
    case 1:
        // do something
        break;
}

let command2: any = 0;
// any 타입을 지정하여 유연한 대응 또한 가능하다

switch (command2){
    case "hi":
        // do something
        break;
    case 0:
        // do something
        break;
}
```



##### 폴스루에 대한 이해와 폴스루의 사용 여부 설정

break 문을 생략해 다음 case 절이 실행되는 상태를 `폴스루(fall-through)`라고 한다.

```typescript
let input = 2;

switch (input % 2){
    case 0:
        console.log(0); // fall-through 발생
    case 1:
        console.log(1);
        break;
}
```



```typescript
ts-config.json

{
    "compilerOptions": {
        "noFallthroughCasesInSwitch": true // default: false = fall-through 허용
    }
}
```



```typescript
noFallthroughCasesInSwitch: true 설정시에도 case 절이 비었다면 fall-through 허용

let input2 = 0;

switch (input2){
    case 0: // 명령문이 없으므로 fall-through 허용
    case 1:
    	console.log(1);
    	break;
}
```



### 4.2. 반복문의 다양한 사용법

#### 4.2.1. for 문과 이터러블 객체

`이터러블 (iterable)`은 반복 가능한 객체인 배열, 문자열, DOM 컬렉션, 맵, 셋 등을 말한다.



##### 기본적인 for문

```typescript
// ES5
for (var i = 0 ; i < 2 ; i++){ }
console.log(i); // var -> function scope

// Typescript
for (let j: number = 0 ; j < 2 ; j++){ }
// console.log(j) // Error // let -> block scope

let 선언자로 블록 레벨 스코프를 적용, number 타입을 추가해 타입 안전성을 확보
```



##### ES5의 for in 문

```javascript
var islands = ["jeju", "jindo", "namhaedo"];

for (var index in islands){
    console.log(index, islands[index]);
}

var fruits = {"a": "apple", "b": "banana"};

for (var property in fruits){
    console.log(property, fruits[property]);
}
```



##### ES6의 for of 문

```typescript
1.5 부터 지원

for(const value of [1,2,3]){ // <- const 선언자를 사용했다!!!
    console.log(value);
}

일반 for문(for in, for i ; i <10 ; i++)에서 const 선언자 사용 불가 -> 상수가 되어 증가 값이 바뀌지 않기 때문에 무한 루프가 된다!
for of 문은 Symbol.iterator의 구현을 통해 각 이터레이션 값의 요소글 가져오기 때문에 const 사용 가능
```



##### 맵과 셋을 for of 문에 적용

- 맵과 셋은` tsconfig.json` 파일의 `target 속성이 ES5` 이면 컴파일 되지 않는다. -> ES5에 대응할 문법이 존재하지 않음
- 맵과 셋을 이용하는 for of 문을 정상적으로 실행하려면 tsconfig.json 파일의 target을 es2015로 수정

```json
{
    "compilerOptions": {
        "target": "es2015"
    }
}
```



```typescript
let itMap = new Map([["one", 1], ["one", 2]]);
itMap.set("one", 3);

for (let entry of itMap) {
    console.log(entry); // [ 'one', 3 ]
}

console.log(itMap.get("one")); // 3
```



- 맵 객체는 ES6 부터 사용할 수 있그 때문에 ES5 에서 동작하는 맵을 구현하려면 별도로 맵을 구현해야함
- 객체 리터럴은 타입 지정이 가능하므로 맵에 사용할 키와 값에 대한 타입을 추가해 타입 안전성을 확보

```typescript
let map: { [key: string]: number; } = {};
map["one"] = 1;
map["one"] = 2;
map["one"] = 3;

for (let entry in map){
    console.log(entry); // one
}

console.log(map["one"]); // 3
```



```typescript
let itSet = new Set(["a", "b", "a"]);

for(let value of itSet){
    console.log(value); // a b
}

```



#####  \[Symbol.iterator\]() 메서드를 이용한 이터러블 객체 사용

\[Symbol.iterator\]() ES6에 추가된 특징 배열, 맵, 셋과 같은 이터러블 객체 (iterable object)를 순회하는 데 사용한다.

배열, 맵, 셋은 이미 \[Symbol.iterator\]() 메서드를 구현하고 있으므로 for of 문이나 for in 문에서 사용할 수 있다.

```typescript
let arr = [1,2];
let itObj = arr[Symbol.iterator]();

console.log(typeof itObj);
console.log(itObj.next()); // { value: 1, done: false }
console.log(itObj.next()); // { value: 2, done: false }
console.log(itObj.next()); // { value: undefined, done: true }
```



#### 4.2.2. while 문과 do-while 문

##### while 문

```typescript
let count: number = 1;
let sum: number = 0;

while (count <= 100) {
    sum += count;
    count++;
}

console.log(sum);
```



##### do-while 문

조건에 상관없이 do 블록에 위치한 명령문을 최소한 1번 이상 수행

```typescript
let cnt: number = 0;

do{
    console.log(cnt);
    cnt++;
} while (cnt != 4);
```

