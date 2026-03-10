---
created: 24-07-16
modified: 24-07-16
tags:
---
https://podo.agit.in/g/300061151/wall/405842496
## host 추가 준비

- API 쪽과 동일한 호스트를 사용하여 CORS 방지
- 신규 도메인 생성 요청 : https://podo.agit.in/g/300061151/wall/399916578
	- request-domain과 request-record가 있다. 
		- 근데 서로 다른 도메인에 record는 같은걸 쓰면 ingress에서 host를 보고 service를 분기해주는 느낌?
	- https://podo.agit.in/g/300061151/wall/399620898

## 문의
- 도메인 이름 중 상위 도메인(kakaoent.io)은 API를 따라가는게 나을 거 같은데 하위도메인은 어떻게 하면좋을지
	- {env}-translation.kakaoent.io, {env}-clip.kakaoent.io
	- CORS나 인증서, 쿠키 세션 공유의 이점


## 추가 제안사항

- [참고](https://kakaopagecorp.slack.com/archives/C05J5CK5FE3/p1721270980482799)
편하게 하려면 인증서 받아야돼서 귀찮긴 한데  
`*.``[web.kakaoent.io](http://web.kakaoent.io/)` 처럼 저희만 관리하는 도메인 받아놓는게 편하긴 합니다.[web.kakaoent.io](http://web.kakaoent.io/) 를 저희쪽 Ingress 로 등록해놓으면 여기 하위 도메인이 모두 Ingress 로 들어올거고,  
[dev-ai-shorts.web.kakaoent.io](http://dev-ai-shorts.web.kakaoent.io/) 와 같은 하위 도메인은 내부에서 간단하게 vhost 기반으로 룰 만들면 되니까요.

근데 와일드카드 인증서 하나 더 받으려면 비용도 있어서  
*.[kakaoent.io](http://kakaoent.io/) 하나로 관리하는것도 괜찮을 것 같네요 ㅋㅋ

