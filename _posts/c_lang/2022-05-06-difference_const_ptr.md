---
layout: post
current: post
cover:  assets/blog/c-cpp-minecraft.png
navigation: True
title: const 키워드 사용 정리 (int const *, int * const)
date: 2022-05-06 00:00:00
tags: [c_cpp]
class: post-template
subclass: 'post'
author: daniel
---


**const** 로 변수를 상수로 선언하기

`C` 자료형을 선언할 때 `const` 를 사용하면 변수를 상수화 할 수 있다.

~~~c
int var = 1;
const int con = 1;

var = 2; // ok
con = 2; // error
~~~


`const` 는 자료형 앞이나 뒤에 붙여도 상관이 없다.

`const int` == `int const`


이는 포인터를 사용할 때에도 마찬가지이다.

Read it backwards (as driven by [Clockwise/Spiral Rule](http://c-faq.com/decl/spiral.anderson.html)):

- `int*` - pointer to int
- `int const *` - pointer to const int
- `int * const` - const pointer to int
- `int const * const` - const pointer to const int

Now the first `const` can be on either side of the type so:

- `const int *` == `int const *`
- `const int * const` == `int const * const`

If you want to go really crazy you can do things like this:

- `int **` - pointer to pointer to int
- `int ** const` - a const pointer to a pointer to an int
- `int * const *` - a pointer to a const pointer to an int
- `int const **` - a pointer to a pointer to a const int
- `int * const * const` - a const pointer to a const pointer to an int