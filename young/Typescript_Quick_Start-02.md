# Typescript Quick Start

## 02 _ 개발 환경 구축

```bash
$ npm outdated -g typescript # 설치된 패키지가 최신 버전인지를 확인
```

```bash
$ npm uninstall -g typescript # 기존의 전역에 설치된 패키지 삭제
$ npm cache clean # 기존 설치에 사용됐던 패키지 설치 관련 파일을 삭제
$ npm install -g typescript # 최신 버전 다운로드
```



```types
let hello: string = "hello typescript!";
console.log(hello);
```

`tsc hello.ts & node hello.js`

--target 컴파일 옵션을 생략하면 ES3 버전을 기준으로 자바스크립트를 생성



### 2.2.2. tsc 명령어와 컴파일 옵션

```bash
$ tsc -p ../ # (--project) 지정한 경로를 프로젝트 루트 디렉터리로 설정하는 옵션
	# but tsconfig.json 파일이 없다면 단독으로 사용한 tsc 명령어가 제대로 동작하지 않는다
	# Cannot find a tsconfig.json file at the specified directory: '../'
	# tsconfig.json 파일명을 바꾸었다면 파일명을 직접 지정한다.
$ tsc -p ../tsconfig2.json

```



ECMA스크립트 버전별로 컴파일하기(-t 이용)

```bash
$ tsc hello.ts -t ES5 # ES5 버전으로 변환
	# ES3(default), ES5, ES6(ES2015), ES2016, ES2017 or ESNext
```



여러파일 컴파일

```bash
$ tsc file1.ts file2.ts

$ dir *.ts /b /s > ts-files.txt
$ tsc @ts-files.txt
```

```bash 
$ tsc file1.ts, file2.ts --out file3.js # 하나의 자바스크립트 파일로 합쳐서 생성
```



변경 감지 기능

```bash
$ tsc hello.ts -watch # (-w)
```



### 2.2.3. tsconfig.json 파일을 이용한 컴파일 

- tsconfig.json 파일이 위치한 경로가 타입스크립트 프로젝트의 루트 경로가 된다

```bash
$ tsc --init # 기본 tsconfig.json 파일이 만들어진다
```

```json
{
    "compilerOptions": {
        "target": "es6",  // 컴파일 후 변환될 ECMA스크립트 버전
        "module": "commonjs", // 모듈 형식을 지정한다
        "sourceMap": true, // 컴파일 후 맵(map) 파일의 생성 여부를 결정한다
        "strict": true, // true 이면 엄격 타입검사 모드를 활성화 한다
        "noImplicitAny": false, // any 타입으로 암묵적 형변환 여부를 결정한다. default: false
        "esModuleInterop": true, // ECMA스크립트 모듈과 상호 운용성을 가능하게 하는 속성. true: CommonJS 모듈을 디폴트 모듈처럼 호출할 수 있다.
        //"outFile": "./dist-specific/out.js", // files의 파일들의 컴파일 결과를 합쳐 생성
        "removeComments": false, // true: 주석은 모두 삭제된 채로 출력
        "outDir": "dist", // 컴파일 결과를 특정 디렉터리로 출력하도록 설정, outFile 속성과 대치되는 속성, ts 파일의 컴파일 결과를 1:1로 대응시켜 .js 파일로 저장.
        "pretty": true, //                 
        "importHelpers": true,
        "strictNullChecks": false, // 변수에 null, undefined 할당 불가
        "noImplicitThis": true,
        "alwaysStrict": true,
        "noUnusedLocals": true,
        "noUnusedParameters": false,
        "noImplicitReturns": true,
        "noFallthroughCasesInSwitch": true,
        "moduleResolution": "node",
        "baseUrl": ".",
        "allowSyntheticDefaultImports": true,
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true,
        "resolveJsonModule": true,
        "lib": [
            "es5",
            "es6",
            "dom",
            "es2015.core",
            "es2015.collection",
            "es2015.generator",
            "es2015.iterable",
            "es2015.promise",
            "es2015.proxy",
            "es2015.reflect",
            "es2015.symbol",
            "es2015.symbol.wellknown",
            "esnext.asynciterable"
        ],
        // "files":[ // 컴파일 할 파일 목록을 입력하면 명령어에 파일명을 열거하지 않아도 된다.
        //     "./src/hello1.ts",
        //     "./src/hello2.ts"
        // ],
        "include": [ // 컴파일 시 사용할 디렉터리를 설정, 생략시 모든 디렉터리 컴파일 대상
            "src/**/*",
            "src2/**/*"
        ],
        "exlude": [ // 컴파일 시 제외할 디렉터리를 지정
            "node_modules",
            ".vscode"
        ]
    }
}
```

- noImplicitAny: 타입을 지정하지 않은 변수는 자동으로 any 타입으로 추가. 따라서 타입을 선언하지 않아도 타입 에러가 발생하지 않는다.
- sourceMap: 맵 파일 생성여부. 맵 파일은 컴파일 과정에서 타입스크립트 파일과 자바스크립트 파일 간의 연결 정보를 담고있다. => 개발 과정에서 디버깅에 유용but 배포시 사용하지 않으므로 상황에 맞게 설정



#### tsconfig.json 속성 확장하기

config/base.json

```json
{
    "compilerOptions": {
        "removeComments": true,
        "sourceMap": true,
        "exlude": [ 
            "node_modules",
            ".vscode"
        ]
    }
}
```

tsconfig-extends.json

```json
{
    "extends": "./config/base.json",
    "compilerOptions": {
        "outFile": "./dist/extends/out.js",
        "target": "es5"
    }
}
```



### 2.2.4. ts-node 모듈을 이용한 컴파일과 실행

```bash
$ tsc hello.ts
$ node hello.js
	=>
$ ts-node hello.ts
```



### 2.3. 타입스크립트의 개발 도구

### 2.3.1.VSCode 소개

- 타입스크립트 개발에 최적화된 경량 편집기이다.
- 타입스크립트의 설정 파일(tsconfig.json)을 기반으로 인텔리센스(IntelliSense)를 지원한다.
- 태스크러너 설정을 통해 tasks.json 파일을 기준으로 태스크를 설정할 수 있다.
- 타입스크립트 확장 기능이 많아 개발에 도움을 준다.



### 2.3.4. 태스크러너 설정

VSCode에서 빌드를 수행하는 단축키는 <Ctrl + Shift + B>

이 단축키를 눌렀을 때 빌드 작업을 자동화 하려면 태스크 러너를 설정해야 합니다. 태스크 러너를 설정하려면 `.vscode/task.json` 파일을 추가해 줘야 하는데, VSCode에서는 tasks.json파일을 생성하기 위한 명령어를 제공합니다.

1. <Ctrl + Shift + P>를 눌러 커맨드 팔레트(Command Palette)를 엽니다.

2. `Configure Task`라는 키워드를 입력하면 설정 작업을 위한 커맨드 목록 표시됩니다.
3. `tasks.json` 파일 만들기
4. `$ tsc --init` 을 통해 tsconfig.json 파일 만들기
5. `Configure Task` 재입력
   - `tsc:build `- ts 파일을 바로 컴파일한다.
   - `tsc:watch` - watch 모드 상태에서 ts 파일이 변경될 때마다 컴파일한다.



```json
{
    "version": "2.0.0", // tasks.json에서 사용할 형식(format)에 대한 버전을 명시
    "tasks": [
        { // tsc:build
            "type": "typescript",
            "tsconfig": "tsconfig.json",
            "problemMatcher": [
                "$tsc"
            ],
            "group": "build"
        },
        { // tsc:watch
            "type": "typescript",
            "tsconfig": "tsconfig.json",
            "option": "watch",
            "problemMatcher": [
                "$tsc-watch"
            ],
            "group": {
                "kind": "build",
                "isDefault": true // <Ctrl + Shift + B> 입력시 바로 실행할 코드
            }
        }
    ]
}
```



#### 특정 파일만을 빌드하도록 tasks.json 설정하기

`$ tsc hello.ts` 와 동일한 <Ctrl + Shift + B> tasks.json 작성

```json
{
    "version": "2.0.0",
    "tasks": [
        { 
            "label": "현재 TS 파일을 컴파일하기", // 사용자가 지정할 수 있는 태스크의 이름이다.
            "type": "shell", // shell 값을 명시적으로 입력했다면 셸 커맨드를 실행한다.
            "command": "tsc -w ${file}", // 실제 수행되는 명령어를 입력한다.
            "group": { // 태스크가 속한 그룹을 설정한다.
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```



#### tasks.json에 ts-node모듈 적용

```json
{
    "version": "2.0.0",
    "tasks": [
        { 
            "label": "특정 TS 파일을 ts-node로 실행, 결과보기",
            "type": "shell",
            "command": "ts-node ${file}", // ${file}=> 현재 열린 파일
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```



### 2.3.5. 타입스크립트의 확장 기능

- <Ctrl + Shift + X> 확장기능창
- Search "typescript"

```bash
Latest TypeScript and Javascript Grammar
TypeScript Toolbox
```



- 외부에 정의된 모듈을 불러올 수 있는 임포트 구문의 경우 tsconfig.json 파일에 module옵션을 추가해야 타입스크립트 컴파일러가 외부 모듈을 인식해 컴파일이 제대로 이뤄집니다.

```json
{
    "compilerOptions": {
        "module": "ES2015"
    }
}
```



#### TSLint

- 명명규칙이나 코딩 가이드를 지킬 수 있게 린트 검사를 수행
- tslint.json 파일을 기준으로 검사해 확인 가능
- tslint.json 파일은 최상위 디렉터리에 추가해야한다.
- 타입스크립트 파일에 대한 린트 검사를 수행하려면 TSLint 패키지를 별도로 설치해야한다.

```bash
$ npm install -g tslint
$ tslint --init
$ tslint ${file}
```



#### ts-node 패키지와 tslint 패키지 결합

- <Ctrl + Shift + B> 컴파일 결과와 린트 결과를 함께 확인할 수 있어서 편리하다.

```json
{
    "version": "2.0.0",
    "tasks": [
        { 
            "label": "ts-node와 ts-lint를 동시에 실행해 특정 TS파일의 결과 보기",
            "type": "shell",
            "command": "ts-node ${file} '&' tslint ${file}",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": { // 사용자 인터페이스 출력을 제어할 때 사용하는 속성
                "reveal": "always", // ---|
                "panel": "new"      // ----- 태스크가 실행될 때마다 새로운 터미널이 생성돼 출력 결과가 새로 출력된다
            }
        }
    ]
}
```

