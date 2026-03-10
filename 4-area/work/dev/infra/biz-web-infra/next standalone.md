---
created: 24-07-21
modified: 24-07-21
tags:
---
https://nextjs.org/docs/pages/api-reference/next-config-js/output

## output

output file tracing을 활용하여 필요한 파일만 포함하여 빌드할 수 있다. 즉, 각 페이지와 종속성을 자동으로 추적하여 production 버전 배포시 필요한 모든 파일 결정.

## how it works

- @vercer/nft 를 사용하여 import, require, fs 사용을 정적으로 분석하여 페이지가 로드할 수 있는 모든 파일 결정.

## 자동으로 Traced files 카피

프로덕션 배포를 위해 필요한 파일만 복사하는 standalone 폴더 자동 생성.

- `.next/standalone` 폴더 생성. node_modules 설치 없이 배포 가능
- `next start` 대신 최소한의 `server.js` 파일 출력
- `public`, `.next/static` 폴더를 복사하지 않음. 이 건 CDN에서 처리하는게 이상적
- 하지만 위 폴더들을 `standalone/public`, `standalone/.next/static` 폴더에 수동으로 복사한 후 server.js파일 사용시 자동으로 이를 제공. 

> Good to know
> - next.config.js는 next build 중에 읽히고 server.js 출력 파일로 직렬화
> - 프로젝트가 특정 포트나 호스트 네임 필요한 경우 server.js 실행전 PORT, HOSTNAME 환경변수 정의 `PORT=8080 HOSTNAME=0.0.0.0 node server.js`

### 주의사항

- 모노레포 설정에서 추적 수행시, 프로젝트 디렉터리가 기본적으로 추적에 사용. 외부 파일 포함하는 옵션 있음
- outputFileTracingExcludes, outputFileTracingIncludes로 필요한 파일 포함 혹은 제외


### 실험적 turbo trace
- 종속성 추적은 매우 복잡한 계산과 분석을 필요로함. turbotrace


























