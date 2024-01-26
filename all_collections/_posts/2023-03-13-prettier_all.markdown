---
layout: post
title: "VS Code - 프로젝트 전체에 prettier formatting 수행하기"
date: 2023-03-12 16:21:00 +0100
categories: ["tips", "vsCode"]
---

VS Code의 현재 프로젝트 전체 파일에 Prettier formatting을 수행하기 위해, 먼저 prettier를 설치한다.

이후, package.json에 다음과 같은 script를 추가한다.

```json
  "pretty": "prettier --write \"./**/*.{js,jsx,ts,tsx,json}\""
```

만일, 프로젝트에서 eslint를 함께 사용하고 있다면 위 스크립트에

```json
  eslint --fix
```

를 추가한다.

이후,

```bash
  npm run pretty
```

를 수행하면, 프로젝트 전체 파일에 위 스크립트에서 정의한 파일 형식 전체에 prettier formatting이 수행된다.
