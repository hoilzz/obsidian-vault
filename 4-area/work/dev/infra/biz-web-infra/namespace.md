---
created: 24-07-14
modified: 24-07-14
tags:
---

- 클러스터 내에서 리소스를 논리적으로 구분하는 방법
- 클러스터는 물리적 또는 가상 머신의 집합으로 구성된 하나의 큰 리소스 풀.
- 네임스페이스는 그 안에서 자원을 구분하고 격리

- 클러스터는 큰 빌딩이고, 네임스페이스는 빌딩 내의 사무실 공간
	- 사무실 공간에서는 해당 공간을 사용하는 팀이나 프로젝트가 자원을 독립적으로 관리
- 실질적인 사용 예
	- 개발, 테스트, 프로덕션 환경 분리
```yaml
apiVersion: v1 
kind: Namespace 
metadata: 
	name: development 
--- 
apiVersion: v1 
kind: Namespace 
metadata: 
  name: production
```