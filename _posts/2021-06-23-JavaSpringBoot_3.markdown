---
layout: post
title:  "Java SpringBoot(2) - View 환경설정"
date:   2021-06-23 10:00:00 +0100
categories:
---

# Java SpringBoot(3) - build & run
&nbsp;
&nbsp;
• **1. 빌드 방법**
- 터미널에서 해당 프로젝트가 위치한 디렉토리로 이동 후, 다음 커맨드를 사용하여 프로젝트를 빌드한다.

```
  // 해당 프로젝트 디렉토리에서

  ./gradlew build

```

- 빌드가 완료되면 해당 디렉토리에 build 폴더가 생성된다.

• **2. 빌드 완료된 프로젝트 실행**
- 해당 프로젝트 디렉토리의 /build/libs/ 안에 생성된 .jar 파일을 다음 커맨드를 사용하여 실행한다.

```
  java -jar hello-spring-0.0.1-SNAPSHOT.jar
``` 

• **3. 정상적으로 빌드되지 않을 때**
- 다음 커맨드를 사용하면 build 폴더 자체가 삭제된다.

```
  // 빌드 삭제
  ./gradlew clean

  // 빌드 삭제와 동시에 이 프로젝트 빌드
  ./gradlew clean build
```