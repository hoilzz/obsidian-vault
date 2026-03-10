---
created: 24-07-10
modified: 24-07-10
tags:
---
1. docker 초기화: docker init
2. dockerfile로 도커 이미지 정의하기
   - 베이스 이미지: `FROM`
   - 애플리케이션 코드 복사: `COPY`, `ADD`
   - 종속성 설치 `RUN`
   - 컨테이너 시작 명령어 `CMD`
```Dockerfile
# 베이스 이미지 설정 
FROM node:14 
# 작업 디렉토리 설정 
WORKDIR /app 
# 종속성 파일 복사 및 설치 
COPY package*.json ./ 
RUN npm install 
# 애플리케이션 코드 복사 
COPY . . 
# 애플리케이션 실행 명령어 
CMD ["node", "app.js"]
```
3. dockerfile로 도커 이미지 빌드
   - Dockerfile을 기반으로 빌드된 불변의 파일 시스템 스냅샷
   - 애플리케이션 코드, 런타임, 라이브러리, 환경 설정 등이 포함되어있다. 
   - `docker build -t ${tagName} .`