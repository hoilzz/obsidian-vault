## 구조
```tsx
// pages/search/index.tsx



<>
	{
		keyword && <SearchSuggestion />
	}
	<RecentSearches />
	<SearchMainFetcher>
		<SearchMainContainer />
	</SearchMainFetcher>
</>

// SearchMainContainer.tsx

<PopularSearches />
<RealtimeKeytalk />

```


```tsx

// pages/search/result.tsx

// i. 검색어 결과
<SearchKeywordFetcher>
  <SearchKeywordContainer />
</SearchKeywordFetcher>

```

---
## 검색 결과

- 최초 진입시 searchParam을 API 파라미터로 같이 보내서 호출한다.
	- 호출된 값을 가지고 결과 페이지를 그린다.
- 정렬, 카테고리 탭 선택시 router.push를 통해 다시 API 호출한다.
- 무한 스크롤 로딩시 API 호출한다.

fetcher
- 최초 마운트시에만 동작한다.

#### 객체의 type guard
- qs.Parsed 어쩌고로 받는 얘를, 특정 객체의 인터페이스로 타입 가드 할 수 있는지
	- 삽가능
- 

### keyword


## 문의 
- 필터나 카테고리 전부 querystring으로 관리
	- 필터나 카테고리 누를 떄마다 router path
- URL 보통 기획쪽이랑도 같이 정했나요?


웹에서 검색 관련 문의드립니다~

- 검색시 필터(카테고리, 완료작만 보기), 정렬 값을 쿼리스트링에 추가

기존에는 검색결과 페이지에서 검색어만 쿼리스트링으로 다뤘습니다.
`page.kakao.com/search?word=나혼자`

NEXT에서는 카테고리나 필터, 정렬에 대한 값도 쿼리스트링으로 다루면 어떨지 제안드립니다.
`page.kakao.com/search?word=나혼자&sort=latest&showComplete=true&categoryUid=0`

- 검색 결과 페이지에서 필터나 정렬값 선택될 때 browser history에 쌓기

쿼리스트링에 정렬이나 필터값이 변경 될 때마다 쿼리스트링이 달라지고 컨텐츠도 변경되니 history로 관리되면 좋을 거 같습니다. 타서비스(리디, 아마존) 참고해보니 history에 쌓고 있어서 요것도 제안드려요 ㅎㅎ

위에 제안드린대로 구현하게된다면 정렬이나 필터가 들어간 보관함이나 랜딩리스트도 비슷하게 구현해야할지 같이 확인 부탁드리겠습니다. 

---

보관함 검색과 작품 검색 관련 스펙이 달라서 문의드려요
보관함 검색은 검색어 입력될 때마다 결과가 나오고
작품 검색은 검색어 입력 후 검색 버튼이나 엔터 키를 통해 검색 결과를 확인할 수 있습니다. (참고로 검색은 검색어 입력될 때마다 자동완성된 검색어 리스트가 나옵니다.)
요거는 현재 스펙 그대로 해도 되고 아니면 한쪽 스펙으로 맞춰서 구현이 가능합니다. 요거 한번 확인 부탁드리고자 멘션 드립니다!

