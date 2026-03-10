# ✅ Task Dashboard

- [[focus-workflow|Daily Focus Workflow (매일 1시간)]]

## 빠른 규칙(권장)
- 투두는 체크박스: `- [ ] 할 일`
- “오늘/연체”에 잡히게 하려면 마감일 추가: `📅 2026-02-24`
- “1시간 집중” 분류용 태그(선택): `#focus/study`, `#focus/dev`, `#focus/stock`

## 오늘/연체 (📅 기준)
```tasks
not done
due before tomorrow
sort by due
```

## 오늘만 (📅 기준)
```tasks
not done
due today
sort by due
```

## 데일리 노트 미완료 (`journal/0-d`)
```tasks
not done
path includes journal/0-d
sort by path
reverse
group by path
```

## 마감일 없는 백로그 (템플릿 제외)
```tasks
not done
no due date
path does not include template
path does not include journal/_templates
path does not include 4-worklog
sort by path
group by path
```

## Worklog 백로그 (마감일 없음)
```tasks
not done
no due date
path includes 4-worklog
path does not include 4-worklog/archive
path does not include 4-worklog/log/archive
sort by path
group by path
```

- [[4-worklog/worklog-backlog|Worklog Backlog (4-worklog)]]

## 전체 미완료 (템플릿 제외)
```tasks
not done
path does not include template
path does not include journal/_templates
sort by path
group by path
```

## 1시간 집중 후보 (`#focus/*`)

### 공부
```tasks
not done
tags include #focus/study
sort by due
```

### 개발
```tasks
not done
tags include #focus/dev
sort by due
```

### 주식
```tasks
not done
tags include #focus/stock
sort by due
```

## 안 보일 때 체크
- `Settings → Tasks → Global Filter`가 설정돼 있으면, 그 조건(예: `#task`)을 만족하는 것만 보입니다.
- Tasks 플러그인이 아직 설치/활성화 안 됐으면, 위 쿼리는 그냥 코드블록으로만 보여요.
