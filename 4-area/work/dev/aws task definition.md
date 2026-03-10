---
created: 24-04-11
modified: 24-04-11
tags:
  - dev/ngrinder
  - dev/부하테스트
---

[[ngrinder -3]]

- memoryResevation(soft limit)
	- 컨테이너 인스턴스 뜰 때 필요한 메모리 사이즈
	- 실행중인 모든 컨테이너의 위 값 합계가  해당 컨테이너 인스턴스에서 사용 가능한 메모리 초과 불가.
- memory(hard limit)
	- memory는 컨테이너가 초과할 수 없는 메모리 상한선
	- 만약 컨테이너가 메모리 초과하면 컨테이너는 killed (by out-of-memory(OOM) killer)