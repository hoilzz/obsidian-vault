---
created: 25-10-17
modified: 25-10-17
tags:
---
https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview#before-prompt-engineering

## summary

### .claude 최적화
- **강조 표현 적극 사용**: `IMPORTANT`, `YOU MUST` 등으로 지시사항 준수율 향상
- **반복적 개선 필수**: 계속 실험하고 효과적인 표현 찾기
- Prompt improver로 가끔 최적화
### Allow List & GH CLI
- Always allow 설정으로 편의성 향상
- gh CLI 설치로 코드 리뷰(코드래빗, 사용자 피드백) 반영 자동화

### 커스텀 슬래시 명령어

- `.claude/commands`에 반복 워크플로우를 마크다운으로 저장
- `$ARGUMENTS`로 동적 매개변수 전달

### 복잡한 워크플로우 = 체크리스트

코드 마이그레이션, 빌드 스크립트 등 복잡한 작업 시:

- **Markdown 체크리스트를 스크래치패드로 활용**
- 각 항목을 순차적으로 처리하며 체크

# memo
## Customiza setup
### .claude 최적화
- 가끔 prompt improver를 넣어 최적화한다.
- __강조 표현 추가__: 지시사항 준수율을 높이기 위해 강조 표현 자주 사용
	- __IMPORTATNT__, __YOU MUST__
- 반복적 개선이 중요.
- 어떤 표현과 형식이 효과적인지 실험 정신 중요
### allow list
- always allow 하면 편할듯.
### gh cli
- 이거 설치해서 코드래빗 혹은 사용자 리뷰 반영하는데 잘 쓰일듯.

## give claude more tools

### 커스텀 슬래시 명령어

반복되는 워크플로우(디버깅 루프)의 경우 .claude/commands 의 마크다운 파일로 저장할 수 있음. 

#### $ARGUMENTS 키워드

- 커스텀 슬래시 명령어에 매개변수 전달하기
```
Please analyze and fix the GitHub issue: $ARGUMENTS.

Follow these steps:

1. Use `gh issue view` to get the issue details
2. Understand the problem described in the issue
3. Search the codebase for relevant files
4. Implement the necessary changes to fix the issue
5. Write and run tests to verify the fix
6. Ensure code passes linting and type checking
7. Create a descriptive commit message
8. Push and create a PR

Remember to use the GitHub CLI (`gh`) for all GitHub-related tasks.
```

## try common workflow

### 1. explore, plan, code, commit

1. 탐색
	- 코드 작성하지 말라고 명시적 지시. 
	- 복잡한 문제의 경우 서브에이전트를 활용하여 세부사항 검증. 효율성 손실 없이 컨텍스트 가용성 보존
	- _근데 서브에이전트를 내가 만들어야 하는거아님?_
2. 계획
	- 특정 문제에 접근하는 방법에 대한 플랜 세우도록 요청
	- __think__ 를 사용하여 확장 사고 모드 트리거
		- 사고예산 레벨: think < think hard < think harder < ultrathink
3. 코딩
4. 커밋


### 2. write tests, commit; code, iterate, commit

- 단위, 통합, e2e 테스트로 검증 후 수정을 iteration 

### __3. write code, screentshot result, iterate__

__이걸로 figma -> 디자인 컴포넌트__ 활용하자
1. 스샷 도구 제공
	- Puppeteer MCP server
	- 수동으로 스샷 복붙
2. 시각적 목업 제공
	- 이미지 복붙, DND, 이미지 파일 경로
3. 구현 및 반복
	- 클로드에게 다음을 요청
		- 디자인을 코드로 구현
		- 결과 스샷 촬영
		- __결과가 목업과 일치할 때 까지 반복__
- 반복의 중요성
	- 인간과 마찬가지로 클로드 출력도 반복을 통해 크게 향상됨.

__2,3번 방식 모두 명확한 목표 / 반복적 개선 이라는 핵심 원칙 __

### 4. safe YOLO mode

- `claude --dangerously-skip-permissions`
	- 린트 오류 수정, 보일러플레이트 코드 생성
## optimize workflow

### 지시사항 구체적으로

### 조기에 그리고 자주 방향 수정하기

#### 1. 코딩 전에 계획 수립 요청

Claude에게 코딩하기 전에 계획을 세우도록 요청합니다. 계획이 좋아 보인다고 확인할 때까지 코드를 작성하지 말라고 명시적으로 지시하세요.

**사용 예시:**

```
"먼저 이 문제를 어떻게 해결할지 계획을 세워줘. 
계획을 확인하기 전까지는 코드를 작성하지 마."
```

#### escape으로 중단하기

얘가 이상한 방향으로 가면 중단하여 조기개입.

### __복잡한 워크플로우에 체크리스트와 스크래치 패드 사용하기__

- 코드 마이그레이션, 복잡한 빌드 스크립트 실행과 같을 때 claude에게 md 파일을 체크리스트와 작업스크래치패드로 사용하도록 하여 성능 향상시키기 가능

#### 예시1. 대량 린트 이슈 수정
- 체크리스트 생성



