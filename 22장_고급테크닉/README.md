# 22장 고급 테크닉

## 1 고급함수.
### 1.1  안전한 타입탐지
Object의 네이티브 toString() 메서드는 어떤 값에서도 호출할수 있으며.
[Object NaticeConstructorName] 형식의 문자를 반환한다.

``js
	console.log(Object.prototype.toString.call(value) ); // '[object Array]'
```
배열을 어떤 전역 컨텍스트에서 생성했든 네이티브 생성자 이름은 항상 같으므로  toString()이 반환하는 값도 일정.

이러한 점에 착안하여 다음과 같은 함수를 만들수 있음.

```js
	function isArray(value){
		return Object.prototype.toString.call(value) = '[object Array]';
	}
```

같은 방법을 써서 값이 네이티브 함수인지 정규표현식인지도 판단가능.
```js
	function isFunction(value){
		return Object.prototype.toString.call(value) === "[object Function]";
	}
	function isRegExp(value){
		return Object.prototype.toString.call(value) === "[object RegExp]";
	}

``

Object의 toString() 메서드는 네이티브 아닌 생성자의 생성자 이름을 알 수 없으므로 개발자가 정의한 객체 인스턴스에서 호출하면 "[object Object]"를 반환한다.


### 1.2 스코프 확인 생성자.
생성자는 단순히 new 연산자로 호출하는 함수일 뿐임을 기억.
이런방식으로 사용할때 생성자의 this객체는 다음과 같이 새로 생성된
객체 인스턴스를 가리킵니다.

```js
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
	}

	var person = new Person('nicholas', 29, 'engineer');
```

new 생성자 없이 생성자를 호출하면 문제가 발생.
this 객체는 런타임에서 해당 객체에 묶이므로 Person)을
직접 호출하면 this가 전역 객체에 묶이게 됩니다.

```js
	var person = Person('Nicholas' , 29, 'engineer');
	console.log(window.name);; // 'Nicholas';
```

여기서 new 연산자를 생략하면 Person인스턴스의 프로퍼티여야 할 데이터가 window객체에 묶임.
이 문제는 this객체의 늦은 바인딩 때문에 생긴것.

해결책은 **스코프 확인 생성자**
스코프 확인 생성자는 실행에 앞서 먼저 this 객체가
정확한 타입의 인스턴스인지를 확인해야 한다.
그렇지 않다면 새인스턴스를 생성하여 반환.

```js
	function Person(name, age, job){
		if(this instanceof Person){
			this.name = name;
			this.age = age;
			this.job = job;
		}else {
			return new Person(name, age, job);
		}
	}

	var person1 = Person('nic', 29 , 'engineer');
	console.log(window.name); // ""
	console.log(person1.name); // 'nic'

	var person2 = new Person('shelby', 34, 'Ergonomist');
	console.log(person2.name); 'shelby'

```

위 코드의 Person 생성자에는 if문을 추가해서 this객체가 Person의 인스턴스인지 확인하고, 그렇지 않다면 현재 컨텍스트에서
new 연산자와 함께 자신을 재귀 호출하여 Person 인스턴스를 반환.
생성자를 이렇게 만들면 실수로 new를 쓰지않더라도 객체를 올바르게 초기화 할수있음.

스코프 확인 생성자에는 주의 할 점이있음.
이 패턴을 사용하면 생성자를 호출하는 컨텍스트가 그대로 장긴다.
생성자 훌치기 패턴을 사용하여 상속하면서 프로토타입 체인을
같이 쓰지 않으면 상속이 깨질수 있음.

```js
	function Polygon(sides){
		if(this instanceof Polygon){
			this.sides = sides;
			this.getArea = function(){
				return 0;
			}
		}else {
			return new Plygon(sides);
		}
	}

	function Reacangle(width, height){
		Polygon.call(this, 2);
		this.width = width;
		this.height = height;
		this.getArea = function(){
			return this.width * this.height;
		};
	}

	Rectangle.prototype = new Polygon();

	var rect = new Rectangle(5, 10);
	console.log(rect.sildes); //2

```


### 1.3 지연 로딩 함수.

### 1.4 함수 바인딩
함수 바인딩이란 특정한 this값과 특정한 매개변수를 넘기면서
다른 함수를 호출하는 함수이다.
이 테크닉은 보통 콜백 및 이벤트 핸들러와 함께 사용해서 코드 실행 컨텍스트를 유지하면서도 함수를 변수처럼 전달하는 데 쓰임.

```js
	var handler = {
		message : "Event handled",
		handleClick : function(event){
			console.log(this.message);
		}
	};

	var btn = document.getElementById("my-btn");
	EventUtil.addHandler( btn, 'click', handler.handleClick);

```
위 예제는 handler라는 객체를 생성.
handler.handleClick() 메서드는 dom 버튼의 이벤트 핸들러로 등록됨.
언뜻 보기엔 콘솔에 "Event handled" 가 표시될것 같지만 사실은 'undefined'가 표시됨.
문제는 대부분 브라우저에서 handler.handleClick()의 컨텍스트가 저장되지 않으므로 this객체는 handler가 아니라
DOM 버튼을 가리킨다는 점.

```js
	var handler = {
		message: "Event handled",
		handleClick : function(event){
			console.log(this.message);
		}
	};

	var btn = document.getElementById("my-btn");
	EventUtil.addHandler(btn, 'click', function(event){
		handler.handleClick(event);
	});
```
이코드는 클로저를 써서 handler.handleClick()을 onclick 이벤트 핸들러에서 직접 호출.
이 경우 말고 클로저를 여러 개 쓰면 코드를 이해하고 디버그하기 어려워짐.
따라서 많은 자바스크립트 라이브러리들은 함수를 특정한 컨텍스트에 묶는 함수를 만듬.
일반적으로 이런 함수를 bind()라고 부른다.

단순한 bind() 함수는 함수와 컨텍스트를 매개변수로 받고
주어진 함수를 주어진 컨텍스트에서 다른 매개변수를 전달하며 호출하는 함수르 반환.

```js
function bind(fn, context){
	return function(){
		return fn.apply(context, arguments);
	};
}
```
bind() 내부에서는 전달받은 함수를 apply()를 통해 호출하면서
context객체와 매개변수를 넘기는 클로저가 생성됨.
여기서 arguments객체는 bind()가 아니라 내부 함수에 사용됨.
반환된 함수(클로저)를 호출하면 자신이 넘겨받았던 함수를 실행하면서
주어진 컨텍스트와 매개변수를 전달합니다.
bind() 함수는 다음과 같이 사용합니다.

```js
	var handler = {
		message : "Event handled",
		handleClick : function(event){
			console.log(this.meassage);
		}
	};

	var btn = document.getelementById("my-btn");
	EventUtil.addHandler(btn, 'click', bind(handler, handleClick, handler));
```
위 예제는 bind() 함수를 써서 함수를 생성하여 컨텍스트를 관리하는 EventUtil.addHandler()에
넘겼음.

```js
	var handler = {
		message: "Event handled",
		handleClick : function(evnet){
			alert(this.message + ":" + event.type);
		}
	};
	var btn = document.getElementById("my-btn");
	EvenetUtil.addHandler(btn, "click", bind(handler.handleClick, handler) );
```
직접적으로 바인드된 함수를 통해 모든 ㅐ개변수를 전달하므로
handler.handleClick() 메서드는  evnet 객체 역시 전달받음.
ECMAScript5에서는 모든함수에. 네이티브 bind() 메서드를 도입.
이제 bind()함수를 만들 필요없이 함수에 속한 메서드를 바로 호출하면됨.
```js
	var handler = {
		message : "Event handled",
		handleClick : function(event){
			alert(this.message + ":" + event.type );
		}
	};
	var btn = document.getElementById("my-btn");
	EventUtil.addHandler(btn, "click", handler.handleClick.bind(handler));

```
네이티브 bind() 메서드는 이미 설명한 함수와 비슷하게 동작하므로 this의 값이
될 객체를 넘기면 됨. (ie9+)

바인드된 함수는 함수를 여러번 호출해야 하므로 메모리도 많이 사용하고 일반적인
함수에 비해 다소 느리므로 꼭 필요할 때만 쓰길 권함.

### 1.5 함수 커링
커링은 매개변수 일부가 미리 지정된 함수를 생성
기본적인 방법은 함수 바인딩과 마찬가지로 클로저를 사용해 새 함수를 반환하는 것.
커링이 바인딩과 다른 점은 이렇게 생성된 새함수를 호출 할 때
매개변수 일부가 미리 지정된다는 점.

```js
	function add(num1, num2){
		return num1 + num2;
	}
	function curriedAdd(num2){
		return add(5, num2);
	}
	console.log(add(2,3)); // 5
	console.log(curriedAdd(3)); // 8
```
이 코드는 add()와 curriedAdd() 두가지 함수를 정의.
curriedAdd()는 add()의 첫번째 매개변수를 5로 지정한 것과 같음.
엄밀히 말해 curriedAdd()는 함수 커링이 아니지만 개념을 설명하기에 적합.

다음 함수는 커링을 만드는 범용함수.
```js
	function curry(fn){
		var args = Array.prototype.slice.call(arguments, 1);
		return function(){
			var innerArgs = Array.prototype.slice.call(argumnets),
				finalArgs = args.concat(innerArgs);
				return fn.apply(null, finalArgs);
		};
	}
```

curry() 함수의 주요 목적은 반환 함수의 매개변수를 적절한 순서로 정렬하는것.
curry()함수의 첫번째 매개변수는 커링할 함수이며. 다른 매개변수는 모두 전달할 값
call()을 써서 slice()의 컨텍스트를 arguments 객체로 바꾸고 1을 넘겨서
두번째 이후의 매개변수를 모두 가져옴.
이제 args 배열은 외부 함수에서 전달받은 매개변수.
slice()를 써서 넘겨받은 메개변수를 모두 포함하는 innerArgs 배열을 만듬.
이 배열은 내부 함수에서 사용.

```js
	function add(num1, num2){
		return num1 + num2;
	}
	var curriedAdd = curry(add, 5);
	console.log(curriedAdd(3)); // 8
```
add()의 첫번째 매개변수를 5로 고정한 커링 함수를 만듬.

- 함수 매개변수를 모두 전달할 수도 있음.
```js
	function add(num1, num2){
		return num1 + num2;
	}
	var curriedAdd = curry(add, 5, 12);
	console.log(curriedAdd()); // 17
```
이코드에서는 커링된 add()함수에 매개변수 두가지를 모두 제공했으므로 나중에 추가하지 않아도 됨.
함수 커링은 보통 함수 바인딩의 일부로 포함되어 더 복잡한 bind() 함수를 만드는 데 사용

```js
	function bind(fn, context){
		var args = Array.prototype.slice.call(arguments, 2);
		return function(){
			var innerArgs = Array.slice.call(arguments),
				 finalArgs  = args.concat(innerArgs);     
			return fn.apply(context, finalArgs)
		}         
	}
```
curry()는 단순히 감쌀 함수만 받지만 bind()는 함수와 context 객체를 함께 받음.
따라서 바인드된 함수의 매개변수는 세번 째부터 시작하므로 slice()를 처음 호출하는 두 번째 행에 2를 지정


## 2. 쉽게 조작할 수 없는 객체
자바스크립트의 단점중 하나중에 컨텍스트가 같다면 객체를 수정하지 못하게 막을 방법이 없음.
개발자들이 실수로 다른 개발자의 코드를 덮어쓰곤 하며,
심지어 네이티브 객체를 잘못된 방법으로 수정하기 까지 한다.

### 2.1 확장 불가능한 객체
기본적으로 자바스크립트의 객체는 확장가능.
언제든 프로퍼티나 메서드를 객체에 추가할수 있음

```js
	var person = { name: 'nic'};
	Object.preventExtensions(person);

	person.age = 29;
	console.log(person.age); //undefined

```
일단 Object.preventExtensions()를 호출하면 person 객체에 새 프로퍼티나 메서드를 추가할수 없음

객체를 확장 불가능하도록 수정하면 새 멤버를 추가할 수는 엉ㅄ지만 이미 존재하는 멤버의
성격은 달라지지 않으므로 멤버를 수정하거나 삭제할수 있음.
객체가 확장 가능한지 판단하는  **Object.isExtensible()** 메서드도 있음.

```js
	var person = {name : 'nic'};
	console.log(Object.isExtensible(person)); // ture

	Object.preventExtensions(person);
	console.log(Object.isExtensible(person)); // false
```
