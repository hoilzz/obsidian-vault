

# git-flow

## 1. 브랜치부터 정석으로

- feature/KW-XXX
	- git-flow는 root 브랜치 ㄴㄴ해

## 2. git-flow 고고

- 역머지 까먹기 쉬움. 사용 ㄴㄴ
- git flow release finish RELEASE
	- release 브랜치를 master로 병합
	- release 이름으로 태그
	- release 브랜치를 develop 브랜치로 역머지(back-merge)
	- relase 브랜치 삭제

## 3. release 브랜치를 만드는 시점을 최대한 늦춘다

- develop은 언제 나가도 괜찮은 상태로 둬야함
	- 리얼 오픈되면 안되는 기능 feature flag로 제어
	- 판단이 어렵다면 일단 개발 후 feature flag로 제어해도 괜춘. 


