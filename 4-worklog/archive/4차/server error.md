---
created: 25-04-06
tags: 
done: true
wip: false
---




- 기존대로 200OK 
	- 이러면 200OK일 때 statusCode를 보고 에러 스로우 해주는게 필요.
- 400 Error
	- 이러면 ApiErrorBoundary로 잘 throw되지만 getSerializedPagewebError에서 axios error를 처리하지 않음.













