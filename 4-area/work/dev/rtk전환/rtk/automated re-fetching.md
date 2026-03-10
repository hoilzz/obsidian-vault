mutation endpoint에 영향을 받는 데이터를 가지는 쿼리 endpoint에 대해 자동화 re-fetch 하는 cache tag 시스템이 있음.

특정 mutation을 실행하면 특정 쿼리의 캐시 데이터를 _invalid_ 한다. 캐시된 데이터는 invalid되고 만약 active subscription에 캐시 데이터가 있다면 re-fetch 함

---

### 컨텐츠홈 적용

- 컨텐츠홈 mutation, query 파악
- 연관관계 파악
	- 어떤 mutation 실행시 query를 재실행 하는지

- ContentHomeOverview(seriesId)
	- seriesId 변경마다 refetch
	- setSeriesLike Mutation
		- fetchContentHomeOverview(seriesId)
	- setSeriesAlarm
		- fetchContentHomeOverview
	- writeComment
		- commentListAndBlockUserList
		- 대댓글은
		- commentList
