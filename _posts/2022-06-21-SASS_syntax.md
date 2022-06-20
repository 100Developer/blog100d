---
toc: true
layout: post
description: SASS 문법
categories: SASS syntax
title: SASS 문법
---

### 주석
```html
// 컴파일되지 않는 주석
/* 컴파일되는 주석 */

```
---

### 중첩
```scss
.btn{
  position:absolute;
  &.action{
    color:red;
  }
}
```

```css
.btn{
  position:absolute;
}
.btn.active{
  color:red;
}

```
---

### 중첩 벗어나기
```scss
.list{
  $w:100px;
  $h:50px;
  li{
    width:$w;
    height:$h;
  }
  @at-root .box{
    width:$w;
    height:$h;
  }
}
```
```css
.list li{
  width:100px;
  height:50px;
}
.box{
  width:100px;
  height:50px;
}

```

---

### 중첩된 속성
```scss
  .box{
    font:{
      weight:bold;
      size:10px;
      family:sans-serif;
    };
    margin: {
      top:10px;
      left:20px;
    };
    padding: {
      bottom:40px;
      right:30px;
    }
  }
```

---

### 변수
> $변수이름 : 속성값;

```scss
$color-primary:#444444;
$url-image:"assets/images/";
$w:200px;

.box{
  width:$w;
  margin-left:$w;
  background:$color-primary url($url-image + "bg.jpg");
}

```

---

### 변수 유효범위

```scss
.box1{
  $color:#111;
  background:$color;
}
//Error
.box2{
  background:$color;
}
```

---

### 변수 재할당

```scss
$red:#ff0000;
$blue:#0000ff;

$color-primary:$blue;
$color-danger:$red;

.box{
  color:$color-primary;
  background:$color-danger;
}
```

---

### 변수 전역 설정

```scss
.box1{
  $color:#111 !global;
  background:$color;
}

.box2{
  background:$color;
}

```

---

### 변수 초깃값 설정

```scss
$color-primary:red;
.box{
  $color-primary:blue !default;
  background:$color-primary;
}
```

---

### 변수 문자보간

```scss
$family: unquote("Droid_Sans");
@import url("http://fonts.googleapis.com/css?family=#{family}")
```
> 내장함수 unquote()는 문자에서 따옴표를 제거합니다.

---

### 가져오기

> @import로 외부에서 가져온 Sass 파일은 모두 단일 CSS 출력 파일로 **병합**됩니다. 
> 또한 가져온 파일에 정의된 모든 변수 또는 Mixins 등을 주 파일에서 사용할 수 있습니다.

```scss
@import "hello.css";
@import "http://hello.com/hello";
@import url(hello);
@import "hello" screen;
```

---

### 여러파일 가져오기, 파일 분할

```scss
@import "header", "footer"
```
> 컴파일시, 분리되어서 컴파일이 된다.
```scss
@import "_header", "_footer"
```

---

### 연산 -숫자
> 일반적으로 절대값을 나타내는 PX 단위로 연산을 합니다. 
> 상대적 단위 (%, em, vw등)의 연산의 경우 CSS calc()로 연산해야 합니다.
```scss
width:calc(50%-20px)
```
> 나누기 연산은 ()로 한다
```scss
height:(100px /2)
/* 추가 가능 */
$x:100px;
//변수시 가능
width:$w /2;
//2개 이상 연산시 가능
font-size:10px +12px /3
```

---

### 연산 -문자
> 문자 연산에는 +가 사용됩니다.
> 문자 연산의 결과는 첫번째 피연산자를 기준으로 합니다. 
> 첫 번째 피연산자에 따옴표가 붙어있다면 연산 결과는 따옴표로 묶습니다.
> 반대로 첫 번째 피연산자에 따옴표가 붙어있지 않다면 연산 결과도 따옴표를 처리하지 않습니다.
```scss
div::after{
  content:"Hello"+World;
  flex-flow: row +"-reverse"+""+wrap
}
```
```css
div::after{
  content:"Hello World"
  flex-flow:row-reverse wrap;
}
```


---

### 연산 -논리
```scss
$w:100px;
.item{
  display:block;
  @if($w > 50px or $w <90px){
    width:400px;
  }
}
```


---

### 재활용 -Mixin, Include01
```scss
@mixin size ($w:100px, $h:100px){
  width:$w;
  height:$h;
}

.box1{
  @include:size;
}
.box2{
  @include:size($h:300px);
}
```

```css
.box1{
  width:100px;
  height:100px;
}
.box2{
  width:100px;
  height:100px;
}
```


---

### 재활용 -Mixin, Include02
```scss
@mixin 믹스인이름{
  스타일;
}

@mixin large-text{
  font-size:22px;
  font-weight:bold;
  font-family:sans-serif;
  color:orange;
}
```

```scss
@mixin large-text{
  font:{
    size:22px;
    weight:bold;
    family:sans-serif;
  }
  color:orange;
  &::after{
    content:"!!";
  }
  span.icon{
    background:url("/images/icon.png");
  }
}
.box1{
  @include large-text;
}
```

```css
.box1{
  font-size:22px;
  font-weight:bold;
  font-family:sans-serif;
  color:orange;
}

.box1::after{
  content:"!!";
}

.box1 span.icon{
  background:url("/images/icon.png");
}
```

---

### 재활용 -인수
>개념

```scss
@mixin 믹스인이름($매개변수:parameters){
  스타일;
}

@include 믹스인이름(인수);
```

```scss
@mixin dash-line($width, $color){
  border:$width dashed $color;
}

.box1{ @include dash-line(1px, red);}
.box2{ @include dash-line(4px, blue);}
```

```css
.box1{ 
  border:1px dashed red;
}
.box2{ 
  border:4px dashed blue;
}
```
---

### 재활용 -인수 -기본값 설정
>개념

```scss
@mixin 믹스인이름($매개변수:기본값){
  스타일;
}

@include 믹스인이름(인수);
```

```scss
@mixin dash-line($width:1px, $color:black){
  border:$width dashed $color;
}

.box1{ @include dash-line;}
.box2{ @include dash-line(4px);}
```

```css
.box1{ 
  border:1px dashed black;
}
.box2{ 
  border:4px dashed black;
}
```
---

### 재활용 -인수 -키워드 인수
>개념

```scss
@mixin 믹스인이름($매개변수A:기본값, $매개변수B:기본값){
  스타일;
}

@include 믹스인이름($매개변수B:인수);
```
---

### 재활용 -인수 -가변인수
>개념

```scss
@mixin 믹스인이름($매개변수...){
  스타일;
}

@include 믹스인이름(인수A, 인수B, 인수C);
```

```scss
@mixin var($w, $h, $bg...){
  width:$w;
  height:$h;
  background:$bg;
}

.box{
  @include var(
    100px,
    200px,
    url("image/a.png") no-repeat 10px 20px,
    url("image/b.png") no-repeat,
  ); 
}
```

```css
.box{ 
  width:100px;
  height:200px;
  background:url("image/a.png") no-repeat 10px 20px, url("image/b.png") no-repeat,
}
```

```scss
@mixin font(
  $style:normal,
  $weight:normal,
  $size:16px,
  $family:sans-serif
){
  font:{
    style:$style;
    weight:$weight;
    size:$size;
    family:$%family;
  }
}

div{
  //매개변수 순서와 개수에 맞게 전달
  $font-value:italic, bold, 16px, sans-serif;
  @include font($font-value...);
}
span{
  //필요한 값만 키워드 인수로 변수에 담아 전달
  $font-value:(style:italic, size:22px);
  @include font($font-value...);
}
a{
  //필요한 값만 키워드 인수로 전달
  @include font((weight:900, family:monospace)...);
}

```

```css
div{
	font-style:italic;
	font-weight:bold;
	font-size:16px;
	font-family:sans-serif;
}
span{
	font-style:italic;
	font-weight:normal;
	font-size:22px;
	font-family:sans-serif;
}
a{
	font-style:normal;
	font-weight:900;
	font-size:16px;
	font-family:monospace;
}

```

---

### 재활용 -Content
>개념

```scss
@mixin 믹스인이름(){
  스타일;
  @content;
}

@include 믹스인이름(인수){
  //스타일 블록
}
```

```scss
@mixin icon($url){
  &::after{
    content:$url;
    @content;
  }
}

.box1{
  @include icon("image/icon1.png");
}
.box2{
  @inlcude icon("image/icon2.png"){
    display:block;
    position:absolute;
    width:100px;
    height:100px;
  }
}

```

```css
.box1::afte{
	content:"image/icon1.png";
}
.box2::after{
	content:"image/icon2.png";
	display:block;
	position:absolute;
	width:100px;
	height:100px;
}
```
---

### 재활용 -Content
>개념

```scss
@mixin 믹스인이름(){
  스타일;
  @content;
}

@include 믹스인이름(인수){
  //스타일 블록
}
```

```scss
@mixin icon($url){
  &::after{
    content:$url;
    @content;
  }
}

.box1{
  @include icon("image/icon1.png");
}
.box2{
  @inlcude icon("image/icon2.png"){
    display:block;
    position:absolute;
    width:100px;
    height:100px;
  }
}

```

```css
.box1::afte{
	content:"image/icon1.png";
}
.box2::after{
	content:"image/icon2.png";
	display:block;
	position:absolute;
	width:100px;
	height:100px;
}
```
---

### 확장 -Extend
>개념

```scss
@extend 선택자;
```

```scss
.btn{
	padding:10px;
	margin:10px;
	background:blue;
}

.btn-danger{
	@extend .btn;
	background:red;
}

```

```css
.btn, .btn-danger{
	padding:10px;
	margin:10px;
	background:blue;
}

.btn-danger{
	background:red;
}
```
---

### 확장 -추천하지 않는 이유

```scss
.container{
	width:300px;
	height:300px;
	background:red;
	.item{
		width:200px;
		height:200px;
		background:blue;
		.icon{
			width:100px;
			height:100px;
			background:green;
		}
	}
}

.wrapper{
	.new-icon{
		@extend .icon;
	}
}

```

```css
.container{
	width:300px;
	height:300px;
	background:red;
}

.container .item{
	width:200px;
		height:200px;
		background:blue;
}

.container .item .icon, .container .item .wrapper .new-icon, .wrapper .container .item .new-icon{

	width:100px;
			height:100px;
			background:green;
}
```
---

### 함수 - IF 

> if(조건, 표현식1, 표현식2)
> 삼항 함수와 비슷


```scss
$width:550px;
div{
	width:if($width > 300px, $width, null)
}
```

```css
div{
	width:550px;
}
```

---

### 조건문 - IF(지시어) 

> @if 지시어는 조건에 따른 분기 처리가 가능하며, if문(if statements)과 유사합니다.
> 같이 사용할 수 있는 지시어는 @else if 가 있습니다.
> 추가 지시어를 사용하면 좀 더 복잡한 조건문을 작성할 수 있습니다.


```scss
$bg:true;
div{
	@if $bg{
		background:url("/images/a.jpg");
	}
}
```
```scss
$color:orange;
div{
	@if $color == strawberry{
		color:#fe2e2e
	} @else if $color == orange{
		color:#fe9a2e;
	} @else if $color == banana{
		color:#ffff00;
	} @else {
		color:#2a1b0a;
	}
}
```


```css
div{
	color:#fe9a2e;
}
```
---

### 조건문 - IF - 예제

```scss
	@function limitSize($size){
		@if ($size >= 0 and $size <= 200px){
			@return 200px;
		} @else {
			@return 800px;
		}
	}
	
	div{
		width:limitSize(180px);
		height:limitSize(340px);
	}
```

```css
div{
	width:200px;
	height:800px;
}
```

```scss
	@mixin positionCenter($w, $h, $p:absolute){
		@if(
			$p == absolute
			or $p == fixed
			or not $p == relative
			or not $p == static
		){
			width:if(unitless($w), #{$w}px, $w);
			height:if(unitless($h), #{$h}px, $h);
			position:$p;
			top:0;
			bottom:0;
			left:0;
			right:0;
			margin:auto;
		}
	}
	
	.box1{
		@include positionCenter(10px, 20px);
	}
	.box2{
		@include positionCenter(50, 50, fixed);
	}
	.box3{
		@include positionCenter(100, 200, relative);
	}
	
```
---
