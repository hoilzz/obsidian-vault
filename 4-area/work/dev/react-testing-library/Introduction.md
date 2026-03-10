
### problem

테스트는 컴포넌트의 구현 세부사항을 포함하지 않고, 테스트가 의도한 대로 확신을 줄 수 있도록 하는데 집중.


### solution

[소프트웨어 사용 방식과 테스트는 닮아야한다. 그래야 더 확신을 가질 수 있다.](https://testing-library.com/docs/guiding-principles/)

렌더된 react component의 인스턴스를 처리하는 것보다 실제 DOM으로 동작하는게 낫다. 
해당 라이브러리에는 DOM을 쿼리할 수 있는 장치가 있다.
- label text로 form element 찾기 (유저가 하듯이)
- link 찾기
- 텍스트로 버튼 찾기
- `data-testid`로 엘리먼트 찾기
	- text content와 label이 없거나 실용적이지 않을 때 사용

## example

### quickstart
```
// arrange
// act
// assert
```
### full example
```
import msw ...

test('loads and display greeting', ..)
test('handle server error', ..)

```































