---
layout: post
title: "VS Code - javascript Debugger"
date: 2022-12-06 13:46:00 +0100
categories: ["tips", "vsCode"]
---

기존 vscode에서 javascript 코드 디버깅을 진행할 때에는 Debugger for Chrome을 사용하였지만,
해당 확장 툴이 deprecated 된 관계로 최근에는 javascript debugger를 사용한다.

javascript debugger 사용 방법에 대해 정리한다.

**launch.json 작성**

```json
// launch.json

{
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "chrome debugger",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}",
      "sourceMaps": true,
      "trace": true
    }
  ]
}
```

**typescript의 경우**

**tsconfig.json**을 일부 수정해 주어야 한다.

```json
  // compilerOptions 항목의 sourceMap을 true로!

  {
      "compilerOptions": {
          "sourceMap": true,
          .
          .
          .
      }
      .
      .
      .
  }
```
