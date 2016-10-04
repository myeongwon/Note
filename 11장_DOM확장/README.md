#  dom 확장

: dom 확장의 우선적 표준은 선택자 api와 html5 두가지 


## 1.선택자 api 
: css선택자로 패턴을 만들고 그에 맞는 dom요소를 선택하는 능력 

### 1.1 querySelector() 메서드
: 매개변수로 css 쿼리를 받고 패턴에 일치하는 첫번째 자손 요소를 반환하며 일치 하는것이 없으면 null을 반환
```js
	var body = document.querySelector('body');
	var myDiv = document.querySelector('#myDiv');
```

### 1.2 querySelectorAll() 메서드 
: css쿼리를 매개변수로 받고 일치하는 노드 전체를 반환  이메서드는 NodeList의 정적인스턴스를 반환 
```js
	var strong = document.querySelectorAll('p strong');
```
NodeList 객체에서 item()메서드나 대괄호 표기법을 써서 원하는 요소를 가져올수 있음.
```js
	var i, len, strong;
	for( i=0, len=strongs.length; i< len; i++){
		strong = strongs[i]; 
		strong.className = 'important';
	}
```

### 1.3 matchesSelector()메서드
: css선택자를 매개변수로 받고 요소가 그에 일치하면 true 일치하지하지않으면 false반환 
```js
	if(document.body.matchesSelector('body .page1')){
		//true면 
	}
```

요소를 쉽게 비교할수 있음.
현재 거의 


## 2. 요소간 이동
- childElementCount : 자식 요소 숫자를 반환하되 텍스트 노드와 주석은 제외.
- firstElementChild : 마지막 자식 요소를 가리킴. 
- lastElementChild : 마지막 자식 요소 
- previousElementSibling : 이전 형제요소를 가리킴 
- nextElementSibibling :  다음 형제요소를 가리킴 

. 특정 요소의 자식 요소를 순회할 때 

```js
	var i, len, child = element.firstChild;
	while(child != element.lastChild){
		if(child.nodeType == 1){  // 요소인지 확인 
			proceesschlild(chld);

		}
		child = child.nextSibling;
	}


	var i, len, child = element.firstElementChild;
	while( child != element.lastElement(child)){
		proceeChild(child); // 요소임을 이미알고있음.
		child = child.nextElementSibling;
	}
```
 ie9+


 ## 3. html5

 ### 3.1 클래스 관련 추가사항

 - getElementsByClassName() 메서드 
: 클래스 이름 문자열을 매개변수로 받으며 해당 클래스를 모두 가진 요소의 NodeList를 반환 함. 
	
```js
	// 순서에 상관 없이 클래스 이름에 username과 current가 모두 잇는 요수 찾음.
	var allCurrentUsernames = document.getelementsByClassName('username current');
```
- classList 프로퍼티 

	//user 클래스 제거
```js	
	var classNames = div.className.split(/\s+/);

	//제거할 클래스 이름을 찾습니다.
	var pos = -1, i, len;
	for( i=0, len=classNames.length; i <len; i++){
		if(classNames[i] == 'user'){
			pos = i;
			break;
		}
	}
	// 클래스 이름 제거
	classNames.splice(i,1);
	// 클래스 이름을 다시 지정
	div.className = classNames.join(' ');
```

  *add(value) : 주어진 문자열 값을 목록에 추가합니다.
  				값이 이미 존재하면 추가하지 않음. 

  *contains(value) : 주어진 값이 목록에 존재하면 true를, 그렇지 않다면 false;
  * remove(value) : 주어진 문자열 값을 목록에서 제거 
  * toggle(value) : 값이 목록에 존재하면 제거하고 그렇지 않으면 추가

```js
 	div.classList.remove('active');
	div.classList.toggle('active');

	// 클래스 이름을 순회 
	for( var i=0, len =div.classList.length; i<len; i++){
		doSomething(div.classList[i]);
	}
```
 ### 3.2 포커스 관리 
: document.activeElement

```js
 	var button = document.getEleemntById('myButton');
 	button.focus();
 	console.log( document.activeElement === button ); //ture 
```

- document.hasFoucs();  문서에 포커스가 있는지 나타내는 불리언 값을 반환.
```js
	var button = document.getElementById('myButton');
	button.focus();
	console.log(document.hasFocus()); // true
```

### 3.3 HTMLDocument의 변화 

 * readyState 프로퍼티 
 :  loading - 문서를 불러오는 중. 
 :  complete - 문서를 완전히 불러옴. 

 활용의 좋은예 문서를 불러왔는지 확인하는것 
```js
 	if(document.readyState == 'complete'){
 		// 코드 
 	}
```

 *호환성 모드 
 * head 프로퍼티
```js
  	var head = document.head || document.getElementsByTagName('head')[0];

````
### 3.4 문자셋 프로퍼티 

```js	
	console.log(document.charset);  // 'utf-16';
	document.charset = 'utf-8';

	if(document.charset != document.defaulfCharset){
		alert('custom character set being used.');
	}
````
### 3.5 커스텀 데이터 속성

	: html5 요소에서는 요소의 렌더링에 필요한 정보나 시맨틱 값이 아닌 데이터를 접두사 data- 붙은 비표준 속성에 제공.
```js
	<div id="myDiv" data-appId="12345"></div>

	var div = document.getElementById('my Div');

	// 값가져오기
	var appId = div.dataset.appId;
	var myName = div.dataset.myname;

	// 값을 저장
	div.dataset.appId = 2345;
	div.dataset.myname = 'cho';

	//myname이 존재하는지 확인 
	if (div.dataset.myname){
		console.log(div.dataset.myname);
	}

```js
### 3.6  마크업 삽입 
	
 *innerHTML 프로퍼티
 : 문자열을 적절한 dom 트리로 파싱 
```js
	div.innerHTML = 'hello world!';
```
 *outerHTML 프로퍼티 
 : html요소를 자식 노드와 함께 반환. 
```js
  var p = document.createElement('p');
  p.appendChild(document.createTextNode('this is a paragraph.'));
  div.parentNode.replaceChild(p, div);
```
  *insertAdjacentHTML()  메서드 
  : 삽입할 위치와 html텍스트 두가지를 매개변수로 받음. 
   첫 번째 매개변수는 반드시 다음값중 하나 

   - beforebegin : 호출한 요소 바로앞에 삽입 
   - afterbegin :  호출한 요소의 첫번째 자식 요소 바로 앞에 삽입 
   - beforeend : 호출한 요소의 마지막 자식 요소 바로 다음에 삽입 
   - afterend :  호출한 요소 바로 다음에 삽입 

### 3.7 scrollIntoView() 메서드 
 	document.form[0].scrollIntoView();

 요소에 포커를 주어도 브라우저가 포커스를 제대로 표시하도록 요소를 스크롤 합니다.




 ## 4. 전용 확장 

 ### 4.1 문서모드
 ### 4.2 children 프로퍼티 
 : ie9미만 버전과 타브라우져 사이에는 공백을 텍스트 노드로 취급하는 방법이 달라 만들어짐 
 요소의 자식 요소만 포함하는 htmlCollection임. 
```js
 	var childCount = element.children.length;
 	var firstChild = element.children[0];
```

 ### 4.3 contains()메서드 
 : 문서트리를 순회하지 않고도 주어진 노드가 다른 노드의 자손인지 확인 가능함.
 매개변수로 받은 노드가 호출한 노드의 자손이면 true 아니면 false를 반환 
```js
	console.log( document.documentElement.contains( document.body) ); //ture
```
ie9+ 이상지원 

노드사이의 관계는 **Dom 레벨 3메서드인 compareDocumentPosition()**을 통해서도 확인 가능. 

- 마스크 1 : 단절. 매개변수로 주어진 노드가 문서에 존재 하지않음.
- 마스크 2 :  선행. 매개변수로 주어진 노드가 dom트리에서 참조노드보다 앞에 
- 마스크 4 : 후행 . 
- 마스크 8  : 조상
- 마스크 16 : 자손 
```js
	var result = document.documentElement.compareDocumentPosition( document.body);
	console.log(!! (result & 16));
```

### 4.4 마크업 삽입 

 - innerText 프로퍼티 
```js
	function getInnerText(element){
		return (typeof element.textContent == 'string') ? 
		element.textContent : element.innerText;
	}

	function setInnerText(element, text){
		if(typeof element.textContent == 'string'){
			element.textContent : text;
		}else {
			element.innerText = text;
		}
	}
```

 - outerText 프로퍼티 
 : 호출한 요소의 자식 노드만 교체하는 것이 아니라 자식 노드를 포함함 젠체 요소를 교체 
```js
 	div.outerText = 'hello world';

 	var text = document.createTextNode('hello world');
 	div.parentNode.replaceChild(text, div);
```

### 4.5 스크롤 
 (크롬과 사파리에서만 )
 - scrollIntoViewIfNeeded(alignCenter)
 - scrollByLines(lineCount)
 - scrollByPages(pageCount) 	: pageCount에 주어진 숫자의 페이지 높이만큼 요소를 스크롤 합니다. 페이지높이는 요소의 높이를 기준으로 판단. 

```js
	// body 요소를 다섯 줄 스크롤함. 
 	document.body.scrollByLines(5);
 	// 요소가 현재 보이지 않으면 보이게끔 스크롤 함.
 	document.images[0].scrollIntoViewIfNeeded();
 	// body 요소를 한 페이지 위로 스크롤 함.
 	document.body.scrollByPages(-1);
```