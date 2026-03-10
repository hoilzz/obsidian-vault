---
created: 25-04-07
tags: 
done: true
wip: false
---
- [x] 제롬과 해당 문제에 대해 앱쪽 해결책에 대해 문의한다 📅 2025-04-07 ✅ 2025-04-07
- [x] 기획과 해당 문제에 대해 기존 해결책에 대해 문의한다 📅 2025-04-07 ✅ 2025-04-07
- [x] ini player 엘린과 논의한대로 구현 찾아보기 📅 2025-04-08 ✅ 2025-04-10
- [x] DRM issue 관련 문서 정리하기 📅 2025-04-17 ✅ 2025-04-21


https://kakaoent.atlassian.net/wiki/spaces/KAKAOENTFE/pages/3850436766/ini-player+DRM
[참고](https://kakaopagecorp.slack.com/archives/C048X4VRJ9H/p1743757324566099?thread_ts=1743648208.337059&cid=C048X4VRJ9H)

### 이슈 파악

- 안드로이드에서 로컬 서버를 프록시 태울 경우 영상 재생안된다. (3002)
- security_level 4로 업
	- 안드로이드, PC, iOS 전부 검은 화면만 나온다. error 이벤트에 걸리지 않는다. 


### 기기 테스트

- 조건: security_level: 4 + video option
	- Zenfone 4 (L1, 근데 L3로 동작한다고 한듯?): 9999에러(IniInternal)
		- 기존에는 재생, 캡처 블락 O
	- G5: 재생 O, 캡처 블락 O
	- S22 ultra: 재생 O, 캡처 블락 O

- 전부 확인

---

### 작업 리스트
1. 현재 상황에서 9999(ini internal error)
	- 일단 9999가 언노운 메시지를 보여주니 엘린이 알려준 메시지 고고
	- 엘린 메시지대로라면 다시시도 버튼 빼야될 거 같다..

2. 1번에서 좀 더 진화된 형태: 
	- L1, L3 구분할 수 있는 해키 코드를 통해 drm security_level 동적으로 요청. 

### 2차 작업

#### OS 11 이하 security level 1 (default)

| 기기                     | 크롬 재생 | 삼성 재생 | 크롬 캡처, 녹화 불가 | 삼성 캡처, 녹화 불가 |
| ---------------------- | ----- | ----- | ------------- | ------------ |
| 갤럭시 S7 Edge (7.0)      | O     | O     | O             | X            |
| 갤럭시 S7 Edge (8.0)      |       |       |               |              |
| 갤럭시 노트5 (7.0)          | O     | O     | O             | X            |
| 갤럭시 노트 20 Ultra (10.0) | O     | O     | O             | X            |









---

### 개념정리

- Widevine: google이 만든 DRM 기술. 동영상 불법 복제, 캡처 보호하기 위해 사용.

#### widevine 보안 수준

| 레벨     | 설명                                 | 용도                        |
| ------ | ---------------------------------- | ------------------------- |
| **L1** | 키 관리 + 비디오 디코딩을 **하드웨어(TEE)**에서 처리 | **HD/4K 재생 허용**, 캡처 방지 강함 |
| **L2** | 키만 하드웨어, 디코딩은 소프트웨어                | 거의 안 씀                    |
| **L3** | 모든 걸 **소프트웨어**에서 처리                | **SD 화질만**, 캡처 방지 어려움     |

#### widevine L1, L3 support
```ts
async function getWidevineLevel() {
    const keySystemAccess = await navigator.requestMediaKeySystemAccess("com.widevine.alpha", [{
  initDataTypes: ["cenc"],
  videoCapabilities: [{ contentType: "video/mp4; codecs=\"avc1.42E01E\"" }]
}])
    return keySystemAccess.getConfiguration().distinctiveIdentifier === 'required' ? 'L1' : 'L3'
}
```
- 참고로 widevine은 안드로이드만 지원, iOS는 fairplay

### 서비스정책
- _L1,L2,L3 가 OS레벨과 매핑되는지 혹은 디바이스와 매핑되는지_
	- 이거는 OS 버전과 상관없음. 기기에 따라 달라짐
	- ex. 아이폰 15에서 iOS12에서는 L1 지원, L1 지원하도록 iOS 업데이트 -> 이거 아님. 걍 기기 고정임.
- 

- 디바이스 L1, L3 등 디바이스의 L1 미지원에 따라 security_level 동적 요청하는지?
	- 한다면 어떻게 구현되어있는지
- 우리 상황이 기존에 security_level 1이었는데.. android OS11~14 브라우저에서 캡처 녹화를 막기 위해 security_level을 4로 설정해야됨.
	- 이렇게 되면 L1미지원 브라우저에서 재생이 안되기 때문에 이에 대한 서비스 정책을 알고 싶다.


- L1을 기본적으로 동작하려고 함.
- L1미지원에서는 에러가 떨어짐. 
	- 어떤기계가 문제가 있는지 에러코드가 있는 경우 
	- 시큐리티 레벨 낮춰서 다시 호출
	- 기본저긍로 1~3 L1 -> L3로 바로 낮춤. L2는 낮춤.
	- 보여주기라도 하자..
	- 근데 어쨌든 얘기해보니까 L3일 때 구글의 화이트리스트와 달라서 생긴 문제를 해결. 즉, 우리랑 문제 결이 다름.
		- 시큐리티 레벨 왜캐 높임? 더 낮아도 되는데 -> 구글의 화이트리스트
		- 근데 이건 잘못된 화이트리스트. 시큐리티 레벨 높여야 
	- 여튼 우리 상황(시큐리티 레벨 단순히 높여서 그냥 L1 미지원 재생 불가)
		- 어떻게든 재생 VS 캡처 막기 중 정책 방향 알고 계신거
		- 후자라고 답변 받음. 저작권 이슈가 더 크기 때문에
- 이거 관련 스펙은 예전부터 정해진거임. 페이지에서 영화 보여줬었으니까..




