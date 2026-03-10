---
created: 25-12-30
tags: 
done: 
wip: true
---
현재 문제점 (Variables 미사용 시)

### 1. AI가 색상값을 추측해야 함

  // Figma에서 받은 정보
  "테마/라이트/35 #E0E0E0": "#E0E0E0"

  // AI가 해야 하는 일: "이게 el-35인가? el-30인가?"
  className="bg-el-35"  // 추측 기반 매핑

  Variables가 있으면:
  // Figma Variable 이름이 코드 토큰과 일치
  Variable: "el-35" → 바로 "bg-el-35" 사용


### 2. 하드코딩 발생

  방금 구현에서 이런 코드가 생겼습니다:

  // 고정 색상 - 토큰이 없어서 하드코딩
  className="bg-[#FFD618]"

  Variables로 관리하면:
  // Figma: Variable "fixed-yellow" = #FFD618
  // 코드: 토큰 "fixed-yellow" 추가
  className="bg-fixed-yellow"

### 3. 폰트 토큰 매핑 오류 가능성

  Figma 텍스트 스타일: "미디움2 B"
            ↓
  AI 추측: "font-medium2-bold" 인가? "font-md2-bold" 인가?

-  Variables 도입 시 AI 자동화 이점

  | 현재 (Variables 없음)   | 도입 후                         | 
  |-------------------------|---------------------------------|
  | \#E0E0E0 → el-35 추측    | Variable명 el-35 → 바로 사용    |
  | 색상 하드코딩 발생      | 토큰명으로 자동 변환            |
  | 폰트 스타일명 수동 매핑 | 1:1 매칭으로 자동 변환          |
  | 다크모드 토큰 추측      | 모드별 값 자동 인식             |
  | 검증 불가               | "이 토큰이 유효한가?" 체크 가능  |

- Before (현재)

>   Figma 분석 → 색상값 #E0E0E0 발견
           → colorTheme.js 검색
           → "el-35가 맞나?" 확인
           → 적용

- 토큰 하나당 검색/확인 필요
- 매핑 오류 가능성

  - After (Variables 사용)

  > Figma 분석 → Variable "el-35" 발견
           → 바로 "bg-el-35" 적용

- 즉시 매핑
- 100% 정확

이외에

- 디자인 시스템 일관성 - 토큰 외 사용 방지
- 색상/폰트 AI 자동화













