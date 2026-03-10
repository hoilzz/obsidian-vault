### 1. xss & open redirect

```
// xss 예시
GET /user/auth/1666230805?access_token=sXwxxxx&app_user_id=1528545172&success_url=javascript:alert(1)&failure_url=javascript:alert(1)&user_auth_type=autoCharge&type=auto_charging HTTP/1.1


// 오픈리다이렉트 예시

[Request]
https://page.kakao.com/user/auth/1666230805?access_token=sXwxxxx&app_user_id=1528545172&success_url=https://google.com&failure_url=https://google.com&user_auth_type=autoCharge&type=auto_charging

[Response]
HTTP/1.1 200 OK

...(생략)...
</style><img alt="로딩중" src="/public/images/img_loading_static.svg" class="css-11jcu80-Loading-Loading-Loading"/></div></div></div><script src="/public/js/ini-player.js"></script></div><script id="__NEXT_DATA__" type="application/json">{"props":{"pageProps":{"initialError":{"name":"UserAuthError","message":"필요한 정보가 없습니다."},"userAuthSearchParams":{"failureUrl":"https://google.com","type":"unknown"},"pageProcessId":"1666230805","_superjson":{"values":{"initialError":["Error"]}}},"__N_SSP":true},"page":"/user/auth/[pageProcessId]/[[...pathParams]]","query":{"access_token":"sXwxxxx","app_user_id":"1528545172","success_url":"https://google.com","failure_url":"https://google.com","user_auth_type":"autoCharge","type":"auto_charging","pageProcessId":"1666230805","pathParams":["isMobile__eq__false","isWebView__eq__false"]},"buildId":"a9Fit-YL9jVcnwTnkzIQg","isFallback":false,"gssp":true,"customServer":true,"scriptLoader":[]}</script></body></html>

>>> '확인' 버튼 클릭 시 success_url 또는 failure_url 경로로 이동
```

### 해결방안

1. javascript:, alert, confirm, prompt, eval 등 XSS 공격에 사용되는 문자열 필터링 
2. 리다이렉트 URL에도 *.kakao.com* 만 허용
	- 우회패턴에 대한 완화를 위해 도메인 검증은 슬래쉬(`/`) 까지 포함
	- 역슬래쉬의 경우 브라우저, 웹뷰에서 슬래쉬로 인식하거나 치환 후 요청되니 서버에서 역슬래쉬 제거 혹은 슬래쉬로 치환한 후 도메인 검증 부탁.

```
// 아래와 같은 패턴으로 도메인 검증에 대한 우회가 이루어질 수 있으니 참고하셔서 적용 부탁드립니다.
 http://kakao.com\@google.com
 http://kakao.com\\\@google.com
 http://kakao.com.google.com
 http://google.com?kakao.com
```

- `*.kakao.com` 만 통과시켜라 이말인듯
- 아무거나.kakao.com/
- 아무거나.kakao.com?



### result

걍 node와 웹 API URL 써서 hostname 만검사


#### test 
- 웹
	- 회원가입
	- 작품홈에서
	- 웹은 리다이렉트 URL 자체가 없어서 할필요 없음.
- 웹뷰
	- 회원가입
	- 작품홈
- 빌링
	- 웹
		- 충전
	- 웹뷰
		- 충전

#dev/xss
