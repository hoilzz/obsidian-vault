---
created: 26-01-07
tags: 
done: 
wip: true
---
https://www.anthropic.com/engineering/claude-code-best-practices

### claude-code-best-practice 요약
1. CLAUDE.md에 프로젝트 정보를 추가한다.
2. 워크플로우
	1. 탐색 - 플랜-구현-리뷰&PR
	2. TDD
3. safe YOLO 모드 (dangerously-skip-permissions)
4. 멀티 클로드 워크플로우
	1. 하나는 코드 작성, 다른 하나는 검증 with git worktree

### 

## claude-code-best-practice
### CLAUDE.md
- 자동으로 컨텍스트(프로젝트 정보)를 불러온다. 

- 루트 디렉토리의 이 파일은 클로드의 성능을 비약적으로 상승 시킨다(영구 Context)
	- 프로젝트 가이드: 빌드/테스트 명령어, 스타일 가이드(tailwind 사용, 함수형 컴포넌트 선호)
	- 도메인 지식: 비즈니스 로직의 특이사항이나 자주 쓰이는 유틸리티 함수 위치
	- 에이전트 규칙: 수정 전 반드시 테스트 코드를 작성할 것, API 타입은 types/api.ts 를 참조할 것
- 워크플로우 속도와 정확도를 높이려면 튜닝해야한다.
	- repo 루트, app별 루트
	- 중요한 핵심정보만 간결하게
	- 명령/도구 사용 사례 중심으로 작성
	- 컨텍스트 양이 너무 많으면 claude가 느려진다. 
- 허용 도구 관리
- github CLI(gh) 설치
	- 깃헙 작업(PR 생성/수정)을 클로드에게 맡기려면 gh CLI를 설치하자
- .claude/commands에 템플릿 프롬프트를 넣으면 command로 빠르게 호출할 수 있다.
```
/project:fix-github-issue 1234
```

### 워크플로우 4단계 (explore - plan -implement - review)

1. explore: 관련 파일을 읽고 맥락을 파악하고 + CLAUDE.md 같은 프로젝트 가이드 파일을 이용해 전체 구조를 이해해야한다. 
2. plan: think (think hard) 를 통해 Claude가 논리적으로 고민하게 만들어야한다. 어떻게 수정할 건지 계획을 세워서 컨펌하는 단계를 거친다.
3. implement: 구현 고고. 클로드는 작은 단위로 수정하고 테스트 실행하며 검증하는 방식을 선호.
4. review & PR: 구현이 끝나면 변경 사항을 요약하고, PR 업로드

- 계획단계가 매우 중요하다. 

#### 테스트 작성 -> 커밋 -> 코드 -> 반복

1. 기대되는 입출력 기준 테스트 생성
2. 테스트 실행, 실패 확인
3. 테스트 커밋
4. 테스트를 통과하는 코드 작성 -> 바반복

### 워크플로우 최적화

- 명확한 지시
	- 구체적인 파일명/작업 단위를 줘야한다
	- 일반적인 지시보다 __세부조건, 기대결과__ 를 추가해야한다. 


### 멀티 Claude 워크플로우

- 하나는 코드 작성, 다른 하나는 검증
- 두번째 클로드가 첫번째 클로드 작업 리뷰
- 세번쨰 클로드로 코드와 리뷰 피드백 읽기
- git worktree


### 하위 에이전트(sub-agents) 활용

- 복잡한 문제는 서브에이전트가 특정 부분을 조사 및 검증하도록 두는게 효율적
	- context 길어지는 것 방지

---


---


## Boris Cherny claude code 

1. 여러 클로드 세션 병렬로 진행
2. 터미널 외에 웹에서도 추가로 claude qudfuf tlfgod
3. 팀 전체가 공유하는 CLAUDE.md
	1. 클로드가 실수할 떄마다 이 파일에 추가하여 같은 실수 계속 안하도록 학습 시킨다.
	```sh
		타입체크, 런 테스트, 커밋 전 린트, PR 생성 전에 할일 등에 대한 명령어 명시
	```
4. Plan Mode  -> auto-accept edits
5. slash command
	1. /commit-push-pr 같은 슬래시 커맨드 만들어 쓰고, 대부분 git bash를 포함한 반복작업 자동화
6. Sub Agents (build-validator, code-architect, oncall-guide..)
	1. code-simplifier: 코드 정리
	2. verify-app: 최종 테스트 흐름
7. PostToolUse Hooks로 포매팅
	1. 코드 생성 후 자동 포맷팅 되는 훅 사용
8. 모든 도구 연결: 개발에 필요한 여러 도구 자동 연결
	1. Slack에 메시지 보내기
	2. Sentry 로그 검색
	3. 쿼리 실행 등
9. 장시간 작업 처리법
	1. 백그라운드 에이전트에게 검증
	2. 에이전트 stop hook 사용
	3. ralph-wiggum 플러그인 활용: 장시간 작업 방치용 유틸임. 
10. 핵심팁: 검증 루프를 만들어라
	1. AI가 생성한 코드를 스스로 검증할 수 있도록 설정하면 품질 2-3배 향상
	2. 구체적으로
		1. 브라우저 자동 테스트
		2. 테스트 스위트 실행
		3. CLI에서 bash명령으로 검증

## 내 생각

> 병렬 프로그래밍에 대한 이해도를 높여야한다


> A 작업 중
> B 작업 중



1. Planning을 통한 PRD
2. PRD를 기반으로 Taskmaster 
3. 의존성 없는 것들을 병렬로 업무 수행
4. PRD를 코드로 옮기기 위한 서브에이전트 및 스킬
	1. API 연동 agent: Fetcher(path, http method) + Type(parameter, Response) + Adapter
	2. container agent
	3. presentational agent
	4. 단위 테스트 agent
	5. e2e test agent
5. 위 과정에서 서브에이전트 및 스킬을 이용하여 테스트 작성 -> 커밋 -> 코드 작성 ->  이걸 반복
6. 모든 태스크 작업 완료 및 테스트 통과시 PR업로드




