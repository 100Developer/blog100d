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

.box1
```


