
# 7장 함수표현식

함수표현식을 사용하면 다른 함수에서 사용할 수 있도록 함수를 반환하는 형태도 가능합니다. 


```js
function createComparisonFaunction( propertyName ){
	return function(obj1, obj2){
		var v1 = object1[propertyName];
		var v2 = object2[propertyName];
		
		if(v1 < v2) {
			return -1;
		}else if( v1 > v2 ){
			return 1;
		}else {
			return 0;
		}
	};
}

```
createComparisionFunction()은 익명함수를 반환.


##1. 재귀 
: 재귀함수는 일반적으로 다음 예제처럼 함수가 자기 자신을 이름으로 호출하는 형태 


```js
function factorial(num){
	if( num <=1 ){
		return 1;
	}else {
		return num* functorial(num -1);
	}
}

var anotherFactorial = factorial;
factorial = null;
console.log(anotherFactorial(4) );

```
함수 동작하지 않음.
더이상 함수가 아닌 factorial()을 실행하려 하기 때문

 arguments.callee를 쓰면 이문제를 예방함.



 ```js
 function factorial(num){
	if( num <=1 ){
		return 1;
	}else {
		return num* arguments.callee(num -1);
	}
 }
 
 ```

 재귀함수를 사용할때는 항상 함수 이름대신 arguments.callee 쓰기를 권장 

 스트릭모드에서는 arguments.callee값에 접근할수 없음. 
 대신 이름붙은 함수 표현식을 써서 같은 결과를 나태낼수있음


 ```js
 var factorial = (function f(num){
 	if( num <=1 ){
 		return 1;
 	}else {
 		return num * f(num -1);
 	}
 });
 
 ```
f()를 생성해서 변수 factorial에 할당. 



##2. 클로저 
클로저란 다른 함수의 스코프에 잇는 변수에 접근 가능한 함수. 



```js

function createComparisionFunction ( propertyName){
	return function(object1, object2){
		var value1 = object1[propertyName];
		var value2 = object2[propertyName];


		if(value1 < value2) {
			return -1;
		}else if( value1 > value2){
			return 1;

		} else {
			return 0;
		}
	}
}

```
클로저를 잘 이해하기 위해서는 스코프 체인이 어떻게 생성되고 사용되는지 자세히 알아야 한다.

함수를 호출하면 실행 컨텍스트와 스코프 체인이 생성된다.
함수의 활성화 객체는 arguments및 이름 붙은 매개변수로 초기화됨. 
함수를 실행하면 값을 읽거나 쓸 변수를 스코프 체인에서 검색 

```js
	function compare(value1, value2){
		if(value1 < value2){
		return -1;
	}else if( value1 > value2){
		return 1;
	}else{
		return 0;
	}
}
var result = compare(5, 10);

``

다른 함수의 내부에 존재하는 함수는 외부함수의 활성화 객체를 자신의 스코프 체인에 추가합니다.

따라서 createComparisionFunction() 외부 함수 
내부에 있는 익명함수의 스코프 체인에는 외부 함수의 활성화 객체를 가리키는 포인터가 존재. 

외부함수가 실행을 마치고  익명 함수를 반환하면 익명 함수의 스코프 체인은 
외부 함수의 활성화 객체와 전역변수 객체를 포함하도록 초기화 된다.

이때문에 익명함수는 외부 함수의 변수 전체에 접근할 수 있게 됩니다.
외부함수가 실행을 마쳤는데도 활성화 객체가 파괴되지 않는 점인데 아직 익명함수의 스코프 체인에서 이를 참조하기 때문에 

외부함수가 실행을 마치면 실행 컨텍스트의 스코프 체인은 파 괴 되지만 활성화 객체는 익명 함수가 파괴될때까지 메모리에 남습니다 .



클로저는 외부함수의 스코프를 보관해야 하므로 다른 함수에 비해 메모리를 많이 요규함.
클로저를 과용하면 메모리 문제가 생길 수 있으니 반드시 필요할때만 사용


### 2.1 클로저와 변수
클로저는 항상 외부 함수의 변수에 마지막으로 저장된 값만 알수 있음.
클로저가 특정 변수가 아니라 전체 변수 객체에 대한 참조를 저장함.

```js
	function createFunctions(){
		var result = new Array();

		for( var i=0; i< 10; i++){
			result[i] = function(){
				return i;
			};
		}
		return result;
}
```

결과는 모든 함수가 10을 반환.
모든 함수가 스코프 체인에 createFunctions()의 활성화 객체를 포함하므로 이들은 모두 같은 변수, 즉 i를 참조 createFunctions()이 실행을 마치면 i에 10이 저장됨.
모든 함수가 같은 변수 객체를 참조하고 그 변수 객체에서 i 값은 10이므로 모든 함수에서는 i는 10

**익명 함수를 만들어 즉시실행  클로함수로 변경

```js
	function createFunctions(){
		var result = new Array();

		for( var i=0; i<10; i++){
			result[i] = function(num){
				return function(){
					return num;
				};

			}(i);
		}
		return result;
}
```
각 함수가 다른 숫자를 반환.

익명함수는 매개변수로 num 하나만 받는데 이는 결과 함수가 반환할 숫자.
변수 i 를 익명함수에 매개변수로 넘김.
함수 매개변수는 값 형태로 전달되므로 i의 현재 값을 매개변수 num에 복사.
익명함수는 num을 간직한 클로저를 생성하여 반환.
이제 result 배열에 들어 있는 각 함수는 고유한 num값을 가지면 그값을 반환함.



###2.2 this 객체 
```js
var name = 'the window';
var object = {
	name : 'my Object',
	getNameFunc : function(){
		return function(){
			return this.name;
		};
	}
};
console.log(object.getNameFunc()());  // 'the window' ]
```

```js
	
	var name = 'the window';

	var object = {
		name : 'my object',
		getNameFunc : function(){
			var that = this;
			return function(){
				return that.name;
			};
		}
	};
	console.log(object.getNameFunc()()); // my Object
```
익명 함수를 정의 하기 전에 외부 함수의 this객체를 that이란 변수에 할당.
이제 클로저를 정의하면 that은 외부함수에 고유한 변수이므로 클로저는 이 변수에 접근 가능

함수가 반환된 뒤에도 that은 여전히 object에 묶여 있으므로 object.getNameFunc()()을 호출하면 'my object' 반환 

###2.3 메모리 누수
클로저는 ie9 이전 버전에서 메모리 문제를 일으킴.
가비지 컬렉션 방법이 다르기 때문에... 

익명함수는 assignHandler()함수의 활성화 객체에 대한 참조를 유지. 


```js

	function assignHandler(){
		var element = documnet.getelementById('someElement');
		var id = element.id;

		element.onclick = function(){
			console.log(id);
		};
		element = null;
	}
```


## 3. 블록스코프 만들기 흉내내기

```js
	function outputNumber(count){
		for(var i=0; i< count; i++){
		console.log(i);
	}
	console.log(i); //count
}
```


## 4.고유변수
### 4.1 정적고유변수
### 4.2 모듈패턴 

더글러스 크록포드가 고안한 모듈패턴은 싱글톤에서 같은 일을 함.
싱글톤이란 인스턴스를 단 하나만 갖게 의도한 객체.

```js
	var singleton = {
		name :value,
		method : function(){
		//메서드 코드
		}
	}
```

```js
	
	var singleton = function(){
		// 고유변수와 함수
		var provateVariable = 10;

		function provateFunction(){
			return false;
		}

		// 특권 / 공용 메서드와 프로퍼티
		return {
			publicProperty :true,
			publicMethod : function(){
				privateVariable++;
				return privateFunction();
			}
		};
	}();
```
모듈 패턴은 객체를 반환하는 익명 함수를 사용.
익명 함수 내부에서는 첫번째로 고유변수와 함수를 정의 

그다음 객체 리터럴을 함수값으로 반환.
반환된 객체 리터럴에는 공용이 될 프로퍼티와 메서드만 들어 있음.

객체는 익명 함수 내에서 정의 되었으므로 공용 메서드는 모두 고유 변수와 함수에 접근 가능함. 

객체 리터럴이 싱글톤에 대한 공용 인ㅌ터페이스를 정의 하는것 

```js

	var application = function(){
		// 고유 변수화 함수.
		var components = [];
		
		// 초기화
		components.push(new BaseComponent());

		//공용 인터페이스
		return {
			getComponentCount : function(){
				return components.lengt;
			},
			registerComponent : function(){
				if(typeof component == 'object'){
					components.push(component);
				}
			}
		};
	}();
```
모듈 패턴은 단하나의 객체를 반드시 생성하고 몇가지 데이터를 가지며 또한 고유 데이터에 접근 가능한 공용 메서드를 외붕에 노출하도록 초기화 해야 할때 유용 


### 4.3 모듈 확장 패턴
객체를 반환하기 전에 확장하는 패턴. 
이 패턴은 싱글톤 객체가 특정 타입의 인스턴스지만 프로퍼티나 메서드를 추가하여 확장해야 할때 유용. 

```js
	var singleton = function(){
	// 고유 변수와 함수.
	var privateVariable = 10;

	function privateFunction(){
		return false;
	}

	// 객체생성
	var object = new CustomType();

	//특권.공용 프로퍼티와 메서드추가
	object.publicProperty = true;
	object.publicMethod = function(){
		privateVariable++;
		return privateFunction();
	};

	reutrn object;
}();
```



