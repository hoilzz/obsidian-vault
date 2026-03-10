---
created: 26-02-09
tags:
done: true
wip: false
---
해커톤 우승자의 가이드 중 필요한 것만 발췌

## 1 에이전틱 사고방식

### 1-1 분해하고 정복하라

분할정복 알고리즘 이름 딴듯?. 걍 큰 문제를 잘개 쪼개라

- 로그인 페이지 만들어줘. 
	- 이거 잘 동작안될꺼라는건 다 알듯
	- A->B보다는 A->A1->A2->A3->B 로 가라
### 1-2 계획 모드 vs 욜로 모드

### 1-3 컨텍스트: AI 기억력 지배하는 기술

- 여러 주제 한 대화에 섞으면 성능 39% 저하될 수 있음
- 관리 전략
	- 단일 목적 대화: 로그인 구현하다가 대시보드 얘기하지말기. 걍 새 탭 열어
	- 선제적 압축: HANDOFF.md 파일 만들고 \/clear 후 새롭게 시작
	   ```
	   Put the rest of the plan in the system-prompt-extraction folder as        HANDOFF.md. 
	   Explain what you have tried, what worked, what didn’t work, 
	   so that the next agent with fresh context is able to just load 
	   that file and nothing else to get started on this task and finish 
	   it up.
	   ```
	- \/context 명령어
		- 컨텍스트 사용 누가 많이 하는지 알 수 있음. (불필요한 MCP 끄자.)
	- **자동 압축 비활성화**
		- 45K 토큰 예약해놔서, 사용 가능한 컨텍스트 줄어듬

### 1-4 올바른 추상화 선택

- 바이브코딩과 딥다이브를 적재적소에
	- 바이브코딩: 일회성 프로젝트, 빠른 실험
	- 딥 다이브: 프로덕션, 보안중요, 복잡한 버그 디버깅

### 1-5 미지의 영역 더 용감하게

- rust 잘 모르는데 클로드가 하라는대로 했다가, 코드 뭔지 모르겠어서 설명 듣다가.. 잘 안되면 다른 접근 방식 썼다가.. 필요에 따라서 높은 추상화 명령(사용자 프로필 페이지 만들어줘) 했다가 낮은 추상화 명령(이 정규식이 왜 이메일 유효성 검사에 실패하는지) 등을 통해 최종 솔루션 도달


## 2 환경 설정 필수 명령어

### 2-1 커스텀 상태 라인
- 라온이 알려준 플러그인으로 충분(claude-hud)

### 2-2 필수 슬래시 명령어 마스터

- \/clear, \/usage..

### 2-3 CLAUDE.md: AI를 위한 프로젝트 설명서

- \/init: AI가 스스로 온보딩
- 실시간 메모리 업데이트
	- 자연어로 지시
	- 저자는 CLAUDE.md를 **최대한 간결**하고 명확하게 유지하라고 조언

```
// 나쁜 예
## Authentication                                                                 

Our authentication system is built using NextAuth.js, which is a complete authentication solution for
Next.js applications. It provides a flexible and secure way to add authentication to your app.We use the Credentials provider to authenticate users with email and password. The session strategy is set to JWT, which means that the session is stored in a JWT token on the client side. This is more scalable than
storing sessions in a database. When a user logs in, we verify their credentials against the database, and if they are correct, we create a JWT token and send it to the client. The client then includes this
token in the Authorization header of subsequent requests...             

---
좋은 예 (간결함):                                                               						
## Authentication                                                       

- NextAuth.js with Credentials provider                                 
- JWT session strategy                                                  
- **DO NOT**: Bypass auth checks, expose session secrets        
```

### 2-4 터미널 별칭

```
# Claude Code 기본 별칭
                                                     
alias c='claude'                                                        
alias cc='claude --continue' # 마지막 대화 이어가기                              
alias cr='claude --resume' # 대화 목록에서 선택                                 
alias ch='claude --chrome' # 크롬 통합 모드      
```

- alias 써서 타이핑 시간 아껴라~

### 2-5 세션 관리: 대화 잃지 않기

- 컴터 재부팅, 다른 작업하다가 돌아왔을 때 `claude --continue`
- 세션에 이름 붙이기: `/rename auth-system-refactor`
	- 그러면 이 세션 이름으로 재개 가능: `claude --resume auth-system-refactor`

> 중요한 작업에 이름을 붙이자-

- 현재 대화 내역 공유하기 `/export`

## 3 생산성 극대화 하기

### 3-1 음성 코딩

- 관심있으시면 보시는걸 추천

### 3-2 터미널 출력 기술

- 터미널에 나온 내용 에디터로 옮기는 정도 내용

### 3-3 전체 선택의 힘

- example.com 읽고 가져와줘 하지말고 cmd+A로 걍 복붙해라~

### 3-4 마크다운 써라~

### 3-5 단축키

### 3-6~ 다 패스

## 4 컨텍스트 관리 예술

### 4-1 선제적 컨텍스트 압축

- HANDOFF.md 활용

```
# 1. 대화가 50k 토큰 이상 사용 중일 때                                              
> "/context로 현재 사용량 확인"                                                 

# 2. HANDOFF.md 생성 요청                                                   
> "지금까지의 작업 내용을 HANDOFF.md 파일로 정리해줘.                                    
다음 에이전트가 이 파일만 읽고 작업을 이어갈 수 있도록                                       
시도한 것, 성공한 것, 실패한 것, 다음 단계를 명확히 작성해줘."                                

# 3. 새 세션 시작                                                            
> "/clear"                                                              

# 4. HANDOFF.md 로드                                                      
> "@HANDOFF.md 이 파일을 읽고 작업을 이어가줘"      
```

- handoff example
```
# 프로젝트: Stripe 결제 통합                                                    

## 목표                                                                   
Stripe Checkout을 사용한 일회성 결제 시스템 구현                                      
## 완료된 작업                                                               
✅ Stripe SDK 설치 및 API 키 설정                                              
✅ `/api/create-checkout-session` 엔드포인트 구현                               
✅ 성공/취소 페이지 라우팅                                                         

## 시도했지만 실패한 것                                                          
❌ Webhook 서명 검증 - 로컬 환경에서 테스트 불가                                        
- 해결책: ngrok 사용 필요                                                     
## 현재 문제                                                                
- Webhook 이벤트 처리 로직 미완성                                                 
- 데이터베이스에 결제 기록 저장 필요                                                   

## 다음 단계                                                                
1. ngrok으로 로컬 서버 노출                                                     
2. Stripe 대시보드에서 Webhook URL 등록                                         
3. `payment_intent.succeeded` 이벤트 핸들러 구현                                
4. 결제 기록을 `payments` 테이블에 저장                                            
```

### 4-4 대화 복제

- \/clone 으로 현재 대화 유지하면서 다른 접근 방식 시도
	- TODO: 추가하기
- \/half-clone: 대화 후반부만 유지하고싶을때(전반부 삭제)

## 5 Git과 Github 워크플로우

### 5-1 Git + Github CLI 

- PR 템플릿 + gh CLI를 활용하여 Claude가 직접 PR 작성

### 5-2 Git worktree

- worktree 쓰더라도 node_modules 다시 설치 안해도 되니까 편하다. 저자는 보통 2~3개 터미널 탭띄워서 워크트리로 병렬 작업한다

### 5-3 대화형 PR 리뷰

```
# 1. PR 체크아웃                                                       
> "gh pr checkout 123을 실행하고 이 PR의 변경 사항을 요약해줘"                          
# 2. 전체 개요 파악                                                     
> "이 PR이 어떤 문제를 해결하는지, 주요 변경 파일은 무엇인지 설명해줘"                             
# 3. 파일별 심층 리뷰                                                     
> "src/auth/middleware.ts 파일의 변경점을 분석해줘.                        
보안 이슈나 성능 문제가 있는지 확인해줘"                                               
# 4. 테스트 커버리지 확인                                                 
> "이 변경에 대한 테스트가 충분한지 평가해줘"                                              
# 5. 개선 제안                                                         
> "이 로직을 더 효율적으로 바꿀 방법이 있을까?"                                           
# 6. 자동 수정                                                         
> "네가 제안한 개선 사항을 적용하고 테스트를 실행해줘"  
```

- _테스트 코드가 추가된다면 /review 에 4번을 추가해도 좋을 것 같다_

### 5-4 승인된 명령어 검사

- 저자가 만든 cc-safe 를 이용해서 ./claude/settings.json 파일 스캔하여 위험한 승인 명령어 감지
	- sudo, rm -rf, chomd 777...
	- 저자는 한달에 한번 요거 실행한다고 함.
	- _개인적으로 skip-permission을 며칠전부터 사용중.._

## 6 고급기능 - MCP, Hooks, Agents

### 6.1 MCP

- MCP 성능 최적화
	- 10개 미만 MCP 서버
	- 80개 미만의 활성 도구
	- __사용하지 않는거 /mcp로 비활성화__

### 6.2 Hooks: 규칙 강제

AI의 확률적 행동에 __결정론적 제어__ 를 추가할 수 있다.


| **Hook 종류**             | **실행 시점**      | **주요 사용 사례**                    |
| ----------------------- | -------------- | ------------------------------- |
| **`PreToolUse`**        | 도구(Tool) 실행 직전 | 위험 명령어 차단, 입력 데이터 유효성 검사        |
| **`PostToolUse`**       | 도구(Tool) 실행 직후 | 실행 로그 기록, 결과 가공, 알림 전송          |
| **`PermissionRequest`** | 권한 요청 발생 시     | 특정 권한 자동 승인 또는 정책 기반 거부         |
| **`Notification`**      | Claude 알림 발생 시 | 외부 시스템(Slack, Email 등) 인터페이스 통합 |
| **`SubagentStart`**     | 서브에이전트 시작 시    | 하위 프로세스 모니터링 시작, 자원 할당 체크       |
| **`SubagentStop`**      | 서브에이전트 종료 시    | 최종 결과 수집, 리소스 정리 및 보고           |

- 실전 예시
	- PreToolUse에서 위험한 명령어 차단

```
 {                                                                     
	"hooks": {                                                         
	"PreToolUse": {                                                    
	 "command": "bash",                                                
	 "args": ["-c", "if echo $TOOL_INPUT | grep -q 'rm -rf /'; then                 echo 'BLOCKED: Dangerous command'; exit 1; fi"]}          
	}                                                                  
}       
```

- 저자는 hooks를 AI에게 '가드레일' 제공하는 용도로 사용한다고 함. 확률적으로 실수할 수 있으니 절대 규칙으로 강제하도록 사용.
### 6.3 Skills: 재사용 가능한 지식

- reddit 콘텐츠 가져오는 스킬 만들어서 gemini cli를 통해 레딧 크롤링 하는거 만드는 예제

### 6.4 Agents: 전문화된 서브에이전트

- 특정 작업에 전문화된 독립적인 AI 프로세스. 메인 claude가 작업을 위임할 수 있다.
	- 병렬 실행 가능
	- 작업 완료 후 결과를 메인 에이전트에게 반환
- TDD 가이드 에이전트 (https://github.com/affaan-m/everything-claude-code/blob/main/commands/tdd.md)
```
> "tdd-guide 에이전트를 실행하여 이 함수에 대한 테스트를 작성해줘"                
# Claude가 tdd-guide 서브에이전트 실행:                                    
# - 함수 분석                                                            
# - 테스트 케이스 설계                                                     
# - 테스트 코드 작성                                                      
# - 결과를 메인 에이전트에게 반환      
```

### 6.5 플러그인: 기능 번들

Hooks, Agent, Skills, MCP 서버를 하나의 패키지로 묶어서 배포하고 설치

- claude-code-tips 배포

### 6.6 CLAUDE.md vs Skills Vs slash Command Vs plugin

| **기능**               | **로딩 시점**  | **주요 사용자** | **토큰 효율성**      | **주요 사용 사례**                       |
| -------------------- | ---------- | ---------- | --------------- | ---------------------------------- |
| **`CLAUDE.md`**      | 모든 대화 시작 시 | 개발자        | **낮음** (항상 포함)  | 프로젝트 개요, 기술 스택, 코딩 컨벤션 강제          |
| **`Skills`**         | 필요 시 자동 로드 | Claude     | **높음** (필요할 때만) | 복잡한 로직 자동화 (이미지 최적화, API 테스트 등)    |
| **`Slash Commands`** | 사용자가 직접 호출 | 개발자        | **높음** (호출 시만)  | `/commit`, `/deploy` 등 반복적인 작업 실행  |
| **`Plugins`**        | 설치 및 활성화 시 | 개발자        | **보통**          | 여러 기능(Hooks, Skills 등)을 패키지로 묶어 배포 |
- 저자는 Skill과 Slash command는 다른 의도로 설계되었다고 함. Skills은 클로드가 사용, Slash Command는 개발자가 사용.

- 다른 1등 하신분의 클로드 세팅을 참고하여 몇가지 스킬이나 커맨드 도입해봐도 좋을 거 같아요(https://github.com/affaan-m/everything-claude-code/blob/main/commands/tdd.md)


## 7 시스템 최적화와 자동화

### 7-1 시스템 프롬프트 슬림화

클코의 모든 행동은 시스템 프롬프트에 결정된다. 저자는 19K -> 10K로 줄였다고함

- 시스템 프롬프트?
	- 클코가 API 호출 마다 매번 보내는 시스템 프롬프트임.
	- 구성요소
		- Tool 정의: 각 도구마다 JSON Schema + 사용법 설명
		- 행동 지침: 파일 읽을 때 cat 쓰지말고 Read 도구 써라와 같은 규칙
		- 안전 경고
		- 예시: 도구 사용법 보여주는 예시
	- 사용자가 프롬프트에 1줄 입력하더라도 실제 API에는 수많은 토큰이 함께 전송된다.
		- 시스템 프롬프트 + 툴 정의 + 대화 히스토리(user, assistant..) + user의 프롬프트...
		- 매턴마다 시스템 프롬프트 + 도구 정의가 포함되는데
		- 대화가 길어질수록 히스토리도 쌓여서 토큰 소비가 늘어난다.
	- 왜 시스템 프롬프트를 줄여야하는가?
		- 더 많은 코드 파일과 대화 기록을 컨텍스트에 담을 수 있어서
			- 응답 속도 향상(처리할 토큰이 적음), 비용절감(토큰 사용량 감소)
	- 저자는 클코 설치 파일 자체를 수정하여 매 API마다 토큰 절약
		- 반복되는 IMPORTANT, CRITICAL 경고 제거
		- 도구 설명 장황한 예시 축약
		- 거의 안쓰는 도구(NotebookEdit)의 긴 설명 축소

- 이거는 잘 알고 고쳐야 되지 않을까?
	- 저자도 이건 전문가 영역이라 충분한 이해를 하고 수정하라고함. (잘못 수정하면 성능저하) 자기꺼 먼저 적용해보고 자신만의 패치를 사용하라는데..
	- 큰 문제 없다면 순정이 나을듯.


### 7-2 장시간 작업을 위한 수동 지수 백오프

- 대용량 처리, 복잡한 빌드와 같은 장시간 실행 작업시 매 초마다 상태 확인하면 비효율적임.
- 처음에는 짧은 간격으로, 점차 간격을 늘려가며 확인하는 전략(지수 백오프)를 사용
	- 1분 후, 2분 후, 4분 후, 8분 후
- 토큰 절약 + 다른 작업 병렬 가능하도록 해줌

```

> "대용량 CSV 파일(10GB)을 처리하는 스크립트를 실행해줘.                         지수 백오프 방식으로 진행 상황을 확인해줘.                            
처음 1분 후, 그 다음 2분 후, 4분 후, 8분 후 확인해."                                  
# Claude가 실행:                                                        
# 1. 스크립트 실행                                                       
# 2. 1분 대기 후 상태 확인                                                
# 3. 2분 대기 후 상태 확인                                                
# 4. 작업 완료 시까지 반복  
```

### 7-3 백그라운드에서 bash 명령 및 에이전트 실행

- cmd + B 로 백그라운드 보내기
	- 서브에이전트 백그라운드에서 실행해줘~
	- 병렬 서브에이전트 
		- 대규모 코드베이스 분석시 3개의 서브에이전트를 백그라운드 실행해줘~

### 7-4 자동화의 자동화

- 예제 참고(워크플로우 자동화해라 + 자동화여정 예시)

### 7-5 헤드리스 CI/CD

- 클로드 액션 리뷰 자동화

## 8 컨테이너와 샌드박스

## 9 브라우저 통합과 웹 자동화

## 10 실전 활용 사례

### 10-1 작성-테스트 사이클

- TDD 워크플로우
	- https://github.com/affaan-m/everything-claude-code/blob/main/agents/tdd-guide.md
	- https://github.com/affaan-m/everything-claude-code/blob/main/commands/tdd.md

### 10-2 자신만의 워크플로우

1. 음성 텍스트로 변환
2. 자동 커밋 명령어 \/commit
3. github actions 디버거: \/gha {url} 로 실패 원인 분석
4. HANDOFF.md 생성기: [handoff](https://github.com/ykdojo/claude-code-tips/blob/main/skills/handoff/SKILL.md) 로 컨텍스트 압축
	1. https://github.com/affaan-m/everything-claude-code/blob/main/commands/checkpoint.md

### 10-3 대화 기록 검색

- 과거 대화 기록 claude -r 로 검색하여 --resume
### 10-6 출력 검증 방법 마스터

- 테스트 코드 작성
- claude에게 자기 검증 요청. 저자는 모든 걸 다시 확인 후, __검증 가능한 것 / 불가능한 것 표로 만들어줘__ 프롬프트 추천
```
방금 생성한 코드 검토 및 검증. 끝에 검증 결과를 표로 정리해줘
```

### 10-9 TDD

- AI가 내 작업 테스트 못한다고 생각하지만, 실제로 가능. 테스트 작성시 다른 방식으로 문제를 생각하기 때문

### 10-10 복잡한 코드 단순화

- AI를 통해서만 코드 작성하면 이해할 수 없다 (X). 충분히 질문하면 오히려 더 빠르게 이해 가능.
	- `코드 복잡하고 왜 이렇게 작성했고, 더 간단하게 바꿀 수 있는 방법` (command를 만들어도 될듯)
	- https://github.com/affaan-m/everything-claude-code/blob/main/agents/refactor-cleaner.md
	- claude-mem

## TODO
- claude-mem
- refactor agent 
- handoff
- clone
- CLAUDE.md
- test(tdd)
- AI 가이드 위키에 모아두자.




















