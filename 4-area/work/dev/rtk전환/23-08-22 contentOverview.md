--- 

creation date: 23-08-22 
modification date: 23-08-22 
tag: 

---

#### ContentOverViewFetcher

- useFetchContentHomeOverview
	- refetch 제공(기존 캐싱된 데이터 있더라도 이 옵션 전달되면 리페치)
	- 


### fetcher, fetch.thunk

- contentKeytalkContainer (키토크라 안쓸듯?)
- ~~pages/buy/ticket - 이용권 구매 페이지~~
	- 
- pages/content/seriesid
	- ssr에서도 사용중 (fetch.thunk)
- ~~content/seriesid/history/ticket~~
- useContentHomeLikeAndAlarm (fetch.thunk)
	- 좋아요 알람 설정 후 refetch함

### 리덕스 상태 사용하는 곳
- state.contentHome.fetching.overview
	- content/index.tsx - ContentMainPage (근데 이 페이지 파일을 쓰는 곳이 없다?)
	- ~~ContentAgeAuthLimitChecker~~
		- title
	- ContentMainPcContainer
		- preview
	- ~~BuyTicketContainer~~
		- contentOverview 가져오기
	- ContentHomeProductTabMobileContainer
	- ContentHomeTabInfoContainer
	- ContentHomTabPcContainer
	- ContentMainMobileContainer
	- ContentMainPcContainer
	- ContentMainTicketActionContainer
	- ContentNoticeTabMobileContainer
	- ContentNoticeTabPcContainer
	- ContentOverviewContainer
	- ContentPreviewMobileContainer
	- ContentProductListConnectionContainer
	- ContentProductListMobileConnectionContainer
	- ContentReceiveFreetickerContainer
	- ContentTicketContainer
	- selectIsSaleStating
	- contentHome/Selector
	- useContentHomeLastNotice.hook
	- useContentHomeLikeAndAlarm
	- useContentMainTicketActionInfo