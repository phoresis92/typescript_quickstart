# 연산자

----

##  :female_detective: 기본 연산자

###  :dizzy: 산술 연산자 ( arithmetic operator)

-  javascript 지원
- typescript 지원

> 산술 연산자 종류

1. 사칙연산
2. 나머지 연산 (%)

:rotating_light: **더하기 연산자 예외상황**

```javascript
1 + false
```

- javascript  : false를 0으로 인식하여 정답 == 1
- typescript 타입 오류

:heavy_exclamation_mark: **타입스크립트는 엄격하게 타입을 검사함 **

문자열 타입의 숫자를 연산식에 사용할 수 가 없음.



### :dizzy: 비교, 논리, 조건 연산자

<조건문 사용하는 연산자>

1. 비교 연산자
2. 논리 연산자
3. 조건 연산자

**비교 연산자**

| 연산자 기호 | 의미                             |
| :---------- | :------------------------------- |
| ==          | 값비교(속도 빠름)                |
| ===         | 값과 타입 비교(속도 비교적 느림) |
| !=          | 값이 다른지 비교                 |
| !==         | 값과 타입이 다른지 비교          |
| >           | a가 b보다 큰지                   |
| <           | a가 b보다 작은지                 |
| >=          | a가 b보다 크거나 같은지          |
| <=          | a가 b보다 작거나 같은지          |



**논리 연산자**

- &&
- ||
- !

논리연산자는 피연사끼리 타입이 일치하지 않아도됨.



**조건 연산자**

(조건식)?(참일 시 값):(거짓일때 값)  => 삼항 연산자



### :dizzy:불리언 타입과 부정 연산자

 불리언 타입

- true
- false

숫자 1이 조건문에 들어가면 타입 변환이 일어나  true가 됨. 

-1도  true 

:rotating_light: **0을 제외하고 숫자나 문자열이 불리언 타입으로 변환시 true가 됨 **



## :female_detective: 디스트럭처링 ( destructuring )

디스트럭처링 : 객체의 구조를 제거한다는 의미

객체의 구조를 분해 후 할당이나, 확장같은 연산을 수행

자료형에 따라 나뉨

- 객체 디스트럭처링
- 배열 디스트럭처링

### :dizzy: 객체 디스트럭처링

**객체 디스트럭처링의 기본**

객체 리터럴에서 변수명에 대응하는 속성값을 추출해 변수로 할당하는 데 유용 

`디스트럭처링 할당 ( destructuring assignment )` : 객체의 속성값을 변수에 할당하는 것

![ëì¤í¸ë­ì²ë§ì ëí ì´ë¯¸ì§ ê²ìê²°ê³¼](C:\Users\정시윤\Desktop\typescript_quickstart\yun\img\ëì¤í¸ë­ì²ë§ì ëí ì´ë¯¸ì§ ê²ìê²°ê³¼.png)

```javascript
console.log(id); //doitnow-man
console.log(pwd); //hahaha
```

**디스트럭처링 매개변수 선언**

```typescript
function 함수이름({매개변수 1, 매개변수 2}){
    //함수식
}
```



```typescript
function print({name, nationality : ?}){
    //함수식
}
print({name : "happy"});
```



**객체 디스트럭처링 시 type 키워드 활용**

type 키워드를 이용해 매개변수의 타입을 선언함으로써 객체 디스트럭처링을 수행할 수있음

```typescript
type C = { a : string, b?: number }
```

*?는 선택연산자 : 생략할 수 있음*



### :dizzy: 배열 디스트럭처링

배열 디스트럭처링을 이용하면 더욱 간결한 방법으로 배열 요소를 변수에 할당 가능

```typescript
let [nums1, nums2] = numbers;


//numbers 배열 세번째, 네번째 요소를 변수에 할당할려면 앞의 요소는 쉼표로 건너뛰고 원하는 위치에 있는 요소만 저장
let [,,nums3,nums4] = numbers
```



**배열 요소를 함수의 디스트럭처링 매개변수로 전달**

배열 디스트럭처링을 이용하면 함수에 배열을 전달할 때 배열 요소를 디스트럭처링 매개변수로 전달할 수 있다는 장점이 있음

```typescript
function f([first,second]) : [number,string]{}

function f([100,"hi"]);
```



## :female_detective: 전개 연산자

전개연산자 ( spread operator ) "..."

- 나머지 매개변수를 선언할 때
- 배열 요소를 확장할 때
- 객체 요소를 확장할 때

사용합니다.



###  :dizzy: 전개 연산자를 이용한 배열 요소 확장

```typescript
let arr = [3,4,5];

let arr2= [1,2, ...arr];
// [1,2,3,4,5] 
```



###  :dizzy: 전개 연산자를 이용한 객체 요소 확장

``` typescript
let obj = { c:3,d:4,e:5};

let obj2 = {a:1,b:2, ...obj};
//{a:1,b:2,c:3,d:4,e:5}
```

전개 연산자는 얖은 복사 방식으로 값을 복제함.

