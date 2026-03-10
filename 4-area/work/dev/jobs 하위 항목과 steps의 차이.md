---
created: 24-04-29
modified: 24-04-29
tags:
  - dev/actions
---

Jobs
- 작업의 단위
- 별개의 VM or container에서 실행
- workflow.yml에 정의되고 여러 steps를 포함
- Job끼리는 서로 독립적이고 서로 의존성이 없으면 동시에 실행될 수 있음.

Steps
- job을 구성하는 개별 task
- 각 Steps는 Job에서 실행될 action, command, script를 나타냄
- 병렬이 명시적으로 정의되어있지 않으면 step은 순차적 실행됨.
- 코드 체크아웃, 아티팩트 빌드, 앱 배포 등과 같이 수행해야할 작업 정의

작업은 수행할 전체 작업 단위 정의, 단계는 각업내의 개별 작업 또는 작업 정의.
