
[[ssr & next.js]]

- csr에서 getServersideProps를 거쳐서 \_app 을 거친다음에  새로운 queryClient를 생성
	- getServersideProps에서 새로운 queryClient생성은 맞음
		- 아예 queryClient가 hydrate될 때 교체되나 했는데 검색 결과가 안없어짐
	- 희안한건 hydrate 전에 infinite 데이터가 없어져있음.