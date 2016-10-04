#  12 dom 레벨2와 레벨3


## 1. dom의 변경점 
: dom레벨 2와 레벨 3코어의 목적은 xml 요건에 맞도록 dom api를 확장하고 에러처리와 기능 탐지를 개선하는 것 


### 1.1 xml 네임스페이스 
: 서로 다른 xml 기반 언어에서 유래한 요소들을 형식에 맞는 문서에서 섞어 사용하면서 요소 이름사이에 충돌이 일어나지 않게 함. 
xhtml에서만 지원됨. 

## 2. 스타일 
### 2.1 요소 스타일에 접근 

	var myDiv = document.getElementById('myDiv');

	myDiv.style.backgroundColor = 'red';

	myDiv.style.width = '100px';
	myDiv.style.height = '200px';

	myDiv.style.border = '1px solid red';

 - dom 스타일 프로터티와 메서드 
  . cssText 
  . length 
  . parentRule 
  . getPropertyPriority(propertyName)
  . getPropertyValue(propertyName)
  . item(색인)
  ... 

	
	for(var i=0, len=myDiv.style.length; i<len; i++){
		console.log( myDiv.style[i] ); 
	}


	var prop, value, i , len;
	for(i =0, len=myDiv.style.length; i<len; i++){
		prop = myDiv.style[i]; 
		value = myDiv.style.getPropertyValue(prop);
		console.log(prop + " : " + value);
	}

### 2.2 스타일 시트 다루기 

	var supportDom2StyleSheets = document.implementation.hasFeature('StyleSheets','2.0');

### 2.3 요소크기 
 
 - offsetHeight : 요소가 차지하는 전체 높이를 픽셀로 나타낸 수치. 
 - offsetLeft : 요소의 왼쪽 테두리부터 컨테이너 요소의 안쪽 왼편 테두리 사이의 거리를 픽셀로 나타낸 수치 
 - offsetTop :  요소의 위쪽 테두리부터 컨테이너 요소의 안쪽 위 테두리 사이의 거리를 픽셀로 나타낸 수치 
 - offsetWidth : 요소가 차지하는 전체 너비를 픽셀로 나타낸 수치 

 	function getElementLeft(element){
 		var actualLeft = element.offsetLeft;
 		var current = element.offsetParent;

 		while(current !== null){
			actualLeft += current.offsetLeft;
			current = current.offsetParent;
 		}
 		return actualLeft;
 	}

 	function getElementTop(element){
 		var actualTop = element.offsetTop;
 		var current = element.offsetParent;

 		while( current !== null){
 			actualTop += current.offsetTop;
 			current = current.offsetParent;
 		}
 		return actualTop;
 	}


 	- 클라이언트 크기
 	. clientWidth / clientHeight (좌우에 위아래 + 패딩값 )
	: 뷰포트 크기를 알아 낼때 가장 많이 쓰임. 
	
	function getViewport(){
		if(document.compatMode == 'BackCompat'){
			return {
				width : document.body.clientWidth,
				height: document.body.clientHeight
			};
		}else {
			return {
				width:document.documentElement.clientWidth,
				height: document.documentElement.clientHeight
			};
		}
	}

 - 스크롤 크기 
 . scrollHeight : 스크롤바가 없을때 차지했을 콘텐츠의 전체 높이 
 . scrollLeft : 콘텐츠 영역의 왼쪽에 보이지 않는 픽셀 너비 
 . scrollTop : 콘텐츠 영역의 위에 보이지 않는 픽셀 높이 
 . scrollWidth : 스크롤바가 없을때 차지했을 콘텐츠의 전체 너비 


	var doHeight = Math.max( document.documentElement.scrollHeight, document.documentElement.clientHeight);
	var doWidth = Math.max( document.documentElement.scrollWidth, document.documentElement.clientWidth);

  - 요소 크기 확인 


## 3. 이동 

### 3.1 NodeIterator 
: 새 인스턴스는 document.createNodeIterator() 메서드로 생성.

 . root
 . whatToShow 
 .... 

### 3.2 TreeWalker
: NodeIterator를 더 발전시킨 버전 

 .parentNode() : 현재 노드의 부모로 이동
 .firstChild() : 현재 노드의 첫번째 자식으로 이동
 .lastChild() : 현재 노드의 마지막 자식으로 이동 
 .nextSibling() : 현재 노드의 다음 형제로 이동 
 .previousSibling() : 현재 노드의 이전 형제로 이동 

document.createTreeWalker()메서드로 생성. 

##4. 범위 
페이스 조작을 더 광법위하게 만듬. 

### 4.1 dom과 범위 
### 4.2 인터넷 익스플로러 8 및 이전버전의 범위 
