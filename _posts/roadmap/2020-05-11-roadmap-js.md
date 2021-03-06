---
title: "[Backend][RoadMap] Chap 1-3. Basic Frontend Knowledge - JavaScript"
excerpt: "github.com/kamranahmedse/developer-roadmap"
date: 2020-05-13
categories:
  - RoadMap
tags:
  - RoadMap 
  - roadmap

toc : true
toc_label: "=== Contents ==="
toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
classes: wide
---

# Syntax and basic Constructs

`ECMAScript`는 Ecma 인터내셔널에 의해 제정된 ECMA-262 기술 규격에 의해 정의된 범용 스크립트 언어입니다. `JavaScript`는 ECMAScript 사양을 준수하는 범용 스크립팅 언어입니다.
ECMAScript 6는 ECMA-262 표준의 제 6판이며, ECMAScript 사양의 주요 변경 사항 및 개선 사항을 명세합니다. 동의어로는 ‘`ES6`’, ‘`ES2015`’, ‘`ECMAScript 2015`’가 있습니다.  

## inline 방식

아래의 코드에서 onclick에 해당하는 부분이 javascript입니다. 

```html
<!DOCTYPE html>
<html>
<head>
    <style type="text/css">
        #selected{
            color:red;
        }
        .dark {
            background-color:black;
            color:white;
        }
        .dark #selected{
            color:yellow;
        }
    </style>
</head>
<body>
    <ul>
        <li>HTML</li>
        <li>CSS</li>
        <li id="selected">JavaScript</li>
    </ul>
    <input type="button" onclick="document.body.className='dark'" value="dark" />
</body>
</html>
```

## <script></script> 태그

```html
<!DOCTYPE html>
<html>
<body>
    <input type="button" id="hw" value="Hello world" />
    <script type="text/javascript">
        var hw = document.getElementById('hw');
        hw.addEventListener('click', function(){
            alert('Hello world');
        })
    </script>
</body>
</html>
```

## 외부 파일로 분리 : body tag

```html
<!DOCTYPE html>
<html>
<body>
    <input type="button" id="hw" value="Hello world" />
    <script type="text/javascript" src="script2.js"></script>
</body>
</html>
```

```javascript
//script2.js
var hw = document.getElementById('hw');
hw.addEventListener('click', function(){
    alert('Hello world');
})
```

## 외부 파일로 분리 : head tag

window.onload를 하지 않으면 `var hw = document.getElementById('hw');`에서 hw에 null 값이 들어갑니다.  onload 함수는 웹페이지에 있는 모든 코드가 읽히고, 이미지 등의 자원이 모두 다운된 뒤에 onload 함수가 호출되도록 정의되어 있습니다. 이러한 onload 함수를 재정의하는 부분이라고 생각하면 됩니다. 

```html
<!DOCTYPE html>
<html>
<head>
    <script src="script2.js"></script>
</head>
<body>
    <input type="button" id="hw" value="Hello world" />
</body>
</html>
```

```javascript
window.onload = function(){
    var hw = document.getElementById('hw');
    hw.addEventListener('click', function(){
        alert('Hello world');
    })
}
```

## Object Model

웹브라우저의 구성요소들은 하나하나가 객체화되어 있다. 자바스크립트로 이 객체를 제어해서 웹브라우저를 제어할 수 있게 된다. 이 객체들은 서로 계층적인 관계로 구조화되어 있다. BOM과 DOM은 이 구조를 구성하고 있는 가장 큰 틀의 분류라고 할 수 있다.  


브라우저의 여러 구성요소들을 javascript가 제어할 수 있는 object화 하는 것이 `object model`입니다. HTML을 object로 만드는 과정은 브라우저가 수행합니다.  

img tag를 제어한다고 하면 브라우저가 만들어 놓은 image object를 찾아야 합니다. 브라우저가 만들어 놓은 객체의 예시로는 document가 있습니다. 

```javascript
var imgs = document.getElementsByTagName('img');
imags[0].style.width = "300px";
//document.getElementsByTagName는 배열을 return 합니다.
```

![backend-6](/assets/images/backend/backend-6.jpg)  

1. DOM : Document Object Model
    - window는 전역 객체입니다. window.document 와 document는 완전히 같은 객체를 지시합니다.
    - 웹페이지의 내용을 제어합니다. window의 프로퍼티인 document 프로퍼터에 할당된 Document 객체가 이러한 작업을 담당합니다. 
    ```html
    <body>
        <input type="button" onclick="alert(window.location)" value="alert(window.location)" />
        <input type="button" onclick="window.open('bom.html')" value="window.open('bom.html')" />
    </body>
    ```
1. BOM : Brower Object Model
    - Browser가 제공하는 Object Model 입니다.
    - 웹페이지의 내용을 제외한 브라우저의 각종 요소들을 객체화시킨 것입니다. - 전역객체 Window의 프로퍼티에 속한 객체들이 이에 속합니다. 
    ```html
    <body>
        <img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/course/94.png" />
        <img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/course/94.png" />
        <img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/course/94.png" />
        <img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/course/94.png" />
        <img src="https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/course/94.png" />
        <script>
            // body 객체
            console.log(document.body);
            // 이미지 객체들의 리스트
            console.log(document.images);
            /*특정한 엘러먼트의 객체를 획득할 수 있는 메소드도 제공*/
            // body 객체
            console.log(document.getElementsByTagName('body')[0]);
            // 이미지 객체들의 리스트
            console.log(document.getElementsByTagName('body'));
        </script>
    </body>
    ```

## DOM 사용

```javascript
alert('Hello world');
window.alert('Hello world');

var a = 1;
alert(a);
alert(window.a);

var b = {id:1};
alert(a.id);
alert(window.a.id);
```

예제를 통해서 알 수 있는 것은 전역변수와 함수가 사실은 window 객체의 프로퍼티와 메소드라는 것입니다. 또한 모든 객체는 사실 window의 자식이라는 것도 알 수 있습니다. 이러한 특성을 ECMAScript에서는 Global 객체라고 부른다. ECMAScript의 Global 객체는 호스트 환경에 따라서 이름이 다르고 하는 역할이 조금씩 다르다. 웹브라우저 자바스크립트에서 Window 객체는 ECMAScript의 전역객체이면서 동시에 웹브라우저의 창이나 프레임을 제어하는 역할을 합니다.  

## 사용자와 커뮤니케이션 하기

```html
<!DOCTYPE html>
<html>
    <body>
        <input type="button" value="alert" onclick="alert('hello world');" />
    </body>
</html>
```

```html
<input type="button" value="confirm" onclick="func_confirm()" />
<script>
    function func_confirm(){
        if(confirm('ok?')){
            alert('ok');
        } else {
            alert('cancel');
        }
    }
</script>
```

## location

```javascript
//현재 윈도우의 URL 알아내기
console.log(location.toString(), location.href);
console.log(location.protocol, location.host, location.port, 
location.pathname, location.search, location.hash)

//현재 문서를 http://egoing.net으로 이동
location.href = 'http://egoing.net';
//같은 효과
location = 'http://egoing.net';
//현재 문서를 리로드
location.reload();
```

## navigator

```javascript
// 이 객체의 모든 프로퍼티를 열람할 수 있습니다.
console.dir(navigator);
```
- appName : 웹브라우저의 이름. IE는 Microsoft Internet Explorer, 파이어폭스, 크롬등은 Nescape로 표시
- appVersion : 브라우저의 버전을 의미
  ```
  "5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36"
  ```
- userAgent : 브라우저가 서버측으로 전송하는 USER-AGENT HTTP 헤더의 내용. appVersion과 비슷.
  ```
  "5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36"
  ```
- platform : 브라우저가 동작하고 있는 운영체제에 대한 정보

Navigator 객체는 브라우저 호환성을 위해서 주로 사용하지만 모든 브라우저에 대응하는 것은 쉬운 일이 아니므로 아래와 같이 기능 테스트를 사용하는 것이 더 선호되는 방법입니다.

예를 들어 Object.keys라는 메소드는 객체의 key 값을 배열로 리턴하는 Object의 메소드입니다. 이 메소드는 ECMAScript5에 추가되었기 때문에 오래된 자바스크립트와는 호환되지 않습니다. 아래의 코드를 통해서 호환성을 맞출 수 있습니다.   

```javascript
// From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys
if (!Object.keys) {
  Object.keys = (function () {
    'use strict';
    var hasOwnProperty = Object.prototype.hasOwnProperty,
        hasDontEnumBug = !({toString: null}).propertyIsEnumerable('toString'),
        dontEnums = [
          'toString',
          'toLocaleString',
          'valueOf',
          'hasOwnProperty',
          'isPrototypeOf',
          'propertyIsEnumerable',
          'constructor'
        ],
        dontEnumsLength = dontEnums.length;
 
    return function (obj) {
      if (typeof obj !== 'object' && (typeof obj !== 'function' || obj === null)) {
        throw new TypeError('Object.keys called on non-object');
      }
 
      var result = [], prop, i;
 
      for (prop in obj) {
        if (hasOwnProperty.call(obj, prop)) {
          result.push(prop);
        }
      }
 
      if (hasDontEnumBug) {
        for (i = 0; i < dontEnumsLength; i++) {
          if (hasOwnProperty.call(obj, dontEnums[i])) {
            result.push(dontEnums[i]);
          }
        }
      }
      return result;
    };
  }());
}
```

만약 브라우저마다 다르게 동작하는 기능이 있다면 navigator로 브라우저를 구분하고 조건문으로 동일하게 수행되도록 맞춰주는 작업이 필요합니다.  

## window control

[이 부분 강의 동영상](https://youtu.be/30PU5GYCb4A)  

window.open 메소드는 새 창을 생성합니다. 현대의 브라우저는 대부분 탭을 지원하기 때문에 window.open은 새 창을 만듭니다. 아래는 메소드의 사용법입니다.

```html
<!DOCTYPE html>
<html>
<style>li {padding:10px; list-style: none}</style>
<body>
<ul>
    <li>
        첫번째 인자는 새 창에 로드할 문서의 URL이다. 인자를 생략하면 이름이 붙지 않은 새 창이 만들어진다.<br />
        <input type="button" onclick="open1()" value="window.open('demo2.html');" />
    </li>
    <li>
        두번째 인자는 새 창의 이름이다. _self는 스크립트가 실행되는 창을 의미한다.<br />
        <input type="button" onclick="open2()" value="window.open('demo2.html', '_self');" />
    </li>
    <li>
        _blank는 새 창을 의미한다. <br />
        <input type="button" onclick="open3()" value="window.open('demo2.html', '_blank');" />
    </li>
    <li>
        창에 이름을 붙일 수 있다. open을 재실행 했을 때 동일한 이름의 창이 있다면 그곳으로 문서가 로드된다.<br />
        <input type="button" onclick="open4()" value="window.open('demo2.html', 'ot');" />
    </li>
    <li>
        세번재 인자는 새 창의 모양과 관련된 속성이 온다.<br />
        <input type="button" onclick="open5()" value="window.open('demo2.html', '_blank', 'width=200, height=200, resizable=yes');" />
    </li>
</ul>
 
<script>
function open1(){
    window.open('demo2.html');
}
function open2(){
    window.open('demo2.html', '_self');
}
function open3(){
    window.open('demo2.html', '_blank');
}
function open4(){
    window.open('demo2.html', 'ot');
}
function open5(){
    window.open('demo2.html', '_blank', 'width=200, height=200, resizable=no');
}
</script>
</body>
</html>
```

## HTMLElement

getElement* 메소드를 통해서 원하는 객체를 조회했다면 이 객체들을 대상으로 구체적인 작업을 처리해야 합니다. 이를 위해서는 획득한 객체가 무엇인지 알아야 합니다. 그래야 적절한 메소드나 프로퍼티를 사용할 수 있습니다.  

```html
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="active">JavaScript</li>
</ul>
<script>
    var li = document.getElementById('active');
    console.log(li.constructor.name);
    var lis = document.getElementsByTagName('li');
    console.log(lis.constructor.name);
</script>
```

```txt
//실행결과
HTMLLIElement 
HTMLCollection
//즉 실행결과가 하나인 경우 HTMLLIELement, 복수인 경우 HTMLCollection을 리턴하고 있다. 
```

```html
<a id="anchor" href="http://opentutorials.org">opentutorials</a>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li id="list">JavaScript</li>
</ul>
<input type="button" id="button" value="button" />
<script>
    var target = document.getElementById('list');
    console.log(target.constructor.name);
 
    var target = document.getElementById('anchor');
    console.log(target.constructor.name);
 
    var target = document.getElementById('button');
    console.log(target.constructor.name);
 
</script>
```

```txt
//실행결과
HTMLLIElement
HTMLAnchorElement
HTMLInputElement
```

엘리먼트 객체에 따라서 프로퍼티가 다릅니다. 하지만 모든 엘리먼트들은 HTMLElement를 상속 받고 있습니다.  

```txt
interface HTMLElement : Element {
    attribute DOMString       id;
    attribute DOMString       title;
    attribute DOMString       lang;
    attribute DOMString       dir;
    attribute DOMString       className;
};
```

![backend-7](/assets/images/backend/backend-7.jpg)  


## HTMLCollection

```javascript
console.group('before');
var lis = document.getElementsByTagName('li');
for(var i = 0; i < lis.length; i++){
    console.log(lis[i]);
}
console.groupEnd();
console.group('after');
lis[1].parentNode.removeChild(lis[1]);
for(var i = 0; i < lis.length; i++){
    console.log(lis[i]);
}
console.groupEnd();
```


# Learn DOM Manipulation

Document Object Model로 웹페이지를 자바스크립트로 제어하기 위한 객체 모델을 의미합니다. window 객체의 document 프로퍼티를 통해서 사용할 수 있습니다. Window 객체가 창을 의미한다면 Document 객체는 윈도우에 로드된 문서를 의미한다고 할 수 있습니다. DOM의 하위 수업에서는 문서를 제어하는 방법에 대한 내용을 다룹니다.  

문서를 자바스크립트로 제어하려면 제어의 대상에 해당되는 객체를 찾는 것이 제일 먼저 할 일입니다. 문서 내에서 객체를 찾는 방법은 document 객체의 메소드를 이용하는 것입니다. 

## document.getElementsByTagName

문서 내에서 특정 태그에 해당되는 객체를 찾는 방법은 여러가지가 있습니다. getElementsByTagName은 인자로 전달된 태그명에 해당하는 객체들을 찾아서 그 리스트를 `NodeList`라는 유사 배열에 담아서 반환합니다. NodeList는 배열은 아니지만 length와 배열접근연산자를 사용해서 엘리먼트를 조회할 수 있습니다.  

```html
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
<script>
    var lis = document.getElementsByTagName('li');
    for(var i=0; i < lis.length; i++){
        lis[i].style.color='red';   
    }
</script>
</body>
</html>
```

만약 조회의 대상을 좁히려면 아래와 같이 특정 객체를 지정하면 됩니다.  

```html
<!DOCTYPE html>
<html>
<body>
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ol>
<script>
    var ul = document.getElementsByTagName('ul')[0];
    var lis = ul.getElementsByTagName('li');
    for(var i=0; lis.length; i++){
        lis[i].style.color='red';   
    }

    //class를 기준으로 객체를 조회하기
    var lis = document.getElementsByClassName('active');
    for(var i=0; i < lis.length; i++){
        lis[i].style.color='red';   
    }

    //id 값을 기준으로 객체를 조회하기
    var li = document.getElementById('active');
    li.style.color='red';

    //css 선택자의 문법을 이용해서 제일 먼저 나타나는 객체를 조회하기
    var li = document.querySelector('li');
    li.style.color='red';
    var li = document.querySelector('.active');
    li.style.color='blue';

    //querySelector과 기본적인 동작방법은 같지만 모든 객체를 조회
    var lis = document.querySelectorAll('li');
    for(var name in lis){
        lis[name].style.color = 'blue';
    }
</script>
</body>
</html>
```

# jQuery

결과는 Body 태그 아래에 <h1>Hello world</h1> 코드가 만들어집니다. 아래와 같이 jQuery( document ).ready(function( $ ) {}로 감싸는 것이 이상적입니다.

```html
<!DOCTYPE html>
<html>
<body>
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
    <script>
    jQuery( document ).ready(function( $ ) {
      $('body').prepend('<h1>Hello world</h1>');
    });
    </script>
</body>
</html>
```

기본 문법은 단순합니다. $()는 jQuery의 함수입니다. 이 함수의 인자로 CSS 선택자(li)를 전달하면 jQuery 객체라는 것을 리턴합니다. 이 객체는 선택자에 해당하는 엘리먼트를 제어하는 다양한 메소드를 가지고 있습니다. 위의 그림에서 css는 선택자에 해당하는 객체들의 style에 color:red로 변경합니다.  

```javascript
$('li').css('color','red');
```

DOM에서 javascript로 작성하는 코드와 jQuery를 사용하는 코드입니다. 완전히 같은 기능을 수행합니다.  

```javascript
var li = document.getElementById('active');
li.style.color='red';
li.style.textDecoration='underline';
```

```javascript
$('$active').css('color', 'red').css('textDecoration', 'underline');
```

이처럼 코드 작성을 간결하게 하기 위해서 jQuery를 사용합니다. 

## jQuery 객체

jQuery 함수의 리턴값으로 jQuery 함수를 이용해서 선택한 엘리먼트들에 대해서 처리할 작업을 프로퍼티로 가지고 있는 객체입니다. jQuery 객체의 가장 중요한 특성은 암시적인 반복을 수행한다는 것입니다. DOM과 다르게 jQuery 객체의 메소드를 실행하면 선택된 엘리먼트 전체에 대해서 동시에 작업이 처리됩니다. 암시적 반복은 값을 설정할 때만 동작합니다. 값을 가져올 때는 선택된 엘리먼트 중 첫번째에 대한 값만을 반환합니다. 

//TODO 
# Learn Fetch API / Ajax(XHR)

# ES6 + and modular Javascript

# Hoisting

# Event Bubbling

# Scope

# Prototype

# Shadow DOM

# strict
