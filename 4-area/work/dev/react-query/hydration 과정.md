
1. getServersideProp
   -  qc = new QueryClient
   - run `qc.prefetchQuery`
   - return dehydrate(queryClient)
      - cache의 frozen representation을 만듬
      - 서버에서 클라로 prefetch된 쿼리를 전달할 때 유용
2. `_app` 
   - `<Hydrate state={pageProps.dehydratedState} />`
      - 이전에 dehydrated state를 cache에 추가한다.
      - dehydration에 포함된 쿼리가 queryCache에 이미 있는 경우, `hydrate` 는 덮어쓰지 않고 자동으로 삭제됩니다. 
3. 