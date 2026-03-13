---
name: work-morning
description: 업무 아침 루틴 — 워크로그 태스크 확인 → 오늘 할 일 정리 → 워크로그 작성
user-invocable: true
allowed-tools: Read, Grep, Glob, Edit, Write, Bash, AskUserQuestion
---

# 업무 아침 루틴

출근 후 업무 아침 루틴을 진행한다.

## 순서

1. **오늘 워크로그 확인**: `4-worklog/log/` 에서 오늘 날짜(YYYY-MM-DD 형식) 파일을 읽는다. 없으면 새로 생성한다.
   - 생성 시 템플릿(`template/template-worklog.md`) 구조를 참고하되, Templater 구문은 실제 값으로 치환
   - 전날 워크로그를 읽어서 `<<` `>>` 네비게이션 링크를 올바르게 설정

2. **업무 미완료 태스크 수집**: `4-worklog/` 경로에서만 `- [ ]` 태스크를 검색한다.
   - `4-worklog/archive/`, `4-worklog/log/archive/` 제외
   - 연체(📅 오늘 이전) 우선 표시
   - `4-worklog/current/` 의 작성중 문서(wip: true) 목록도 보여주기
   - 백로그(`4-worklog/worklog-backlog.md` 참고) 간단히 표시

3. **사용자에게 오늘 할 일 확인**: "오늘 업무 뭐 할까요?" 라고 물어본다.
   - 후보 목록에서 고르거나 새로 추가 가능

4. **워크로그 업데이트**: 사용자가 선택하면 오늘 워크로그의 `오늘 할 일` 섹션에 태스크를 작성한다.
   - 공유 사항이 있으면 `공유` 섹션에도 작성

5. **마무리**: 오늘 업무 계획을 간단히 요약해서 보여준다.

## 규칙
- 한국어로 대화
- 간결하게
- 워크로그에 쓸 때 기존 템플릿 구조를 유지할 것
- `4-worklog/` 범위만 다룬다 (개인 태스크는 `/morning` 사용)
