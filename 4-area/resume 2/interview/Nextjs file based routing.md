---
created: 25-05-22
tags:
  - "#25/6-3"
done: true
wip: false
---
## Summary

- 파일 구조 기반 라우팅은 직관적이다. 
	- 코드 스플리팅이 알아서 된다.
	- colocation: 관련 UI 로직이 같은 폴더에 있어 유지보수 용이
- 앱라우터 왜나옴?
	- loading, layout, error 등 자주쓰던 얘들 추상화 잘해놓음
	- RSC 쓸 수 있음.


### 1. **라우팅이란?**
- 클라이언트 요청 URL(path)에 따라 어떤 화면 혹은 데이터를 응답할지 결정하는 **path 셀렉팅 프로세스**.
- CSR과 SSR 모두 라우팅이 있지만, 구조와 처리 방식이 다름.
    
---
### 2. **CSR vs SSR 관점에서 라우팅**

| 구분     | CSR (Client Side Rendering)      | SSR (Server Side Rendering) |
| ------ | -------------------------------- | --------------------------- |
| 초기 응답  | `index.html` + JS 번들 하나          | 요청된 path에 따라 HTML 직접 응답     |
| 페이지 구조 | 대부분 SPA(single endpoint)         | 여러 path 대응 (multi-endpoint) |
| 라우팅 처리 | 브라우저에서 JS로 처리 (ex: react-router) | 서버에서 path별 처리               |
| 번들     | 하나의 큰 JS 번들                      | path 단위 코드 스플리팅 가능          |
✅ **CSR도 여러 path로 구성 가능하지만**, 결국 서버는 하나의 entry(`index.html`)만 응답하며, 클라이언트 라우터가 내부적으로 path를 분기함. 반면, SSR은 요청마다 **서버가 직접 HTML을 응답**하기 때문에 path별 처리가 핵심이 됨.


### 3. **라우팅 방식 종류**
#### 1) 명시적 라우팅
- 대표: `react-router`, `vue-router`
- `path`와 `컴포넌트`를 직접 매핑
- ❓ **SPA는 endpoint가 하나인데 서브 path들은 어떻게 처리되나?**
    - 정답: 서버가 모든 경로 요청을 `index.html`로 리디렉션해주고, 클라이언트 라우터가 이를 해석함
    - → **Nginx, S3, CloudFront 등에서 rewrite 설정 필요**
        
#### 2) 코드 기반 라우팅
- 대표: `express`, `koa`, `nestjs` 등
- `app.get('/about', ...)`처럼 직접 경로 코딩
    
#### 3) 데코레이터 기반
- 대표: `NestJS`, 일부 `Next.js 서버 액션`
- 라우팅 메타데이터를 주입해 구조적으로 라우팅
    
#### 4) **파일 기반 라우팅**
- 대표: **Next.js**
    - `pages` 디렉토리: 파일명 자체가 경로 (직관적)
        - ex: `about.jsx` → `/about`
    - `app` 디렉토리 (App Router):
        - 폴더/파일 구조: `about/page.tsx`, `about/loading.tsx`, `about/layout.tsx`
        - ⬆️ **라우팅 + 상태별 UI를 함께 선언 가능**
        - `layout`, `loading`, `error`, `template` 등 자주 쓰이는 UI 패턴을 **구조화/추상화**하여 관리 용이
            
---
### 4. **App Router 도입 이유 (Next.js 기준)**

- **코드 스플리팅 최적화**: 폴더 기반 path 구분 덕분에 자동 코드 분할
- **Colocation**: 관련 UI/로직이 같은 폴더에 있어 유지보수 용이
- **UI 상태 컴포넌트 분리**: `layout.tsx`, `loading.tsx` 등 도입으로 **복잡한 UI 상태 분기 처리 용이**
- **Server Component 지원**: `app` 구조는 RSC(React Server Components) 기반 SSR 아키텍처에 더 적합

---

## 💡 면접 포인트 요약

- **CSR의 라우팅은 클라이언트에서만 처리되며, SPA 특성상 서버는 모든 요청을 index.html로 응답하도록 구성되어야 한다.**
- **SSR은 서버에서 요청 경로마다 직접 HTML을 렌더링하므로, 라우팅이 핵심 기능이다.**
- **Next.js는 파일 기반 라우팅을 제공하고, 특히 App Router는 더 구조적인 라우팅과 컴포넌트 분리를 가능하게 하여 개발자 경험을 개선한다.**
- **App Router의 가장 큰 강점은 UI 상태별 처리 구조화 + 코드 스플리팅 + colocation 구조로 인한 유지보수성 향상이다.**