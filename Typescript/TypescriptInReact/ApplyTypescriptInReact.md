# React App에 Typescript 적용하기

## 1. 개요

React App에 typescript를 적용하기 위한 방법은 크게 두 가지가 있다. 첫 번째는 처음부터 typescript를 설정하는
방법이고 두 번째는 typescript가 적용되지 않은 React App에 몇 가지 모듈을 설치하고 설정 파일을 작성하는 방법이다.

이번 챕터에서는 위의 두 가지 방법에 대해 정리한다.

---

## 2. CRA으로 Typescript 설정하기

CRA(create-react-app)에 대한 내용은 React의 [Create React app](/REACT/CreateReactApp.md) 챕터에
자세하게 설명되어 있으니 참고하면 된다.

CRA으로 typescript을 설정하는 방법은 한 줄의 명령어로 가능하다.(아래 명령어 참고)

> $ npx create-react-app my-app --template typescript

위의 명령어를 통해 React App를 처음 생성하게 되면 자동으로 typescript가 설정된다.
아래는 생성 직후의 폴더 모습이다. `tsconfig.json` 파일이 생성된 것과 `package.json`에 typescript과 관련된
모듈들이 설치된 것을 확인할 수 있다.

![apply_typescript_react_cra1](/image/Typescript/ApplyTypescriptInReact/apply_typescript_react_cra1.png)  
![apply_typescript_react_cra2](/image/Typescript/ApplyTypescriptInReact/apply_typescript_react_cra2.png)

---

## 3. 기존 React App에 Typescript 적용하기

React 프로젝트를 진행하는 도중에 typescript를 적용하고 싶으면 어떻게 해야할까? CRA방법으로 하기 위해선 처음부터 다시 빌드를 해야하기
때문에 지금까지 작성한 코드를 다시 작성해야 할 수 있다.

그렇다면 어떻게 해야할까? 방법은 간단하다. typescript과 관련한 모듈(라이브러리)을 직접 설치하고 `tsconfig.json` 파일을 만들어 설정해주면 된다.

---

### 3-1. Typescript 의존성 추가

아래의 명령어를 통해 타입 스크립트를 적용하기 위해 필요한 라이브러리들을 `package.json`의 의존성에 추가한다.

- npm

  > $ npm i typescript @types/node @types/react @types/react-dom @types/jest --save-dev

- yarn
  > $ yarn add typescript @types/node @types/react @types/react-dom @types/jest --dev

위의 명령어를 수행하면 `package.json`의 `devDependencies`에 아래와 같은 의존성 라이브러리들이 추가된다.

![apply_typescript_react_1](/image/Typescript/ApplyTypescriptInReact/apply_typescript_react_1.png)

---

### 3-2. Typescript 설정

의존성 라이브러리들을 추가하였으면 `tsconfig.json` 파일을 만들어 필요한 typescript 설정을 해야한다. 직접 파일을 만들어도 되지만
아래의 명령어를 통해 만들 수 있다.

> $ tsc --init

해당 과정에서 아래와 같은 오류가 등장할 수 있다.

![apply_typescript_react_2](/image/Typescript/ApplyTypescriptInReact/apply_typescript_react_2.png)

이는 `tsc` 명령어를 찾을 수 없다는 뜻인데 typescript를 글로벌로 설치하면 해결 가능하다.(아래 명령어 참고)

> sudo npm i typescript -g

`$ tsc --init` 명령어를 통해 `tsconfig.json`이 만들어지면 처음 보는 모습은 아래와 같을 것이다.

```javascript
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Save .tsbuildinfo files to allow for incremental compilation of projects. */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./.tsbuildinfo",              /* Specify the path to .tsbuildinfo incremental compilation file. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects. */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    /* Language and Environment */
    "target": "es2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    // "lib": [],                                        /* Specify a set of bundled library declaration files that describe the target runtime environment. */
    // "jsx": "preserve",                                /* Specify what JSX code is generated. */
    // "experimentalDecorators": true,                   /* Enable experimental support for TC39 stage 2 draft decorators. */
    // "emitDecoratorMetadata": true,                    /* Emit design-type metadata for decorated declarations in source files. */
    // "jsxFactory": "",                                 /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h'. */
    // "jsxFragmentFactory": "",                         /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */
    // "jsxImportSource": "",                            /* Specify module specifier used to import the JSX factory functions when using 'jsx: react-jsx*'. */
    // "reactNamespace": "",                             /* Specify the object invoked for 'createElement'. This only applies when targeting 'react' JSX emit. */
    // "noLib": true,                                    /* Disable including any library files, including the default lib.d.ts. */
    // "useDefineForClassFields": true,                  /* Emit ECMAScript-standard-compliant class fields. */
    // "moduleDetection": "auto",                        /* Control what method is used to detect module-format JS files. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    // "rootDir": "./",                                  /* Specify the root folder within your source files. */
    // "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    // "baseUrl": "./",                                  /* Specify the base directory to resolve non-relative module names. */
    // "paths": {},                                      /* Specify a set of entries that re-map imports to additional lookup locations. */
    // "rootDirs": [],                                   /* Allow multiple folders to be treated as one when resolving modules. */
    // "typeRoots": [],                                  /* Specify multiple folders that act like './node_modules/@types'. */
    // "types": [],                                      /* Specify type package names to be included without being referenced in a source file. */
    // "allowUmdGlobalAccess": true,                     /* Allow accessing UMD globals from modules. */
    // "moduleSuffixes": [],                             /* List of file name suffixes to search when resolving a module. */
    // "resolveJsonModule": true,                        /* Enable importing .json files. */
    // "noResolve": true,                                /* Disallow 'import's, 'require's or '<reference>'s from expanding the number of files TypeScript should add to a project. */

    /* JavaScript Support */
    // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the 'checkJS' option to get errors from these files. */
    // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
    // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from 'node_modules'. Only applicable with 'allowJs'. */

    /* Emit */
    // "declaration": true,                              /* Generate .d.ts files from TypeScript and JavaScript files in your project. */
    // "declarationMap": true,                           /* Create sourcemaps for d.ts files. */
    // "emitDeclarationOnly": true,                      /* Only output d.ts files and not JavaScript files. */
    // "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If 'declaration' is true, also designates a file that bundles all .d.ts output. */
    // "outDir": "./",                                   /* Specify an output folder for all emitted files. */
    // "removeComments": true,                           /* Disable emitting comments. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "importHelpers": true,                            /* Allow importing helper functions from tslib once per project, instead of including them per-file. */
    // "importsNotUsedAsValues": "remove",               /* Specify emit/checking behavior for imports that are only used for types. */
    // "downlevelIteration": true,                       /* Emit more compliant, but verbose and less performant JavaScript for iteration. */
    // "sourceRoot": "",                                 /* Specify the root path for debuggers to find the reference source code. */
    // "mapRoot": "",                                    /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "inlineSources": true,                            /* Include source code in the sourcemaps inside the emitted JavaScript. */
    // "emitBOM": true,                                  /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */
    // "newLine": "crlf",                                /* Set the newline character for emitting files. */
    // "stripInternal": true,                            /* Disable emitting declarations that have '@internal' in their JSDoc comments. */
    // "noEmitHelpers": true,                            /* Disable generating custom helper functions like '__extends' in compiled output. */
    // "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */
    // "preserveConstEnums": true,                       /* Disable erasing 'const enum' declarations in generated code. */
    // "declarationDir": "./",                           /* Specify the output directory for generated declaration files. */
    // "preserveValueImports": true,                     /* Preserve unused imported values in the JavaScript output that would otherwise be removed. */

    /* Interop Constraints */
    // "isolatedModules": true,                          /* Ensure that each file can be safely transpiled without relying on other imports. */
    // "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
    // "preserveSymlinks": true,                         /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,                         /* When type checking, take into account 'null' and 'undefined'. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for 'bind', 'call', and 'apply' methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */
    // "noImplicitThis": true,                           /* Enable error reporting when 'this' is given the type 'any'. */
    // "useUnknownInCatchVariables": true,               /* Default catch clause variables as 'unknown' instead of 'any'. */
    // "alwaysStrict": true,                             /* Ensure 'use strict' is always emitted. */
    // "noUnusedLocals": true,                           /* Enable error reporting when local variables aren't read. */
    // "noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read. */
    // "exactOptionalPropertyTypes": true,               /* Interpret optional property types as written, rather than adding 'undefined'. */
    // "noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
    // "noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
    // "noUncheckedIndexedAccess": true,                 /* Add 'undefined' to a type when accessed using an index. */
    // "noImplicitOverride": true,                       /* Ensure overriding members in derived classes are marked with an override modifier. */
    // "noPropertyAccessFromIndexSignature": true,       /* Enforces using indexed accessors for keys declared using an indexed type. */
    // "allowUnusedLabels": true,                        /* Disable error reporting for unused labels. */
    // "allowUnreachableCode": true,                     /* Disable error reporting for unreachable code. */

    /* Completeness */
    // "skipDefaultLibCheck": true,                      /* Skip type checking .d.ts files that are included with TypeScript. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}
```

굉장히 많은 설정이 필요한 `tsconfig.json` 파일이다. 원하는 옵션을 추가하여 `tsconfig.json`을 설정하면 된다. `tsconfig.json`의 옵션에
대한 자세한 설명은 [Typescript 설정 파일](/Typescript/TypescriptConfig.md)에서 다룬다.

---

### 3-3. 스크립트 수정

마지막 단계이다. `.js` 파일을 `.ts`또는 `.tsx`파일로 수정한다.

아래의 `.js` 파일을 `.tsx` 파일로 수정해보자.

```javascript
import styled from "styled-components";

const Container = styled.div`
  margin: 0 auto;
  min-width: 480px;
  max-width: 480px;
  padding: 20px;
  border-radius: 20px;
  box-shadow: 5px 5px 10px rgba(100, 100, 100, 0.2);
  display: flex;
  margin-bottom: 40px;
  box-sizing: border-box;
`;

const Avatar = styled.div`
  align-self: flex-start;
  min-width: 40px;
  height: 40px;
  background-color: rgba(200, 200, 200);
  border-radius: 50%;
  margin-right: 20px;
`;

const Username = styled.div`
  font-weight: 700;
  color: rgba(60, 60, 60);
  margin: 5px 0px;
`;

const Content = styled.div`
  font-size: 14px;
`;

const Comment = ({ username, content }) => {
  return (
    <Container>
      <Avatar></Avatar>
      <div>
        <Username>{username}</Username>
        <Content>{content}</Content>
      </div>
    </Container>
  );
};

export default Comment;
```

수정하게 되면 3가지의 오류가 나타난다. 크게 나누면 2가지의 오류이다. 하나는 `styled-components`과 연관된 모듈에 대한 선언 파일을 찾지 못해서
발생한 오류이고 다른 하나는 컴포넌트의 `props`가 암묵적으로 `any` 타입을 가지기 때문에 발생한 오류이다. 하나씩 해결해보자.

---

### 3-4. 스크립트 오류 해결1 - declaration 파일 설치

첫 번째 오류에 대한 해결 방법은 빨간줄에 마우스를 올리면 쉽게 찾을 수 있다.

![apply_typescript_react_3](/image/Typescript/ApplyTypescriptInReact/apply_typescript_react_3.png)

세 번째 줄에 보면 `Try "npm i --save-dev @types/styled-components"`가 적혀있고 적힌대로 아래의 명령어를 실행하면 된다.

> $ npm i --save-dev @types/styled-components

`styled-components`와 관련된 declaration 파일(모듈, 라이브러리)이 설치되고 `package.json`의 의존성에 추가된다. 해결완료

---

### 3-5. 스크립트 오류 해결2 - props의 type 설정

두 번째 오류에 대한 해결 방법은 부모 컴포넌트에서 받은 `props`의 타입을 정해주기만 하면 된다. 코드를 아래와 같이 수정한다.

```tsx
interface IComment {
  username: string;
  content: string;
}

const Comment = ({ username, content }: IComment) => {
  return (
    <Container>
      <Avatar></Avatar>
      <div>
        <Username>{username}</Username>
        <Content>{content}</Content>
      </div>
    </Container>
  );
};
```

`interface`와 같은 typescript과 관련된 내용은 앞으로 꾸준히 정리한다. 자세한 설명은 다음에 하고 지금은 오류해결에만 집중한다.

---

## 4. Conclusion

> 티처캔에 타입스크립트를 적용하려고 한다. 그 중 첫번째 과정이 바로 이번 챕터에서 다룬 내용이지 않을까 싶다. 그리고 앞으로 티처캔에 타입스크립트를 적용하면서
> 마주치는 어려움과 배운 내용을 정리하면서 타입스크립트와 친해지도록 하자. 타입스크립트는 내편이다

---

## 참고

[기존 React App에 Typescript 적용하기](https://memostack.tistory.com/281#1.%20%EA%B8%B0%EC%A1%B4%20React%20App%EC%97%90%20Typescript%20%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)  
[[에러 해결] styled-components Could not find declaration file](https://garniel23.tistory.com/m/entry/%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0-styled-components-Could-not-find-declaration-file)

---

📅 2022-08-21
