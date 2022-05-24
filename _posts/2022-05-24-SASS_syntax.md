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






