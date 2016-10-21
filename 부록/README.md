# E.1  map

먼저 임의의 배열을 만들어 map이란 메서드가 있는지 확인. 

```js

if( [1].map ){
	console.log('using native map()');
}
```

우리가 만들 메서드는 elsㄷ 절에서 작업하도록 
```js
if( [1].map ){
	console.log('using native map()');
}else {
	Array.prototype.map = function(callback){

	}
}
````

원래 배열은 map을 호출하는 배열, 즉 this 

```js
if(){
	
}else {
	Array.prototype.map  = function (callback){
		var count = this.length;
		rtn = new Array(count);
	}

}

```

```js

var values = [1,2,3,4,5];
var values2 = values.map(function (data){
	return data *2;
})
console.log(values);
console.log(values2);
````



# E.2  forEach

```js

if(![1].forEach){
	Array.prototype.forEach = function(callback){
		for( var i=0; i<this.length; i++){
			callback(this[i]);
		};
	}
}
```

dom 요소를 가져올때 사용하는 메서드는 진짜 배열이 아니라 배열 비슷한 객체이기 때문에 forEach나 map 같은 전ㅇ용 메서드를 상속하지 못함.

- 객체를 받아 배열을 반환하는 유틸리티 함수를 만드는 방식 
배열 비슷한 객체를 배열로 변환하는 가장 좋은 방법은 Array.slice() 


```
Array.prototype.slice.call(object);
```


```js
function toArray(object){
	return Array.prototype.slice.call(object);
}

```

```js
if( !window.KIP) { KIP = {}; }
if(!KIP.toArray){
	KIP.toArray = function(object){
		return Array.prototype.slice.call(object);
	}
}
```

#E.3 filter 

filter 메서드는 배열을 순회하며 콜백함수를 호출하고 콜백함수에서 true를 반환하는 데이터만 새로운 배열에 담아 반환. 



```js
if( ![1].filter){
	Array.prototype.filter = function (callback){

	}
}

```
filter는 map 과 달리 반환하는 배열의 데이터 숫자가 원래 배열의 데이터 숫자와 같지는 않으므로 미리 배열 크기를 정해서는 안됨. 


```js
if( ![1].filter ){
	Array.prototype.filter = function(callback){
		var rtn = [];
	}
}

```

원래 배열을 순회하면서 각 데잍터마다 콜백을 호출하고 콜백에서 true를 반환하는 데이터를 rtn에 담아 반환 

if( ![1].filter ){
	Array.prototype.filter = function(callback){
		var rtn = [];
		for(var i=0; i< this.length; i++){
			if(callback (this[i]) ){
				rtn.push(this[i])
			}
		}
		return rtn;
	}
}












