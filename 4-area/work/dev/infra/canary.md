---
created: 25-02-21
modified: 25-02-21
tags:
---
새로운 버전의 애플리케이션을 일부 사용자에게 먼저 배포. 문제 없으면 점진적으로 전체 트래픽 확장.

```yaml
// 까나리 배포를 위한 리소스임을 의미, 해당 어노테이션 인그레스는 까나리 트래픽 받음
nginx.ingress.kubernetes.io/canary: "true"  
// 아래 쿠키값 존재시 카나리 배포된 서비스로 트래픽 라우팅
nginx.ingress.kubernetes.io/canary-by-cookie: "_kpwca"
```
