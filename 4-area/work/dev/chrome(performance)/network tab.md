
![i](https://wd.imgix.net/image/NJdAV9UgKuN8AhoaPBquL7giZQo1/7KlTSfkZgbfcXNviw9dm.png?auto=format&w=1600)
- queueing 브라우저가 요청을 큐잉함
	- 더 높은 우선순위의 요청이 있음
	- (최대) 이미 6개의 TCP connection이 해당 origin에 연결되어있음 
		- HTTP/1.0, 1.1 only
	- 브라우저가 디스크 캐시에 잠시 공간 할당중
- Stalled. 큐잉에 설명한 이유로 인해 요청 중단됨
- Content Download.
	- 브라우저는 응답을 받는다. 
