# 3장 언어의 기초

## 1. 문법 
### 1.1 대소문자 구분
### 1.2 식별자
식별자는 변수나, 함수, 프로퍼티, 함수 매개 변수의 이름 
한개이상의 문자로 표기해야함.


- 첫번째 문자는 반드시 글자나 밑줄 , 달러기호 중 하나 
- 다른 문자에는 글자, 밑줄, 달러기호 , 숫자를 자유롭게 쓸수있음.

관습적으로 카멜케이스로 씀. 첫번째 글자는 소문자 단어가 바뀔때 대문자로 쓰는 표기법

	firstSecond 


### 1.3 주석
// 한줄주석 
/* */ 블록주석

### 1.4 스트릭트 모드
'use strict';
자바스크립트가 실행되는 방식을 많이 바꿈.
안전하지 않은 동작에는 에러를 반환.
ie10+


### 1.5 문장.
문장은 세미콜론으로 종료 

	var diff = a- b ;

##  2.키워드와 예약어 
키워드는 식별자나 프로퍼티 이름에 쓸수없음. 
break , do , instanceof ,typeof, case , else, new , var , catch
,finmally, with, in, try, throw, debugger , .... this 

예약어는 식별자나 프로퍼티 이름에 쓸수 없음.
특별한 쓰임새가 없지만 미래에 키워드로 쓸 가능성이 있으므로 예약해 둔것 . 



## 3. 변수 

특정값에대한 이름일뿐.

	var message = 'hi!';
	키워드 식별자. 

함수 안에서 var 키워드를 써서 변수를 정의하면 해당변수는 함수종료시 즉시 파괴됨.
	function test(){
		var message = 'hi'; //전역변수
	}
	test();
	console.log(message); // 에러 

var 연산자를 생략 하면 변수를 전역으로 정의할수 있음.


## 4. 데이터타입

원시 : undefined, Null,  Boolean, 숫자, 문자열 
객체 (이름 -  쌍 값 쌍의 순서 없는 목록)

### 4.1 typeof 연산자
데이터 타입을 알 수 있음. 
함수를 제외한 객체 또는 Null : 'object'
함수 : function

	var message = 'some thing';
	console.log(typeof message); //string
	console.log(typeof(message));  // string
	console.log(typeof 96); // number


### 4.2 undefined 타입 
var 를 써서 변수를 정의했지만 초기화 하지 않으면 undefined 할당

	var message;
	console.log(message == undefined); //  true


### 4.3 Null 타입 
빈객체를 가리키는 포인터 
	var car = null;
	console.log(typeof car); // object


	console.log( null == undefined ); // true
연산자와 피연산자를 비교할 경우 암시적으로 형 변환을 함.



### 4.4 불리언 타입

	var msg = 'hello';
	var msgBoolean = Boolean(msg);
모든 타입을 불리언 값으로 표현할수있음. 

true로 변환되는 값. 
true , 비어있지않은 문자열모두 , 0이 아닌 모든 숫자, 모든 객체, 

false로 변환되는값 
false, "" 빈문자열, 0, NaN, null, undefined


### 4.5 숫자타입

 - NaN :  숫자를 반환할 것으로 의도한 조작이 실패 했을때 반환되는 값 
 NaN 은 어떤 값과 일치하지 않음.

 	console.log(NaN == NaN); // false

  isNaN()함수. 
	: 해당 값이 숫자가 아닌값인지 검사.

	console.log(isNaN(NaN));  // true
	console.log(isNaN(10));  // false
