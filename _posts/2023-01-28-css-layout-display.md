---
layout: single
title: "[Frontend] CSS Layout Display"

categories:
 - Frontend

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

## div, span 의 차이

div 의 경우 display : block 이 default 이기에 같은 줄에 표시가 안되는 것을 확인 할 수 있고  
span의 경우 display : inline 이 default 이기에 컨테이너의 width 따라 같은 줄에 표시가 되는 것을 확인 할 수 있다.

{% include codepen.html hash="XWBYoxv" title="div span" %}

이렇게 div, span 의 차이를 가져오는 display 속성에 대해 알아보고자 한다.

## display : block

block 은 기본적으로 새로운 줄에서 시작하며 모든 width 를 차지하는 형태로 표시되며,  
block 다음에 위치하는 요소는 다음 줄에서 표시되게 된다.  
block 이 기본인 태그는 div, p, h1~h6, form, header, footer, section, article, li 태그 등이 있다.  

{% include codepen.html hash="jOpRmBP" title="display : block" %}

## display : inline

inline 은 기본적으로 줄바꿈이 되지 않고, width, height, margin-top, margin-bottom, padding-top, padding-bottom 속성이 적용되지 않는다.  
inline 다음에 위치하는 요소는 기본적으로 바로 오른쪽에 표시되게 되며, 컨테이너의 width 에 따라 줄바꿈이 되게 된다.  
inline 이 기본인 태그는 span, a, img, input, button, label, select, textarea, strong, em, b, i, u, s, small, sub, sup, mark, del, ins, q, cite, dfn, abbr, time, code, var, samp, kbd, bdo, ruby, rt, rp, bdi 태그 등이 있다.

{% include codepen.html hash="WNKWOXE" title="display : inline" %}

## display : inline-block

## display : flex

## display : inline-flex

## display : none