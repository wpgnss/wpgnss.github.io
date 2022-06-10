---
layout: post
current: post
cover:  assets/blog/c-cpp-minecraft.png
navigation: True
title: C 문법 중 colon ':' 의 사용법 (struct bit field)
tags: [c_cpp]
class: post-template
subclass: 'post'
author: daniel
---

### 참고문서

[https://docs.microsoft.com/ko-kr/cpp/c-language/c-bit-fields](https://docs.microsoft.com/ko-kr/cpp/c-language/c-bit-fields)

### 예제
[예제코드](https://github.com/wpgnss/tips/tree/main/bit_field)

### colon : 의 사용법

코드를 보다가 구조체를 선언할 때 아래와 같은 콜론(:)이 사용된 것을 확인했다.

```c
#include <stdio.h>

typedef struct bit_field {
    unsigned int a : 2;
    unsigned int b : 10;
    unsigned int c : 32;
} BitField_t;
```

평소에 잘 사용하지 않는 문법이라 어떻게 사용하는지 찾아보다가 ‘**구조체 비트 필드**’라는 것을 알게 되었다

위 예제에서 사용된 콜론의 의미는 unsigned int(4byte) 중 지정된 bit 크기만큼 사용하겠다는 의미이다.

```c
#include <stdio.h>

typedef struct bit_field {
    unsigned int a : 2;
    unsigned int b : 10;
    unsigned int c : 32;
} BitField_t;

int main()
{
    BitField_t bf;

    bf.a = 10000;
    bf.b = 10000;
    bf.c = 10000;

    printf("%u\n", bf.a);    //  0:     10000 & b11 (2bit)
    printf("%u\n", bf.b);    //  784:   10000 & b1111111111 (10bit) 
    printf("%u\n", bf.c);    //  10000: 10000 & b1111.....1111 (32bit) 
	
	printf("struct sizeof (%d)\n", sizeof(BitField_t));
    return 0;
}
```

위 코드에서 bf.a, bf.b, bf.c 에 모두 값을 10000으로 할당 하더라도 출력을 해보면 모두 다른 값이 출력 된다.

![](assets/images/2022-06-11-C-문법-중-colon--의-사용법-struct-bit-field/Untitled.png)

이 값은 아래와 같은 계산에 의해 도출된다.

```c
integer 10000 은 bit로 10 0111 0001 0000 이다.
bf.a = b10011100010000 & b11 => b0 => 0
bf.b = b10011100010000 & b1111111111 => b1100010000 => 784
bf.c = b10011100010000 & b1111.....1111 => b10011100010000 => 10000
```

### sizeof struct

이렇게 선언한 bit field struct 를 sizeof 해보면 struct 의 sizeof 와 다른 점이 있다.

```c
typedef struct bit_field {
    unsigned int a : 2;
    unsigned int b : 10;
    unsigned int c : 32;
} BitField_t;

typedef struct normal_struct {
    unsigned int a;
    unsigned int b;
    unsigned int c;
} NormalStruct_t;
```

![](assets/images/2022-06-11-C-문법-중-colon--의-사용법-struct-bit-field/Untitled%201.png)

일반적으로 struct 에서 변수를 선언할 때, 변수 타입마다 정해진 크기의 메모리를 할당한다.

때문에 위 normal_struct 는 uint(4byte) 타입의 변수 3개를 생성했기 때문에 sizeof 12가 출력 된다.

그런대 bit field를 사용하여 선언하면, 하나의 uint 메모리에 여러 개의 변수가 선언되었다.

이는, bit field로 선언된 변수의 bit 합이 변수 타입의 크기 이하일 때만 해당된다.

위 예로, c : 32 로 선언된 경우 4byte(32bit) 를 모두 사용한다.

하지만 a : 2와 b : 10 은 4byte(32bit) 두 변수가 하나의 uint 메모리에 저장될 수 있기 때문에 실제로 두 개의 변수 a, b를 선언하는데 4byte를 사용하게 된다.

때문에 bitfield struct sizeof 는 2개의 uint 메모리를 사용하게 되어 8이 출력된다.

그리고, 아래와 같이 bit field와 일반 변수 선언을 함께할 수 있다.

```c
typedef struct mixed_struct {
    unsigned int a: 3;
    unsigned int b;
    unsigned int c;
} MixedStruct_t;
```