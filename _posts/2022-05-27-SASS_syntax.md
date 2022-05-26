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



