# 노트 대시보드

최근 1개월간 생성된 노트를 주간 단위로 확인하는 대시보드

## 이번 주

```dataview
TABLE WITHOUT ID
  file.link AS "노트",
  file.folder AS "폴더",
  dateformat(file.ctime, "MM-dd") AS "생성일"
FROM "0-inbox" OR "2-project" OR "3-garden" OR "4-area"
WHERE file.ctime >= date(sow)
SORT file.ctime DESC
```

## 지난 주

```dataview
TABLE WITHOUT ID
  file.link AS "노트",
  file.folder AS "폴더",
  dateformat(file.ctime, "MM-dd") AS "생성일"
FROM "0-inbox" OR "2-project" OR "3-garden" OR "4-area"
WHERE file.ctime >= date(sow) - dur(7d) AND file.ctime < date(sow)
SORT file.ctime DESC
```

## 2주 전

```dataview
TABLE WITHOUT ID
  file.link AS "노트",
  file.folder AS "폴더",
  dateformat(file.ctime, "MM-dd") AS "생성일"
FROM "0-inbox" OR "2-project" OR "3-garden" OR "4-area"
WHERE file.ctime >= date(sow) - dur(14d) AND file.ctime < date(sow) - dur(7d)
SORT file.ctime DESC
```

## 3주 전

```dataview
TABLE WITHOUT ID
  file.link AS "노트",
  file.folder AS "폴더",
  dateformat(file.ctime, "MM-dd") AS "생성일"
FROM "0-inbox" OR "2-project" OR "3-garden" OR "4-area"
WHERE file.ctime >= date(sow) - dur(21d) AND file.ctime < date(sow) - dur(14d)
SORT file.ctime DESC
```

## 4주 전

```dataview
TABLE WITHOUT ID
  file.link AS "노트",
  file.folder AS "폴더",
  dateformat(file.ctime, "MM-dd") AS "생성일"
FROM "0-inbox" OR "2-project" OR "3-garden" OR "4-area"
WHERE file.ctime >= date(sow) - dur(28d) AND file.ctime < date(sow) - dur(21d)
SORT file.ctime DESC
```
