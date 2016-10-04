#  6 객체 지향 프로그래밍 

: 객체지향 Object-oriented 언어는 일반적으로 클래스를 통해 같은 프로퍼티와 메서드를 가지는 객체를 여러개 만드는 특징


객체가 특별한 순서가 없는 값의 배열이란 의미 
각 프로퍼티와 메서드는 이름으로 구별하며 값을 대응 


## 1. 객체에 대한 이해 
Object 인스턴스에 프로퍼티와 메서드를 추가하는 방법. 
```js
	var person = new Object();
	person.name = 'cho';
	person.age = '10';
	persona.sayName = function(){
		console.log(this.name);
	};

리터럴 표기법 
	var person = {
		name: 'cho',
		age : '10',
		sayName: function(){
		console.log(this.name);
		}
	}

```
### 1.1 프로퍼티 타입 
프로퍼티에는 데이터 프로퍼티와 접근자 프로퍼티 두가지 타입.
	- 데이터 프로퍼티 
	- 접근자 프로퍼티 
	: 
```js
	var book = {
		_year: 2004,
		edition:1
	};
	Object.defineProperty(book, 'year',{
		get : function(){
			return this._year;
		},
		set : function
		})
```
### 1.2  다중프로퍼티 정의
Object.defineProperties()메서드를 제공. 

```js
	var book = {};
	Object.defineProperties(book, {
		_year : {
			value : 2004
		},
	
		edition: {
			value : 1
		},
		year : {
			get: function(){
				return this._year;
			},
			set: function(newValue){
				if(newValue > 2004){
					this._year = newVaue;
					this.edition += newValue - 2004;
				}
			}
		}

	});
```
### 1.3 프로퍼티 속성 읽기
:ECMAScript5의 Object.getOwnPropertyDescriptor()메서드를 이용해 원하는 프로퍼티의 서술자 프로퍼티를 읽을 수 있습니다. (ie9+)
```js
	var book = {};
	Object.defineProperties(book, {
		_year : {
			value :2004
		},
		edition:{
			value:1
		},

		year: {
			get: function(){
				return this._year;
			},
			set: function(newValue){
				if(newValue > 2004 ){
					this._year = newValue;
					this.edition += newValue - 2004;
				}
			}
		}
	});

	var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
	console.log( descriptor.value );
	console.log( descriptor.configurable); // false
	console.log( typeof descriptor.get ); //undefined 

	var descriptor = Object.getOwnPropertyDescriptor(book, 'year');
	console.log( descriptor.value); // undefined
	console.log( descriptor.enumerable); //false
	console.log( typeof descriptor.get); // 'function'
```
	

## 2. 객체생성
: Object생성자, 객체리터럴이용해 객체 생성시 단점이 있음.
같은 인터페이스를 가진 객체를 여러개 생성할경우 중복 코드가 많아짐.
해결책으로 **팩토리 패던을 사용**

## 2.1 팩터리 패턴 
: 특정 객체를 추상화하는 과정 
```js
	function createPerson(name, age, job){
		var o = new Obeject();
		o.name = name;
		o.age = age;
		o.job = job;
		o.sayName = function(){
			console.log(this.name);
		};
		return o;
	}

	var person1 = createPerson('cho', 10, 'doctor');
	var person2 = createPerson('kim', 30, 'doctor');
```
함수에 필요하나 정보를 매개변수로 받아 객체를 생성함. 
: 이 팩토리 패턴을 쓰면 비슷한 객체를 여러개 만들때의 중복코드는 해결가능. 하지만 **생성한 객체가 어떤 타입인지 알수 없음**


### 2.2 생성자 패턴
: 특정 타입의 객체를 만드는데 쓰임.
네이트브 생성자. Object, Array 는 실행환경에서 자동으로 만들어지는 반면에 
커스텀 생성자를 만들어서 원하는 타입의 객체에 필요한 프로퍼티와 메서드를 정의 할수 있음. 
```js
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.sayName = function(){
			console.log(this.name);
		}
	}

	var person1 = new Person('cho', 10, 'doctor');
	var person2 = new Person('kim', 30, 'doctor');
```


- 명시적으로 객체를 생성하지 않음
- 프로퍼티와 메서드는 this 객체에 직접적으로 할당
- return문이 없음. 
* 첫 글자가 대문자 P인 점은 
생성항자 함수는 항상 대문자로 시작하고 생성자 아닌 함수는 소문자로 시작하는 표기법으로 널리 쓰이기 때문임. 


Person의 새 인스턴스를 만들때 new 연산자를 사용. 
생성자를 이런식으로 호출하면 내부적으로
1. 객체생성
2. 생성자의 this값에 새객체를 할당. this는 새 객체를 가리킴.
3. 생성자 내부 코드 실행. (객체에 프로퍼티 추가)
4. 새 객체 반환. 

타입을 알아내는 목적으로 instanceof 연산자가 더안전.
```js	
	console.log(person1 instanceof Object);  //ture
	console.log(person1 instanceof Person);  // true
	console.log(person2 instanceof Object); //ture
```
 - 함수로서의 생성자 
 생성자 함수와 다른함수의 차이는 호출방법.
 new 연산자와 함께 호출한 함수는 생성자처럼 동작. 
```js
 	var person = new Person('cho', 29, 'doctor');
 	person.sayName();

 	// 함수로 호출 
 	Person('cho', 30, 'doctor');
 	window.sayName(); // 'cho'

 	// 다른 객체의 스코프에서 호출 
 	var o = new Object();
 	Person.call(o, 'kim', 25, 'doctor');
	o.sayName();  // 'kim'
```

- 생성자의 문제점 
: 인스턴스마다 메서드가 생성된다는 점.
```js
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = new Function( console.log(this.name) ); //논리적으로 동등
	}
```	
이런 식으로 생성하면 스코프 체인과 식별자 해석이란 점에서는 다르지만 Function의 새 인스턴스를 만드는 방식과 같음. 

	console.log(person1.sayName == person2.sayName);  //false

똑같은 일을 하는 Function 인스턴스 두개가 따로 존재한다는 점은 상식적이지 않으며, this객체를 응용하여 함수가 런타임에서야 비로소 객체에 묶이도록 할 수 있으므로 더 비효율적 
```js
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = sayName;
	}

	function sayName(){
		console.log(this.name);
	}

	var person1 = new Person('cho', 10, 'doctor');
	var person2 = new Person('kim', 20, 'doctor');
```
이예제는 sayName() 함수를 생성자 바깥에서 정의. 
생성자 내부에서는 sayName 프로퍼티에 전역 sayName() 함수를 할당.

이경우. 함수 중복 문제를 막을수 있지만 전역스코프를 어지럽힘. 
이문제는 프로토타입 패턴으로 해결할수 있음.

### 2.3 프로토타입 패턴
모든 함수는 prototype 프로퍼티를 가짐. 
이 프로퍼티는 해당 참조 타입의 인스턴스가 가져야 할 프로퍼티와 메서드를 담고 있는 객체. 


프로토타입의 프로퍼티와 메서드는 객체 인스턴스 전체에서 공유된다는 점이 프로토타입의 장점. 
```js
	function Person(){}

	Person.prototype.name = 'cho';
	Person.prototype.age = 10;
	Person.prototype.job = 'doctor';
	Person.prototype.sayName = function(){
		console.log(this.name);
	};

	var person1 = new Person();
	person1.sayName(); // 'cho'

	var person2 = new Person();
	person2.sayName(); //'cho'

	console.log(person1.sayName == person2.sayName);  // true
```

프로퍼티들과 sayName()메서드는 Person의 prototype 프로퍼티에 직접 추가되었고 생성자 함수는 비워둠. 

생성자 패턴과는 달리 프로퍼티와 메서드를 모든 인스턴스에서 공유하므로 person1 과 person2는 같은 프로퍼티집합에 접근하며 sayName() 함수를 공유. 


- 프로토타입은 어떻게 동작할까? 

함수가 생성될때마다. 
모든 프로토타입은 자동적으로 constructor 프로퍼티를 갖는데 이프로퍼티는 해당 프로토타입이 프로퍼티로서 소속된 함수를 가리킴 

insPrototypeOf()메서드를 통해 
ㅍ객체 사이에 프로토타입 연결이 존재하는지 isPrototypeOf()메서드를 통해 알수 있음. 
```js
	console.log(Person.prototype.isPrototypeOf(person1)); // ture
	console.log(Person.prototype.isPrototypeOf(person2)); //true
```
ES5 에서는 [[Prototype]]의 값을 반환하는 Object.getPrototypeOf() 메서드가 추가됨. (ie9+)
```js
	console.log(Object.getPrototypeOf(person1) == Person.prototype); // ture
	console.log(Object.getPrototypeof(person1).name); // 'cho'
```

 검색은 객체 인스턴스 자체에서 시작됨.
 인스턴스 프로퍼티 이름을 찾으면 그값을 반환
프로퍼티를 찾지 못하면 포인터를 프로토타입으로 올려서 검색을 계속.함. 프로퍼티를 프로토타입에서 찾으면 그값을 반환함. 

객체 인스턴스에서 프로토타입에 있는 값을 읽을 수는 있지만 수정은 불가능.

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 해당 프로퍼티는 인스턴스에 추가되며 프로토타입까지 올라가지 않음. 



```js
	function Person(){

	}

	Person.prototype.name = 'cho';
	Person.prototype.age = 29;
	Person.prototype.job = 'doctor';
	Person.prototype.sayName = function(){
		console.log(this.name);
	};

	var person1 = new Person();
	var person2 = new Person();

	person1.name = 'kim';
	console.log(person1.name); // 'kim' (인스턴스에서)
	console.log(person2.name); //  'cho' (프로토타입에서)

	delete person1.name;
	console.log(person1.name);  // 'cho' (프로토타입에서)
```

hasOwnProperty()메서드
: 프로퍼티가 인스턴스에 존재하는지 프로토타입에 존재하는지 확인 
Object로부터 상속한것,
해당 프로퍼티가 객체 인스턴스에 존재할 때만 true를 반환.
```js
	function Person(){}

	Person.prototyp.name = 'cho';
	Person.prototype.age = 29;
	Person.prototype.job = 'doctor';
	Person.prototype.sayName = function(){
		console.log(this.name);
	};

	var person1 = new Person();
	var person2 = new Person();

	console.log(person1.hasOwnProperty('namem') ); // false

	person1.name = 'kim';
	console.log(person1.name); // 'kim' (인스턴스에서)
	console.log(person1.hasOwnProperty('name') ); //true

	delete person1.name;
	console.log(person1.name); //'cho' (프로타입에서) 
	console.log(person1.hasOwnProperty('name') ); // false
```
- 프로토타입과 in 연산자
in 연산자의 두가지 쓰임

1. 그 자체로서 사용하는 경우
주어진 이름의 프로퍼티를 객체에서 접근할수 있을때 
해당프로퍼티가 인스턴스에 존재하든 프로토타입에 존재하든 모두 true 반환
```js	
	function Person(){}
	
	Person.prototype.name = 'cho';
	Person.prototype.age = 29;
	Person.prototype.job = 'doctor';
	Person.prototype.sayName = function(){
		console.log(this.name);
	};

	var person1 = new Person();
	var person2 = new Person();

	console.log(person1.hasOwnProperty('name') ); // false
	console.log('name' in person1 ); //ture 
	
	person1.name = 'kim';
	console.log(person1.name); //'kim' (인스턴스에서)
	console.log(person1.hasOwnProperty('name') ); // ture
	console.log('name' in person1); // ture

	delete person1.name;
	console.log(person1.name); // 'cho' (프로토타입에서 )
	console.log(person1.hasOwnProperty('name') ); // false
	console.log('name' in person1); // ture
```

name 프로퍼티 객체에서 직접이든 프로토타입에서든 접근할수 있음.

객체프로퍼티가 프로토타입에 존재하는지는 hasOwnProperty()와 in 연산자를 조합해서 알수있음.
```js	
	function hasPrototypeProperty(object, name)	{
	return !object.hasOwnProperty(name) && (name in object);
} // 프로토타입에 존재하는지 . 프로퍼티가 

	function Person(){}
	
	Person.prototype.name = 'cho';
	Person.prototype.age = 29;
	Person.prototype.job = 'doctor';
	Person.prototype.sayName = function(){
		console.log(this.name);
	};

	var person = new Person();
	console.log(hasPrototypePrototy(person, 'name' )); // true

	person.name = 'kim';
	console.log(hasPrototypeProperty (person, 'name')); // false

```


2. for-in 루프에서 사용하는 경우. (  대체에서 Obeject.keys(),  Object.getOwnPropertyNames() )

객체에서 접근가능. 나열가능한 프로퍼티 반환 인스턴스프로퍼티와 프로토타입 프로퍼티 모두 포함 

ES5 의 경우  Obeject.keys()메서드 통해  객체인스턴스에서 나열 가능 한 프로퍼티의 전체목록을 얻을수 있음. (ie9+)
```js
	function Person(){}

	Person.prototype.name = 'cho';
	Person.prototype.age = 29;
	Person.prototype.job = 'doctor';
	Person.prototype.sayName = function(){
		console.log(this.name);
	}

	var keys = Object.keys(Person.prototype);
	console.log(keys); //'name,age,job,sayName'

	var p1 = new Person();
	p1.name = 'kang';
	p1.age = 32;

	var plkeys = Object.keys(p1);
	console.log(p1key1); // 'name', 'age'
```

나열 가능 여부와 관계없이 인스턴스 프로퍼티 전체 목록을 얻으려면 Object.getOwnPropertyNames() 메서드를 같은 방법으로 사용. (ie9+) 
```js
	var keys = Object.getOwnPropertyNames(Person.prototype);
	console.log(keys); // ["constructor", "name", "age", "job", "sayName"]

```
- 프로토타입의 대체문법 
모든 프로퍼티와 메서드를 담은 객체 리터럴로 프로토타입을 덮어써 반복을 줄이고 프로토타입에 기능을 더 가독성있게 캡슐화하는 패턴 
```js
	function Perosn(){}

	Person.prototype = {
		name : 'cho',
		age : 20,
		job : 'doctor',
		sayName : function(){
			console.log(this.name);
		}
	};
```
construtor프로퍼티가 Person을 가리키지 않는다는 점만 빼면 최종 결과는 같음.
```js
	var friend = new Person();
	console.log(friend instanceof Object); // true
	console.log(friend instanceof Person); // true
	console.log(friend constructor == Person); //false
	console.log(friend constructor == Object); //true


	function Person(){}
	Person.prototype = {
		constructor : Person,
		name : 'cho',
		age : 10,
		job : 'doctor',
		sayName : function(){
			console.log(this.name);
		}
	};
```

이코드는 constructor 프로퍼티를 명시적으로 생성하고 그값에 Person을 지정하여 프로퍼티에 적절한 값을 담기도록 함. 
이렇게 하면 constructor 프로퍼티가 나열 가능해짐.

ES5 경우.   Object.defineProperty()를 쓰는게 좋음.
```js
	function Person(){}

	Person.prototype = {
		name : 'cho',
		age : 10,
		job : 'doctor',
		sayName : function(){
			console.log(this.name);
		}
	};

	// ES5 생성자를 복구 
	Object.defindeProperty( Person.prototype, 'constructor', {
		enumerable : false,
		value : Person
	});
```

- 프로토타입의 동적 성질
프로토타입이 바뀌면 그 내용이 즉시 인스턴스에도 반영이 됨.
프로토타입이 바뀌기 전에 빠져나온 인스턴스도 바뀐 내용을 반영.

```js
	var fr = new Person();
	Persn.prototype.sayHi = function(){
		console.log('hi~');
	};

	fr.sayHi(); // 'hi'  동작함
```

fr인스턴스는 sayHi()가 추가되기 전에 만들어졌는데도 이메서드에 접근할수 있음. 
가능한 이유는 인스턴스와 프로토타입 사이의 느슨한 연결 때문. 

인스턴스와 프로토타입은 포인터를 통해 연결되었을 뿐 인스턴스를 생성할때 sayHi 없는 프로토타입을 복사한것이 아니므로 프로토타입에서 sayHi 프로퍼티를 찾아 여기 저장된 함수를 반환.

```js
	function Person(){}

	var fr = new Person();

	Person.prototype = {
		constructor : Person,
		name : 'cho',
		age : 19,
		job : 'doctor',
		sayName : function(){
			console.log(this.name);
		}
	};


	fr.sayName();; // error 
```


프로토타입 객체를 덮어쓰기 전에 Person의 인스턴스 생성. 
fr.sayName()을 호출 에러가 발생.
fr가 가리키는 프로타입에는 그런 프로퍼티가 존재하지 않음.


- 네이티브 객체 프로토타입 

