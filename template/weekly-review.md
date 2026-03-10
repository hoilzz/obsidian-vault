---
created: <% tp.date.now("YY-MM-DD") %>
tags: weekly-review
---

# 📅 주간 리뷰 — <% tp.date.now("YY-MM-DD") %>

> [[task-dashboard|📋 대시보드]] | [[focus-workflow|🔁 포커스 워크플로우]]

## 이번 주 한 줄 회고
-

## 최근 7일 노트
```dataview
TABLE file.mtime as "수정일"
FROM ""
WHERE file.mtime >= date(today) - dur(7 days)
SORT file.mtime DESC
LIMIT 20
```
- 다시 볼 노트 / 코멘트:
    -

## 투두 정리
- [ ] 백로그에서 안 할 태스크 삭제/아카이브
- [ ] 큰 태스크 → 25분짜리로 쪼개기

## 다음 주 집중 1개
-
