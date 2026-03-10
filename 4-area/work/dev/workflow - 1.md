---
created: 24-04-18
modified: 24-04-18
tags:
  - dev/actions
---
https://docs.github.com/ko/actions/using-workflows/about-workflows

- runs-on: ubuntu-latest
	- github에 의해 호스트되는 virtual machine에서 job을 수행하겠음

- 
### [종속 작업 만들기](https://docs.github.com/ko/actions/using-workflows/about-workflows#creating-dependent-jobs)

- 워크플로의 작업은 모두 병렬 실행. 다른 작업이 완료된 후에만 실행해야할 경우 `needs` 키워드를 사용하여 종속성 만들 수 있음.

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - run: ./setup_server.sh
  build:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - run: ./build_server.sh
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: ./test_server.sh
```
### 종속성 캐싱

- 종속성을 정기적으로 다시 사용하는 경우 이 파일을 캐싱하여 성능 개선할 수 있음 캐시가 만들어지면 동일 레포의 모든 워크플로에서 재사용 가능.

```yaml
jobs:
  example-job:
    steps:
      - name: Cache node modules
        uses: actions/cache@v3
        env:
          cache-name: cache-node-modules
        with:
          path: ~/.npm
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-build-${{ env.cache-name }}-
```

## [워크플로 재사용](https://docs.github.com/ko/actions/using-workflows/reusing-workflows)

103e290d52287bc98fef6833a6047f0819953ecd183f4bc10b603e18b877cab5



- 보이