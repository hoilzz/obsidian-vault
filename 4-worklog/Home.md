---
tags:
  - worklog/home
---

# Worklog Home

- 규칙: [[rule]]
- 진행중 문서: [[current/_index|current index]]

## 🚨 기한 지난 것

```dataview
TASK FROM "4-worklog"
WHERE !completed
AND !contains(file.folder, "4-worklog/log/archive")
AND !contains(file.folder, "4-worklog/archive")
AND (
  (due AND due < today) OR
  (scheduled AND scheduled < today) OR
  (contains(file.folder, "4-worklog/log") AND date(file.name) < today)
)
SORT choice(due, due, choice(scheduled, scheduled, date(file.name))) desc
```

## ✅ 오늘

```dataview
TASK FROM "4-worklog"
WHERE !completed
AND !contains(file.folder, "4-worklog/log/archive")
AND !contains(file.folder, "4-worklog/archive")
AND (
  (due AND due = today) OR
  (scheduled AND scheduled = today) OR
  (contains(file.folder, "4-worklog/log") AND date(file.name) = today)
)
SORT choice(due, due, choice(scheduled, scheduled, date(file.name))) asc
```

## 📅 이번주 (남은)

```dataview
TASK FROM "4-worklog"
WHERE !completed
AND !contains(file.folder, "4-worklog/log/archive")
AND !contains(file.folder, "4-worklog/archive")
AND (
  (due AND due > today AND due <= eow) OR
  (scheduled AND scheduled > today AND scheduled <= eow)
)
SORT choice(due, due, scheduled) asc
```

> [!note]- 전체 미완료 (캡처함: log)
> ```dataview
> TASK FROM "4-worklog/log"
> WHERE !completed
> AND !contains(file.folder, "4-worklog/log/archive")
> SORT file.name desc
> ```

> [!warning]- archive에 남아있는 미완료 (정리 필요)
> ```dataview
> TASK FROM "4-worklog/log/archive"
> WHERE !completed
> SORT file.name desc
> ```
>
> ```dataview
> TASK FROM "4-worklog/archive"
> WHERE !completed
> SORT file.mtime desc
> ```

> [!note]- 전체 미완료 (진행: current)
> ```dataview
> TASK FROM "4-worklog/current"
> WHERE !completed
> SORT file.mtime desc
> ```

## WIP 문서 (current)

```dataview
TABLE file.folder as "folder", wip
FROM "4-worklog/current"
WHERE wip = true
SORT file.mtime desc
```

## 최근 수정 (current)

```dataview
TABLE file.mtime as "updated"
FROM "4-worklog/current"
SORT file.mtime desc
LIMIT 20
```
