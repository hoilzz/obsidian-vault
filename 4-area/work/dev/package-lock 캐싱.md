---
created: 24-04-15
modified: 24-04-15
tags:
  - dev
---


### 해결책 리스트
1. npm version 에 옵션이 있는지 확인
   - 없는 거 같다.
2. version 업을 해서 yarn 에 지원하는 것처럼 있는지 확인 
   - https://github.com/npm/npm/issues/17237
   - 10.5.2로 올려봤는데 안됨
3. package-lock.json 에서 상위 2개 버전을 제거하여 캐싱키 생성 및 사용 
4. 간단하게 해시키로 packge.json 사용해볼까? dependency와 devdependency만 뽑아서..


- 기존에는 npm ci 


## wiki

- lerna clean -y
	- 모든 패키지의 node_modules 디렉토리 제거 (https://github.com/lerna/lerna/tree/main/libs/commands/clean#readme)
	- 근데 보니까 apps 하위로만 다 지우고 있음
- npm ci -> Post Install
  - lerna exec -- npm i --force
- 
