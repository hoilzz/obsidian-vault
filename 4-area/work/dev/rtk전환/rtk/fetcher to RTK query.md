
[[rtk overview]]를 익혀서 fetcher-container의 복잡한 부분을 대체해보자


## fetcher-container의 복잡한 것?
- fetcher쪽 캐싱하는 방식 제각각
	- id를 통한 해결방법
- codegen으로 boilerplate 감소
	- graphql document 만 선언하면 hook 까지 자동으로 만들어주고 이 응답은 normalize까지!


- use 어쩌고는 리덕스에 캐시된 데이터를 저장함.
	- 이 캐시된 데이터는 리덕스 기반 데이터
	- fetcher에서도 use~ 이거 쓰고
	- container에서도 use~ 이거 쓰면 어차피 캐시된 데이터 가져와서 상관없지 않을까?ㅐ

---
### action
- cache-behavior 읽기
- rtk-query를 적용하면 이점
	- 기존 대비 뭐가 줄어드는지
- 검색쪽 적용해보기
	- use 저 방식이 되면
	- use로 fetcher에서 바로 가져와서
- 캐시 적용하기
