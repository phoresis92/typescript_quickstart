# Typescript Quick Start

## Intro

```bash
Node.js 8.9.4
npm install -g typescript@2.7.2
```

책에서 진행할 타입스크립트 버전



```bash
$ tsc hello.ts <- 컴파일
$ node hello.js <- 실행
```

`npm install -g ts-node@3.3.0`

컴파일과 실행 한번에 실행

```bash
$ ts-node hello.ts
```



프로젝트 단위로 컴파일 `tsc` 명령어만으로 컴파일을 수행

```bash
$ tsc --outDir ./dist
```

루트 디렉터리 아래 dist디렉터리 생성, 컴파일 결과 저장



## 01 _ 타입스크립트 소개

### 1.1.2. ECMA스크립트를 지원하는 타입스크립트

- 타입스크립트는 ECMA스크립트를 따르기 때문에 자바스크립트의 특성을 침범하지 않고 최신 ECMA 표준을 지원합니다. 그래서 타입스크립트를 자바스크립트의 상위 집합(superset) 언어라고 합니다.

  ES5 < ES6 < ES7 < ES NEXT < 타입스크립트



### 1.2.1. 타입스크립트의 특징

1. 타입스크립트 = 정적 타입 언어 (static type language) : 컴파일 타임(compile time) => 타임검사 시간 but 안전성 보장
2. 자바스크립트 = 동적 언어 (dynamic language) : 동적 타이핑(dynamic typing) => 런타임시 속도가 빠름

- 타입스크립트 컴파일 단계까지만 사용, 실행은 자바스크립트로 이뤄짐 => 자바스크립트를 보완하는 역할

```bash
타입스크립트 = 자바스크립트 + 타입
Javascript that scales (확장된 자바스크립트)
```



* 대규모 애플리케이션의 필요 조건

1. 모듈화 기능을 통해 모듈 간의 결합도(coupling)를 줄일 수 있어야 한다.
2. 객체지향 프로그래밍을 지원해 코드의 중복과 복잡도를 낮출 수 있어야 한다.
3. 코드 규모가 커지더라도 구조화를 갖춰 개발할 수 있어야 한다.



* 타입스크립트는 다음과 같은 특징을 지원

1. 모듈 시스템 - ES6 모듈과 네임스페이스 지원
2. 클래스와 인터페이스 지원
3. 타입 시스템 지원



#### 1. 모듈 시스템 - ES6 모듈과 네임스페이스 지원

global namespace

```bash

	namespace A	|	namespace B	| namespace C

```

- global namespace과 구분된 별도의 이름 영역을 가진다
- namespace A와 namespace B는 이름 영역이 다르므로 같은 이름의 모듈을 각각 선언 가능



#### 2. 클래스와 인터페이스 지원

- 타입스크립트는 ES6에 없는 인터페이스 선언과 인터페이스 구현을 지원한다.
- 자바는 다중 생성자를 선언할 수 있지만, 타입스크립트는 단일 생성자만 선언할 수 있다.
- 타입스크립트는 자바와 달리 디폴트 초기화 매개변수와 선택 매개변수를 선언할 수 있다.

#### 3. 타입 시스템 지원

- type system, type annotation을 이용해 변수에 타입을 선언
- if 문을 이용한 타입 검사가 많아질수록 런타임 오버헤드(runtime overhead)가 발생 => 타입을 추가하면 런타임에 불필요한 타입검사를 하지 않게 되어 코드양을 줄일 수 있고 성능 저하도 줄일 수 있다.



### 1.2.4. 타입스크립트의 아키텍처



| 에디터              |                       |
| ------------------- | --------------------- |
| 독립 서버(tsserver) |                       |
| 언어 서비스         | 독립 TS 컴파일러(tsc) |

```bash
코어 타입스크립트 컴파일러
```



#### 코어 타입스크립트 컴파일러

​	언어 변환 기능을 수행하는 코어 타입스크립트 컴파일러(core typescript compiler)를 기반으로 한다.

- parser
- binder
- type checker
- emitter
- pre-processor

#### 언어 서비스

​	코드를 컴파일해 도움말, 코드의 포매팅, 코드 색상 지정 같은 편집기에 필요한 기능 제공

#### 독립 서버(standalone server, tsserver)

​	컴파일러와 언어 서비스 같은 하위 레이어를 래핑해 JSON 형식을 통해 외부에 정보를 노출할 수 있게 한다.

#### 편집기

​	독립 서버를 비롯해 하위 레이어의 요소를 모두 고려해 동작하는 최종 응용 단계의 애플리케이션



### 1.2.5. 타입스크립트 컴파일러

- 컴파일링: 한 언어의 소스코드를 다른 언어로 바꾸는 것
- 트랜스파일링: 한 언어의 소스코드를 비슷한 추상화 수준의 다른 언어로 바꾸는 것

tsc 용어 의미 그대로 타입스크립트 컴파일러라고 부르는 것이 맞지만, 엄밀히 보면 트랜스파일러로서 비슷한 추상화 수준으로 변환해 주는 역할을 한다.



```bash
$ tsc --target es5 # --target 옵션에 따라 ECMA스크립트의 특정 버전으로 변환할 수 있다.
```



타입스크립트 파일(.ts) -> 타입스크립트 컴파일러 -- 컴파일 --> 자바스크립트(ES6, ESNext)

​																			|							▼바벨 (컴파일)

​																			 ---- 컴파일 --> 자바스크립트(ES5, ES3)

