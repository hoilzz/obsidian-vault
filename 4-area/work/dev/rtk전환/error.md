## urql의 networkError, graphqlError 구분방법

```ts

// src/utils/result.ts

export const makeErrorResult = (
  operation: Operation,
  error: Error,
  response?: any
): OperationResult => ({
  operation,
  data: undefined,
  error: new CombinedError({
    networkError: error,
    response,
  }),
  extensions: undefined,
});


// fetchSource.ts

Promise.resolve()
      .then(() => {
        if (ended) return;
        return (fetcher || fetch)(url, fetchOptions);
      })
      .then((_response: Response | void) => {
        if (!_response) return;
        response = _response;
        statusNotOk = response.status < 200 || response.status >= maxStatus;
        return executeIncrementalFetch(next, operation, response);
      })
      .then(complete)
      .catch((error: Error) => {
        if (hasResults) {
          throw error;
        }
		// 요기서 쓰인다! 즉, 네트워크 에러는 catch문에서 잡힌 에러로 data, extensions 없음
        const result = makeErrorResult(
          operation,
          statusNotOk
            ? response.statusText
              ? new Error(response.statusText)
              : error
            : error,
          response
        );

        next(result);
        complete();
      });

```






## operation errors 처리하기 feat. gql

https://www.apollographql.com/docs/react/data/error-handling
리모트 서버에서 GQL oepration 실행할 때 GQL errors 나 network error 만들 수 있음

#### GQL errors

- GQL operation의 serverside 실행과 연관된 에러임.
	- syntax error
	- validation error
	- resolver error

- GQL error 발생하면 `erros` 배열을 포함할거임.
- GQL error로 Apollo server가 operation 실행이 안된다면  `4xx` status code로 응답할거임
	- 리졸버에서 에러가 발생했지만 응답에 일부 데이터가 포함된 경우 Apollo Server는 `200` 코드로 응답할거임.

#### Network errors

GQL server와 통신을 시도하는 동안 발생하는 오류임. 보통 4xx or 5xx 상태 코드가 발생함.

network error가 발생한다면, Apollo Client는 useQuery 호출에서 리턴된 `error.networkError` 필드를 리턴할거임.


## RTK query error 타입 문제

https://redux-toolkit.js.org/rtk-query/usage-with-typescript#type-safe-error-handling

base Query에서 에러가 제공될 때, RTK Query는 에러를 직접 제공할거임.

































