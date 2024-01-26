---
layout: post
title: "VS Code - 프로젝트 전체에 organize import(import 정리) 적용하기"
date: 2024-01-19 12:30:00 +0100
categories: ["tips", "vsCode"]
---

VS Code의 현재 프로젝트 전체 파일에 organize import 를 적용하기 위해,
먼저 파일 저장 시점에 organize import를 수행하도록 설정을 변경한다.

vsCode의 설정 -> 사용자 설정 -> 설정 파일을 열어 다음 코드를 추가한다.

```json
  "editor.codeActionsOnSave": {
    "source.organizeImports": "explicit"
  }
```

위 코드를 설정파일에 추가하면, 이후 파일을 저장할 때 마다 import를 자동으로 정리한다.
(정렬, 미사용 import 제거)

전체 프로젝트에 대해 위 내용을 일괄 적용하려면 검색/바꾸기 기능을 통해
'import' 라는 텍스트를 동일하게 import, 또는 'import ' 로 변경해준다.

-

React 프로젝트의 경우 eslint가 react/react-in-jsx-scope 관련 오류를 발생시킬 수 있다.
이는 React 프로젝트를 babel이 빌드할 때, 이 파일이 React.js 임을 알려주기 위해서는
React를 import 해 줘야 하기 때문에 발생하는 오류이다.

하지만, React를 소스 내에서 직접적으로 사용하지 않으면 organize import 과정에서 React import가
제거된다.

Next.js를 사용하는 프로젝트의 경우 Next.js가 알아서 위 작업을 해 주기 때문에 eslint 파일에
아래 코드를 추가하여 ignore 할 수 있다.

```json
  'react/react-in-jsx-scope': 'off',
```

또는 모든 jsx 파일에 일괄적으로 import React from 'react' 를 추가해주는 방법도 있는 것 같다.

[react-in-jsx-scope 관련 stackOverflow](https://stackoverflow.com/questions/42640636/react-must-be-in-scope-when-using-jsx-react-react-in-jsx-scope)

> If you'd like to automate the inclusion of import React from 'react' for all files that use jsx syntax, install the react-require babel plugin:
>
> npm install babel-plugin-react-require --save-dev
> Add react-require into .babelrc. This plugin should be defined before transform-es2015-modules-commonjs plugin because it's using ES2015 modules syntax to import React into scope.

```json
{
  "plugins": ["react-require"]
}
// Source: https://www.npmjs.com/package/babel-plugin-react-require
```
