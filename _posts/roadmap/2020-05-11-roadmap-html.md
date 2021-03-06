---
title: "[Backend][RoadMap] Chap 1-1. Basic Frontend Knowledge - HTML"
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

# HTML(HyperText Markup Language)  

하이퍼텍스트를 가장 중요한 요소로 가지는 Marup라는 형식을 가진 프로그래밍 언어라는 의미입니다. HTML는 사용자와 컴퓨터의 약속이자 웹 페이지를 만드는 명령어입니다.  

## Learn the basics

```html
//진하게
<strong> 진하게 </strong>

//header 1 
<h1> header 1 </h1>

//새 탭으로 열기
<a href="www.github.com/niklasjang" target="_blank"> niklasjang </a> 

//리스트
<ol>                     //ordered list. list를 묶음
  <li>기술소개</li>       //list
  <li>기본문법</li>
  <li>하이퍼텍스트와 속성</li>
  <li>리스트와 태그의 중첩</li>
</ol>
<ul>                     //unordered list. list를 묶음
  <li>최진혁</li>
  <li>최유빈</li>
  <li>한이람</li>
  <li>한이은</li>
</ul>
```

//전체 구조
- html
  - head
    - title
    - meta
  - body

```html
<!-- //전체 구조 예시 -->
<!DOCTYPE html>
<html>
  <head>
    <title>HTML - 수업소개</title> //브라우저 탭의 이름
    <meta charset="utf-8"> //문자 인코딩
  </head>

  <body>
    <h1>HTML</h1>
    <ol>
      <li>기술소개</li>
      <li>기본문법</li>
      <li>하이퍼텍스트와 속성</li>
      <li>리스트와 태그의 중첩</li>
    </ol>
    
    <h2>선행학습</h2>
    
    본 수업을 효과적으로 수행하기 위해서는 웹애플리케이션에 대한 전반적인 이해가 필요합니다. 이를 위해서 준비된 수업은 아래 링크를 통해서 접근하실 수 있습니다. 
  </body>
</html>
```

```htmp
// 어떤 html ㅍ표준에 따라 작성된 문서인지 명시
// 최근 HTML5 표준이라면 아래와 같음
<!DOCTYPE html>
```

```htmp
//같은 디렉터리에 있는 html 파일을 열람
<ol>
  <li><a href="1.html">기술소개</a></li>
  <li><a href="2.html">기본문법<a/></li>
  <li><a href="3.html">하이퍼텍스트와 속성</a></li>
  <li><a href="4.html">리스트와 태그의 중첩</a></li>
</ol>
```

```htmp
<form action="http://localhost/login.php"> //제출된 쿼리스트링이 전송될 위치
    <p>아이디 : <input type="text" name="id"></p>
    <p>비밀번호 : <input type="password" name="pwd"></p>
    <p>주소 : <input type="text" name="address"></p>
    <input type="submit">
</form>

//text area
<form action="">
    <p>text : <input type="text" name="id" value="default value"></p>
    <p>password : <input type="password" name="pwd" value="default value"></p>
    <p>textarea :
        <textarea cols="50" rows="2">default value</textarea>
    </p>
</form>


//dropdown list
<form action="http://localhost/color.php">
    <h1>색상</h1>
    <select name="color">
        <option value="red">붉은색</option>
        <option value="black">검은색</option>
        <option value="blue">파란색</option>
    </select>
    <h1>색상2 (다중선택)</h1>
    <select name="color2" multiple>
        <option value="red">붉은색</option>
        <option value="black">검은색</option>
        <option value="blue">파란색</option>
    </select>
    <input type="submit">
</form>


//버튼 및 초기화
<form action="http://localhost/form.php">
    <input type="text">
    <input type="submit" value="전송">
    <input type="button" value="버튼" onclick="alert('hello world')">
    <input type="reset">
</form>

//라벨, 숨겨진 값
//text가 input tag의 label인 것을 알려줌
<form action="http://localhost/hidden.php">
    <label for="id_txt">text</label> <input id="id_txt" type="text" name="id"> 
    <input type="hidden" name="hide" value="egoing">
    <input type="submit">
</form>
```

```text
//GET과 POST
<form action="http://localhost/method.php" method="post">
    <input type="text" name="id">
    <input type="password" name="pwd">
    <input type="submit">
</form>

//파일 업로드
<form 
  action="http://localhost/upload.php"
  method="post" 
  enctype="multipart/form-data">
    <input type="file" name="profile">
    <input type="submit">
</form>
```

```htmp
<html>
  <head>
    <meta charset="utf-8">
    <meta name="description" content="브라우저 검색시 먼저 나타나는 요약 정보">
    <meta name="keywords" content="코딩,coding,niklasjang,github,블로그를 설명하는 단어들">
    <meta name="author" content="niklasjang">
    <meta http-equiv="refresh" content="30"> //30초 마다 자동 refresh
  </head>
</html>
```

## Writing Semantic HTML

문서의 정보를 보다 잘 표현하기 위해서는 의미에 맞는 태그를 잘 사용해야 합니다. 특히 HTML5에서는 웹페이지에서 통상 많이 사용하는 구조에 의미를 분명히 부여하기 위해서 의미론적 태그(semantic element)를 새롭게 정의해서 제공하고 있습니다. 그 목록은 아래와 같습니다. 단 아래의 태그들은 기능은 없습니다.  

- article	본문
- aside	광고와 같이 페이지의 내용과는 관계가 적은 내용들
- details	기본적으로 표시되지 화면에 표시되지 않는 정보들을 정의
- figure	삽화나 다이어그램과 같은 부가적인 요소를 정의
- footer	화면의 하단에 위치하는 사이트나 문서의 전체적인 정보를 정의
- header	화면의 상단에 위치하는 사이트나 문서의 전체적인 정보를 정의
- main	문서에서 가장 중심이 되는 컨텐츠를 정의
- mark	참조나 하이라이트 표시를 필요로 하는 문자를 정의
- nav	문서의 네비게이션 항목을 정의
- section	문서의 구획들을 정의 (참고)
- time	시간을 정의

## Forms and Validations

웹 페이지에서 form은 사용자에게 자세한 정보를 얻기 위해서 사용됩니다. 사용자에 의해서 입력된 정보는 서버로 전송됩니다. 일반적인 HTML form의 형식은 아래와 같습니다.  

```html

<body> 
<h1 style="text-align: center"> REGISTRATION FORM </h1>           
<form name="RegForm" action="/submit.php" onsubmit="return GEEKFORGEEKS()" method="post">  
      
    <p>Name: <input type="text" size=65 name="Name"> </p><br>        
    <p> Address: <input type="text" size=65 name="Address">  </p><br> 
    <p>E-mail Address: <input type="text" size=65 name="EMail">  </p><br> 
     <p>Password: <input type="text" size=65 name="Password"> </p><br> 
    <p>Telephone: <input type="text" size=65 name="Telephone"> </p><br>   
           
    <p>SELECT YOUR COURSE    
        <select type="text" value="" name="Subject"> 
            <option>BTECH</option> 
            <option>BBA</option> 
            <option>BCA</option> 
            <option>B.COM</option> 
            <option>GEEKFORGEEKS</option> 
        </select></p><br><br> 
    <p>Comments:  <textarea cols="55" name="Comment">  </textarea></p> 
    <p><input type="submit" value="send" name="Submit">      
        <input type="reset" value="Reset" name="Reset">   
    </p>          
</form> 
</body>
```

form에 입력되는 데이터를 정확한 포멧을 가지고 이어야하며, 특정한 필드는 반드시 채워져야합니다. javascript를 통해서 validation을 진행하는 코드의 예시는 아래와 같습니다.  

```html
<html> 
<head> 
<script> 
function GEEKFORGEEKS()                                    
{ 
    var name = document.forms["RegForm"]["Name"];               
    var email = document.forms["RegForm"]["EMail"];    
    var phone = document.forms["RegForm"]["Telephone"];  
    var what =  document.forms["RegForm"]["Subject"];  
    var password = document.forms["RegForm"]["Password"];  
    var address = document.forms["RegForm"]["Address"];  
   
    if (name.value == "")                                  
    { 
        window.alert("Please enter your name."); 
        name.focus(); 
        return false; 
    } 
   
    if (address.value == "")                               
    { 
        window.alert("Please enter your address."); 
        address.focus(); 
        return false; 
    } 
       
    if (email.value == "")                                   
    { 
        window.alert("Please enter a valid e-mail address."); 
        email.focus(); 
        return false; 
    } 
   
    if (phone.value == "")                           
    { 
        window.alert("Please enter your telephone number."); 
        phone.focus(); 
        return false; 
    } 
   
    if (password.value == "")                        
    { 
        window.alert("Please enter your password"); 
        password.focus(); 
        return false; 
    } 
   
    if (what.selectedIndex < 1)                  
    { 
        alert("Please enter your course."); 
        what.focus(); 
        return false; 
    } 
   
    return true; 
}</script> 
  
<style> 
GEEKFORGEEKS {                                         
    margin-left: 70px; 
    font-weight: bold ; 
    float: left; 
    clear: left; 
    width: 100px; 
    text-align: left; 
    margin-right: 10px; 
    font-family:sans-serif,bold, Arial, Helvetica; 
    font-size:14px; 
} 
   
div {  
    box-sizing: border-box; 
    width: 100%; 
    border: 100px solid black; 
    float: left; 
    align-content: center; 
    align-items: center; 
} 
   
form {                                         
    margin: 0 auto; 
    width: 600px; 
}</style>
</head> 
   
<body> 
<h1 style="text-align: center"> REGISTRATION FORM </h1>           
<form name="RegForm" action="/submit.php" onsubmit="return GEEKFORGEEKS()" method="post">  
      
    <p>Name: <input type="text" size=65 name="Name"> </p><br>        
    <p> Address: <input type="text" size=65 name="Address">  </p><br> 
    <p>E-mail Address: <input type="text" size=65 name="EMail">  </p><br> 
     <p>Password: <input type="text" size=65 name="Password"> </p><br> 
    <p>Telephone: <input type="text" size=65 name="Telephone"> </p><br>   
           
    <p>SELECT YOUR COURSE    
        <select type="text" value="" name="Subject"> 
            <option>BTECH</option> 
            <option>BBA</option> 
            <option>BCA</option> 
            <option>B.COM</option> 
            <option>GEEKFORGEEKS</option> 
        </select></p><br><br> 
    <p>Comments:  <textarea cols="55" name="Comment">  </textarea></p> 
    <p><input type="submit" value="send" name="Submit">      
        <input type="reset" value="Reset" name="Reset">   
    </p>          
</form> 
</body> 
</html> 
```

## Conventions and Best Practices

1. Use proper document structure
  ```txt
  <!DOCTYPE html>
  <html>
    <head>
      <title></title> 
      <meta charset="utf-8"> 
    </head>
    <body>
    </body>
  </html>
  ```
1. Declare the correct doctype
1. Always close tags
1. Don't use inline styles
1. Use alt attribute with images : image에 설명을 넣는 태그를 꼭 작성합니다.
1. Validate frequently
1. Place external style sheets within the <head> tag
1. Use meaningful tags
1. Use lowercase markup
1. Reduce the number of elements on a page

## Accessibility

HTML을 작성할 때 접근성을 생각해서 작성해야합니다. 웹 페이지를 navigate하 때 좋은 방법을 생각하고, semactic HTML tag를 사용합니다. 이해하기 쉽고 사용하기 쉽게하기 위함입니다. 

## SEO Basics(search engine optimization)  

검색 로봇들이 사이트의 특징을 잡고 상단에 노출되로록 하는 과정을 의미합니다. 검색엔진 최적화는 의미론적인 html을 작성하는 과정을 포함합니다. 짚고 넘어갈 점은 검색엔진최적화는 구글의 광고 영역에는 영향을 미치지 않고 본문의 부분에만 영향을 미칩니다. 최적화가 잘 되어도 광고글이 먼저 상단에 노출됩니다.  

1. title tag를 위한 권장 사항
  - 페이지의 콘텐츠를 정확하게 설명하는 제목
  - 각 페이지마다 다른 title 사용하기
1. description meta tag를 사용하기
  - 검색시 title 밑에 나타나는 내용
1. URL 구조를 개선하기
  - 이해하기 쉬운 URL을 사용하기
  - 검색엔진이 문서를 크롤링하기도 쉬워집니다.
  - 알수 없는 매개변수를 줄이기
  - 단순한 디렉토리 구조 만들기. 너무 깊은 dir 피하기
  - 같은 내용을 갖고 있는 다수의 html에서는 원본 html만 제외하고 나머지에서는 아래와 같이 작성하기. canonical은 "내가 진짜야"라는 의미입니다. 
  ```html
  <head>
    <link rel="canonical" href="http://localhost/1.html"/>
  </head> 
  ```
  - 만약 redirection을 적용할 수 있다면 redir을 사용하는 것이 더 좋습니다. 
1. 사이트 내에서 이동하기 쉽게 만들기
  - Not Found 404 말고 다른 페이지로 이동할 수 있는 경로 보여주기
1. 검색 엔진을 위한 것이 아닌 사용자를 위한 콘텐츠 작성
  - description에 과도한 정보를 넣지 않기
1. 앵커 텍스트를 위한 권장 사항(\<a>)
  - 내용을 함축적으로 사용하는 링크 이름 부여
1. img tag 사용시 alt 태그 사용하기
1. image 자료는 images와 같은 일반적인 이름의 폴더 안에 위치하기
1. 제목 태그의 적절한 활용(h1 ~ h6)
1. robots.txt를 효과적으로 활용하기
  - www.google.com/robots.txt
  - 접속할 페이지와 접속하지 않을 페이지를 구분해서 정의하기
1. sitemap 활용하기
  - www.google.com/sitemap
  - 기계가 이해하기 쉬운 형태로 하이퍼링크를 정리해서 보여주기
1. page rank
  - 똑같은 단어를 두 개의 페이지에 있을 때 랭킹이 높은 사이트를 먼저 보여줍니다. 
  - 페이지 A를 참조하고 있는 사이트가 많으면 페이지 A의 page rank가 높아집니다. 그리고 이 A 페이지가 참조하는 B 페이지는 page rank가 많이 올라갑니다.