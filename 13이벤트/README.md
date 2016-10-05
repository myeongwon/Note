#  13 이벤트 

## 1. 이벤트흐름
### 1.1 이벤트 버블링 
: 인터넷 익스플로러 이벤트 흐름. 
이벤트 흐름상 문서 트리에서 가장 깊이 위치한 요소에서 시작해 거슬러 흐르는 모양. 

ex) 페이지에서 div 요소를 클릭하면 이벤트 발생순서
1. div 
2. body 
3. html
4. document 


### 1.2 이벤트 캡처링 
: 최상위 노드에서 처음으로 이벤트가 발생 가장 명시적인 노드에서 마지막으로 발생. 

ex) 페이지에서 div 요소를 클릭하면 이벤트 발생순서
1.document.
2. html
3.body
4. div
-현재는 ie9+ 사파리, 크롬 , 오페라 ,파이어폭스에서 지원 
-사실 이들 브라우저 이벤트는 window에서 시작 

### 1.3 dom 이벤트 흐름

-dom레벨 2 이벤트에서 정의한 이벤트 흐름에는 
1. 이벤트 캡처링 단계
2. 타깃 단계
3. 이벤트 버블링 

## 2.이벤트 핸들러 
:이벤트에 응답하여 호출되는함수 

### 2.1 html 이벤트 핸들러 

### 2.2 dom 레벨 0 이벤트 핸들러 
: 이벤트 핸들러를 할당하는 전통적인 방법은 이벤트 핸들러 프로퍼티에 함수를 할당하는 방법 
```js	
	var btn = document.getElementById('myBtn');
	btn.onclick = function(){
		console.log('clicked');
	}


	var btn = document.getElementById('myBtn');
	btn.onclick = function(){
		console.log(this.id)	; // myBtn;
	};
```
이벤트 핸들러 내부에서는 this를 통해 요소의 프로퍼티나 메서드에 접근 가능합니다. 
이런 방식으로 추가한 이벤트 핸들러는 이벤트 흐름에서 버블링 단계에 실행 되도록 의도한 것

이벤트 핸들러를 제거할때 
```js
	btn.onclick = null; // 이벤트 핸들러 제거

```

### 2.3 dom 레벨 2 이벤트 핸들러 
: dom 레벨 2 이벤트에서는 이벤트 핸들러 할당과 제거는 
**addEventListener()와 removeEventListener()**를 정의 

```js
	var btn = document.getElementById('myBtn');
	btn.addEventListener('click', function(){
		console.log(this.id);
	},false);
```
dom 레벨 2 메서드의 주요 장점은 이벤트 핸들러를 추가하므로 이벤트 핸들러가 여러개 있을수 있음. 
```js
	var btn = document.getElementById('myBtn');
	btn.addEventListener('click', function(){
		console.log(this.id);
	}, false);

	btn.addEventListener('click', function(){
		console.log('hello');
	}, false);
```
이벤트 핸들러는 추가된 순서대로 실행되므로 요소의 id가 먼저 표시되고 다음 hello 메서지 표시 
```js
	var btn = document.getElementById('myBtn');
	btn.addEventListener('click', funtion(){
		console.log(this.id);
	},false);
-----------ㄷㅏ른코드 
	btn.removeEventListener('click', function(){
		//동작하지 않음.
		console.log(this.id);
	},false);


	var btn =document.getElementById('myBtn') ;
	var handler = function(){
		console.log(this.id);
	};
	btn.addEventLister('click',handler, false);

	// 다른코드 
	btn.removeEvenetListener('click',handler, false);

```
이벤트 핸들러가 버블링 단계에서 동작하도록 해야 모든 브라우저에서 지원되므로 이방법을 가장 많이 사용. 
이벤트 핸들러를 캡처 단계에 추가하는 건  필요 이벤트가 타깃에 도달하기 전에 가로채야 할 때  가장 적합 



### 2.4 인터넷 익스플로러 이벤트 핸들러 
:attachEvent(), detachEvent() 두 메서드를 구현 
```js
	var btn = document.getElementById('myBtn');
	btn.attachEvent('onclick', function(){
		console.log('clicked');
	});
```
인터넷 익스플로러에서 attachEvent()와 dom레벨 0 접근법의 주요차이는 이벤트 핸들러의 스코프 . 
dom레벨 0 접근법에서 이벤트 핸들러의 this는 요소이지만
attachEvent()로 등록한 이벤트 핸들러는 전역 컨텍스트에서 실행되므로 this는 window.
```js
	var btn = document.getElmentById('myBtn');
	btn.attachEvent('onclick', function(){
		console.log(this === window); //true
	});
```

이벤트 핸들러를 여러개 추가할 수 있음.
```js
	var btn =document.getElementById('myBtn');
	btn.attachEvent('onclick',function(){
			console.log('clicked');
	});
	btn.attachEvent('onclick',function(){
		console.log('hello');
	});
```

두가지 이벤트 핸들러 추가. 하지만 dom표준 메서드와 달리 
추가한 순서의 반대로 동작. 
익명함수로 추가한 이벤트 핸들러는 제거할수 없음.
```js
	var btn = document.getElementById('myBtn');
	var handler = function(){
		console.log('clicked');
	};
	btn.attachEvent('onclick', hadler);

	// 다른코드

	btn.detachEvent('onclick', handler);
```

### 2.5 크로스 브라우저 이벤트 핸들러 
: 이벤트 처리코드가 가능한 많은 브라우저에서 동작하게 하려면 버블링 단계에서만 동작 하도록 해야 함. 
**addHandler()**
이베서드는 EventUtil라는 객체에 등록됨. 
  매개변수로 요소, 이벤트이름, 이벤트 핸들러 함수 세가지 를 받음.

**removeHandler() ** 
```js
	var EventUtil = {
		addHandler : function(element, type, handler){
			if(elment.addEventListenr){
				element.addEventListener(type, handler, false);
			}else if(element.attachEvent){
				element.attachEvent('on' + type, handler);
			}else{
				element['on' + type] = handler;
			}
		},

		removeHandler : function(element, type, handler){
			if(element.removeEventListener){
				element.removeEvenetListener(type, hadler, false);
			}else if( element.detachEvent){
				element.detachEvent('on' + type, handler);
			}else {
				element['on' + type] = null;
			}
		}
	};


	var btn = document.getElementById('myBtn');
	var handler = function(){
		console.log('clicked');
	};
	EventUtil.addHandler(btn, 'click', handler);

	// 다른 코드 

	EventUtil.removeHandler(btn, 'click', handler);
```

## 3. event객체 
: dom 과 관련된 이벤트발생 정보는 event라는 객체에 저장. 

### 3.1 dom event객체 
: 이벤트 핸들러에 전달되는 매개변수는 event객체 하나분. 
```js
	var btn = document.getElementById('myBtn');
	btn.onclick = function(event){
		console.log('event.type') // 'click'
	}

	btn.addEventListener('click', function(event){
		console.log(evenet.type) // 'click'
	},false);
```

-.bubbles : 이벤트가 버블링 되는지 불리언
-.preventDefault() :  이벤트의 기본 행동 취소
-. stopPropagation() : 이벤트 캡처링이나 이벤트 버블링을 모두 취소 bubbles이 true면 이메서드를 쓸수 있음.
-. taget : 이벤트타켓 
-. type : 발생한 이벤트 타입 
-..... 

이벤트 핸들러 내부에서 this객체는 항상 currentTarget의 값과 일치 
```js
	btn.onclick = function(event){
		console.log(event.currentTarget === this); //true
		console.log(event.target === this); //true
	};

	
	// currentTarget , target, this의 값을 비교 

	document.body.onClick = function(e){
		console.log(e.currentTarget === document.body);  //true
		console.log(this === document.body); //true
		console.log( e.target === document.getElementById('myBtn')); //true 
	}

```
preventDefault()메서드는 이벤트의 기본동작을 취소 
링크 클릭의 기본동작은 href 속성에 정의된 url로 이동 
이동하지 않게 하려면 동작을 취소해야한다.
```js
	var link = document.getElementById('myLink');
	link.onclick = function(event){
		event.preventDefalut();	
	};
```

-preventDefault()로 이벤트 취소하려면 해당 이벤트의 cancelable 프로퍼티가 true여야 함. 

-stopPropagation()메서드는 이벤트 흐름을 즉시 멈춰서 이벤트 캡처링이나 버블링을 모두 취소 

```js	
	var btn = document.getElementById('myBtn');
	btn.onclick = function(e){
		console.log('Clicked');  // 실행됨.
		e.stopropatagation();    
	};

	document.body.onclick = function(e){
		console.log('body clicked');   // 실행 안됨 . 
	};
```
eventPhase 프로퍼티 현재 이벤트가 어느 단계에 있는지 나타냄.
eventPhase는  1 : 이벤트 핸들러가 캡처단곙에서 호출 되었으면 
2 : 타킷에서 호출되었으면  3. 버블링단계에서 호출 되었으면 

```js
	var btn =document.getElementByyId('myBtn');
	btn.onclick = function(e){
		console.log('e.eventPhase'); //2  
	}

	
	document.body.addEventListner('click', function(e){
		console.log(e.eventPhase);  //1 
	}, ture);  // ture 면 캡쳐링이니깐 ㅎㅎ 

	document.body.onclick = function(e){
		console.log(e.eventPhase);  // 3 
	};
```
1. 예제 :  document.body에서 발생하며 캡쳐링 단계이므로 1 
2. 예제 : 버튼에서 발생하므로 eventPhase 2 
3. 마지막으로 document.body에서 다시 발생하며 버블링 단계 3 


### 3.2 인터넷 익스플로러의 event객체

- 이벤트 핸들러를 dom 레벨0 접근법으로 할당하면 event객체는 오로지 window 객체의 프로퍼티로만 존재함. 
```js
	var btn = document.getElementById('myBtn');
	btn.onclick = function(){
		var event = window.event;
		console.log(event.type) // 'click'
	}
```
- attachEvent()로 할당하면 event 객체는 함수의 유일한 매개변수로 전달됨. 
```js
	var btn = document.getElementById('myBtn');
	btn.attachEvent('onclick', function(e){
		console.log(event.type) // 'click'
	});
```
이벤트 핸들러의 스코프는 할당 방식에 따라 다르므로 this 역시 항상 이벤트 타깃과 같지않음.
항상 event.srcElement를 사용하는 편이 좋음.

```js
	var btn = document.getElementById('myBtn');
	btn.onclick = function(){
		console.log( window.event.srcElement === this); //true
	};

	
	btn.attachEvent('onclick' , function(){
		console.log( event.srcElement === this); //false;
	});

```
-retrunValue 프로퍼티는 dom표준의 preventDefault()메서드와 마찬가지로 주어진 이벤트의 기본동작 취소
```js
	var link = document.getElementById('myLink');
	link.onclick = function(){
		window.event.returnValue = false;
	};
```
cancelBubble 프로퍼티는 dom 표준의 stopPropagation()메서드와 마찬가지로 이벤트 버블링 취소.
```js
	var btn =documnet.getElementById('myBtn');
	btn.onclick= function(){
		console.log('clicked');
		window.event.cancelBubble =true;
	};

	document.body.onclick = function(){
		console.log('body clicked');
	}
```
cancelBubble =true로 설정해 이벤트 핸들러까지 버블링 되지않음. 
버튼을 클릭했을경우.  clicked 만 콘솔창에 기록됨 
	

### 3.3 크로스 부라우저 이벤트 객체 
: 이벤트 모델을 하나로 모을 수 있는 EventUtil객체를 확장 
```js
	var EventItil = {
		addHandler : function(element, type, handler){

		},

		getEvent : function(e){
			return e? e : window.event;
		},
		getTarget : function(e){
			return e.target || event.srcElement;
		},
		preventDefault : function(e){
			if(e.preventDefault){
				e.preventDefault();
			}else{
				event.returnValue = false;
			}
		},

		stopPropagation: function(e){
			if(e.stopPropagation){
				e.stopPropagation();
			}else {
				event.cancelBubble = true;
			}
		}

	};

```

## 4. 이벤트 타입 

###4.1 ui 이벤트 

 -load 이벤트 
 :전체 페이지를 완전히 불러왔을때 발생. 

	EventUtil.addHandler(window, 'load', function(e){
		console.log('loaded!');
	});

html에서 onload 이벤트 핸들러를 직접 할당할수 있음.
	<img src="sample.png" onload="console.log('test!')">


이미지를 다불러왔을때 실행됨.
	var img = document.getElementsTagName('img');
	EventUtil.addHandler(image, 'load', function(event){
		event = EventUtil.getEvent(event);
		console.log( EventUtil.getTarget(event).src);
	});


새 img 요소를 생성할 때 이벤트 핸들러를 할당하여 이미지를 불러온 시점을 알 수있습니다. 

	EventUtil.addHandler(window, 'load', function(){
		var image = document.createElement('img');
		EventUtil.addHandler(image, 'load', function(event){
			event = EventUtil.getEvent(event);
			console.log(EventUtil.getTarget(event).src);
		});
		document.body.appendChild(image);
		image.src = 'smile.gif';
	});

	EventUtil.addHandler(window, 'load', function(){
		var script = document.createElement('script');
		script.type = 'text/javascript';

		EventUtil.addHandler(script, 'load', function(event){
			console.log('load');
		});
		script.src = 'example.js';
		document.body.appendChild(script);
	});

EventUtil객체를 써서 새로 생성한 script 요소에 onload 이벤트 핸들러를 할당 

	EventUtil.addHandler(window, 'load', function(){
		var link = document.createElement('link');
		link.type = 'text/css';
		link.rel = 'stylesheet';
		EventUtil.addHandler(link, 'load',function(e){
			console.log('css load~! ');
		});
		link.href = 'example.css';
		document.getElementsByTagName('head')[0].appendChild(link);
	});

 - unload 이벤트 
: 문서를 완전히 닫을때 발생 
다른페이지 이동, 각종 참조제거해 메모리 누수 방지 목적 

- resize 이벤트 
: 브라우저 창의 높이나 너비를 바꾸면 window에서 이벤트가 발생. 

- scroll 이벤트 
: 브라우져 별로 다른요소를 참조해야됨. 
	EventUtil.addHandler(window, 'scroll', function(e){
		if(document.compatMode == 'CSS1Compat'){
			console.log(document.documentElement.scrollTop)
		}else{
			console.log(document.body.scrollTop);
		}
	})



### 4.2 Focus 이벤트 
:  요소가 포커스를 받거나 잃을때 발생함. 

focus와 blur는 버블링 되지 않음.


### 4.3 마우스 이벤트와 휠 이벤트 

 - 마우스 휠 이벤트 

	EventUtil.addHandler(document, 'mousewheel', function(event){
		event = EventUtil.getEvent(event);
		console.log('event.wheelDelta');
		
	});

	EventUtil.addHandler(window, 'DOMMouseScroll', function(event){
		event = EventUtil.getEvent(event);
		console.log(event.detail);
	});


### 4.4 키보드와 텍스트 이벤트 
 - 키코드 
 - 문자코드 

 var EventUtil = {
 	getCharCode : function(event){
	 	if(typeof event.charCode == 'mumber'){
	 		return event.charCode;
	 	}else {
	 		return event.keyCode;
	 	}
 	},
 }



