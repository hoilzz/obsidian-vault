---
created: 25-06-02
tags: 
done: true
wip: false
---
- [x] feature 사용하기 ✅ 2025-06-12
- [x] feature로 완전히 분리하기 ✅ 2025-06-12


- isGlobal은 한번 설정되면 안바뀌는 값일까?
	- geoIP API 기반이라 국내, 국외 달라질 수 있을 것 같다. 
	- 그러면 쿠키가 아니라 요청 base로 requestContext에 저장되어야한다. 
- 그러면 글로벌로 가입 후에 잘 쓰다가 한국 들어오면 geoIP에 의해서 국내 웹으로 동작해야하남?


- 이거 첨에 이렇게 한건 globalLoginHandler를 타면 isGlobal을 쿠키에 저장해두고 사용하려고 함.
- 근데 어차피 미로그인시에 geoIP API를 통해 requestContext에 등록되어있으니 이걸 활용하면 될듯.
- 만약 로그인 되어있다면.. 
	- 해당 유저가 로그인된 시점에 isGlobal..
	- 혹은 해당 유저가 웹서비스에서 진입할 당시에 isGlobal로 해야되냐


만약에..
- VPN으로 국내로 왔거나
- 어쨌든 해외 -> 국내 상황이 된다면

앱에서 위 케이스에서 앱을 재부팅한다고 했나..









