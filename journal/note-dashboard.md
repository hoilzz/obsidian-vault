# 노트 대시보드

최근 생성된 노트를 폴더별로 확인하는 대시보드

```dataview
TABLE
  file.folder AS "폴더",
  dateformat(file.ctime, "yyyy-MM-dd") AS "생성일"
FROM "0-inbox" OR "2-project" OR "3-garden" OR "4-area"
SORT file.ctime DESC
```
