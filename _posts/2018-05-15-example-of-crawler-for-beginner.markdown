---
layout: post
title: "크롤러 작업에 대한 이해"
date: 2018-05-15 23:45:30 +0900
categories: jekyll update

---


# 크롤러를 이해해보자!!! 

- request란?
- html, css, javascript
- dom 구조 이해하기
- 크롤러의 동작 원리
    - request(요청해보기)
    - BeautifulSoup(파싱해보기)

---


## 1. request란? 



![Markdowm Image][1]


- 클라이언트(사용자)가 특정한 정보를 서버에 요청하는 작업.
- 이때, 특정한 정보를 __주소(URL)__로 요청함.
- 만약 우리가 네이버의 웹툰에 대한 요청을 보내고자 하면? 
- 한번 확인해보자 [링크](https://www.naver.com/)

---

- 이처럼 서버에 특정한 요청을 보내면 => 서버는 이에 맞는 응답을 줌
- 웹툰에 대한 요청을 보낸 후 서버로부터 받은 결과 값 [링크](http://comic.naver.com/index.nhn)
- 오른쪽 마우스 버튼 -> 검사 혹은 페이지 소스보기(크롬 브라우저 기준)

---
- 과연 우리는 서버가 준 정보를 해석할 수 있을까??????;;;;; 

> 불가능...불가능.... 그래서 중간에서 브라우저가 해석해서 우리가 눈으로 확인할 수 있는 형태로 변환해줌.

[브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)

* 지금까지의 내용을 정리해보면, 
* 우리는 특정한 정보를 얻기 위해 특정한 주소(URL)로 요청을 보냄.
* 서버에서는 사용자로부터 받은 요청에 알맞은 응답(html+css+javascirpt+알파)을 반환.
* 우리가 알아보기 힘든 html, css, javascript등의 문법을 브라우저가 해석해서 사용자가 평소 접하는 웹페이지 형태로 구현.

---


## 2. html, css, javascript 

- HTML 은 웹 페이지상에서 문단, 제목, 표, 이미지, 동영상등을 정의하고 그 구조와 의미를 부여하는 마크업 언어이다.

- CSS 는 배경색, 폰트, 컨텐츠의 레이아웃등을 지정하여, HTML 컨텐츠를 꾸며주는 스타일 규칙 언어이다.
- JavaScript 는 동적으로 컨텐츠를 바꾸고, 멀티미디어를 다루고, 움직이는 이미지등 웹 페이지를 꾸며주도록 하는 프로그래밍 언어이다. 물론, 전부는 아니지만 몇 줄만의 자바스크립트 코드만으로 꽤나 훌륭한 작품을 만들 수 있다.
- [출처 : MDN web Docs](https://developer.mozilla.org/ko/docs/Learn/JavaScript/First_steps/What_is_JavaScript)
- [예시코드](https://jsfiddle.net/akgaj7d7/1/)

---


## 3. dom 구조 이해하기(html)

## HTML(HyperText Markup Language)라고 한다. 

- html은 웹브라우저에 표시되는 글, 이미지 등을 표현하기 위해 "마크업"이라는 문법을 사용한다.
- head, title, body, header, p, div, span, img 등 다양한 태그들이 존재한다.

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <h3>This is a Heading</h3>
    <p>This is a paragraph.</p>
</body>
</html>
{% endhighlight %}

<h3>This is a Heading</h3>
<p>This is a paragraph.</p>



- 자세한 html의 구조에 대한 설명은 [링크](https://developer.mozilla.org/ko/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals)를 참조한다.

![Markdowm Image][2]
- photo from https://www.computerhope.com/jargon/d/dom.htm

- 간략히 표현하자면, html 정보는 위 사진과 같이 dom 구조를 가지고 있다(흔히, tree라고도 표현).

---


## 4. 크롤러의 동작원리 

- 우리가 크롤러라고 부르는 것은 크게 두가지 동작을 하는 프로그램을 말한다. 
- 첫째, __코드를 통해 서버에 특정 정보를 요청__
- 둘째, __서버로 부터 받은 복잡한 html에 구조에서 필요한 데이터를 추출__

__따라서, *크롤러란* 우리가 평소에 <U>클릭 혹은 주소 입력을 통해 특정 페이지에 
접근하는 것</U>과 접근해서 원하는 데이터를 <U>직접 드래그해서 복사하는 작업을 컴퓨터가 대신</U>해주는????__
> 글쓴이가 임의로 정의한 것이므로.... 정확한 정의는 아니다... 이해만 하자;;;;
그래서 우리는 파이썬의 라이브러리를 통해 서버에 정보를 요청하는 작업과 원하는 데이터를 추출하는 작업을 __코드화__ 해볼 것이다.

---


## 5. 파이썬을 이용한 크롤러 제작.
- 서버에 요청을 보내는 라이브러리 : requests 
- html에서 원하는 데이터를 추출하는 라이브러리 : BeautifulSoup4


---
## 6. 본 자료에서는 [네이버 웹툰 정보](http://comic.naver.com/webtoon/weekday.nhn)를 가져오는 크롤러를 만드는 작업을 진행해볼 것이다. 

### (1) 라이브러리 불러오기


```python
import requests
from bs4 import BeautifulSoup
```

### (2) url에 요청보내고, 응답 받아오기


```python
url = 'http://comic.naver.com/webtoon/weekday.nhn'

req = requests.get(url)
html = req.text
```

### (3) 원하는 데이터 추출가능하도록, 응답을 BeautifulSoup로 객체화시키기


```python
soup = BeautifulSoup(html, 'html.parser')
```

### (4) bs의 select 메소드를 통해 원하는 태그 접근하기
- 태그 속 id 속성은 '#', class 속성은 '.'로 접근.
- 구글 개발자 도구 혹은 검사(단축키-윈도우 기준 ctrl+shift+I )를 이용해서 css selector를 편하게 가져올 수 있음.
- 더 자세한 내용은 [링크](https://www.w3schools.com/cssref/css_selectors.asp)를 참조.


```python
webtoon_list = soup.select('#content > div.list_area.daily_all > div.col > div.col_inner > ul')
```


```python
list_of_webtoon_information = []
for webtoons in webtoon_list:
    for webtoon in webtoons.find_all('div'):
        list_of_webtoon_information.append((webtoon.find('a')['href'],  webtoon.find('img')['alt'])) # 튜플로 감싸서 리스트에 append하기
```

### (5) 사진 데이터(웹툰 썸네일) 저장하기
- 사진 정보가 저장된 url 찾기 -> 웹툰 예시에서는 a 태그 안 img 태그의 src 속성 참조.
- 해당 url로 사진 정보에 대한 요청 보내기(requests)
- 요청에 대한 응답 결과를 파일 저장 방식으로 저장하기(사진 데이터는 바이너리로 저장).
- 아래 코드는 하나의 url을 통해 특정 웹툰 이미지를 저장하는 예시.
- 추가 ) 위에서 사용한 코드를 활용해서 모든 웹툰의 이미지를 가져올 수 있는 코드를 작성해보자

---

- 'wb'? 바이너리 파일을 저장할 때 쓰는...
- https://ko.perlmaven.com/what-is-a-text-file 을 참고하면, 텍스트 데이터 외에 다른 속성 혹은 내용을 담고 있는 파일을 바이너리 파일(binary file)이라고 한다.


```python
img_url = 'http://thumb.comic.naver.net/webtoon/597478/thumbnail/thumbnail_IMAG10_487d19d8-3547-43a0-aa94-10ef7fc94cda.jpg'
f = open('webtoon_thumbnail/{}.jpg'.format('뷰티풀 군바리'), 'wb')
img_req = requests.get(img_url).content
f.write(img_req)
f.close()
```
---

- 참고 : [파일 입출력(python)](https://wikidocs.net/26)
[1]: http://acacha.org/svn/LinuxProgramacio/moodle/sessio5/imatges/cs-120-3.334.png
[2]: https://www.computerhope.com/jargon/d/dom1.jpg