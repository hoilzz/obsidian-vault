---
created: <% tp.file.creation_date("YY-MM-DD") %>
tags:
---
<< [[4-worklog/log/2026-02-28]] | [[4-worklog/log/2026-03-02]] >>

### 오늘 할 일
- [ ]

### 공유
- 

### 작성중인 문서 (`4-worklog/current`)
```dataview
LIST
FROM "4-worklog/current"
WHERE wip = true OR done != true
SORT file.mtime desc
```

<%*
await tp.file.rename(tp.date.now("YYYY-MM-DD"));
%>