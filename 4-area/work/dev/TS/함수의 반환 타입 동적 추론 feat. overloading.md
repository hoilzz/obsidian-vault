---
created: 24-12-10
modified: 24-12-10
tags:
---
```
interface A {
	type: 'A';
	value: string;
}

interface B {
	type: 'B';
	value: number;
}

// 함수 오버로드
function myFunction(condition: true): B;
function myFunction(condition: false): A;
function myFunction(condition: undefined): A;
function myFunction(condition?: boolean): A | B;

// 함수 구현
function myFunction(condition?: boolean): A | B {
	if (condition) {
		return {type: 'B', value: 42}; // B 타입 객체
	}
	return {type: 'A', value: 'Hello, World!'}; // A 타입 객체
}

// 호출 예시
const result1 = myFunction(true); // B 타입으로 추론
const result2 = myFunction(false); // A 타입으로 추론
const result3 = myFunction(); // A 타입으로 추론
```