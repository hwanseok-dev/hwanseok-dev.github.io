---
title: "[Django] *args와 **kwargs의 차이점"
excerpt: "가변 (키워드)인자의 사용법"
date: 2021-01-13
last_modified_at: 
categories:
  - Django
tags:
  - Django
  - args
  - kwargs  
  - '*args'
  - '**kwargs'
toc : true
toc_label: "Table of contents"
toc_icon: "list"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: false
---

**Info Notice:**  
안녕하세요. **HwanSeok**입니다.  
본 포스팅은 django의 기초 개념을 다지는 포스팅입니다.  
{: .notice--info}

## 필요성

The Polls 프로젝트를 진행하다가 [여기][1]에서 `reverse()`에 대한 설명을 보았습니다. reverse()는 args와 kwargs를 인자로 받을 수 있는데 두 개를 동시에 받을 수는 없다고 나옵니다.

```
args and kwargs cannot be passed to reverse() at the same time.
```  

그 이유를 알아보기 위해 [여기][2]를 참고했습니다.  

## kwarg

키워드 args는 지정된 키워드를 가진 인자를 말합니다.  

```python
foo(Firstname="HwanSeok")
```  

키워드 인자의 개념은 간단한데, 가변인자의 개념과 결합되면 다음과 같은 규칙을 따라야 합니다.  

## variable length non- keyword argument : `*args`

아래와 같이 정의해둔 argument의 갯수보다 많은 인자를 받을 수는 없습니다.   

```python
def adder(x,y,z):
    print("sum:",x+y+z)

adder(5,10,15,20,25)
```

이러한 경우 가변인자 `*args`를 사용할 수 있습니다. \*(is called asterisk)를 사용하면, 인자들이 tuple로 전달이 됩니다. 그리고 전달된 인자들이 함수 안에서 같은 이름을 가진 tuple을 생성합니다.  

```python
def adder(*num):
    sum = 0
    
    for n in num:
        sum = sum + n

    print("Sum:",sum)

adder(3,5) # Sum : 8
adder(4,5,6,7) # Sum : 22
adder(1,2,3,5,6) # Sum : 17
```

## variable length keyword argument : `**kwargs`

가변 키워드 인자는 `*args`가 아닌 `**kwargs`를 사용해야합니다. \*\*를 사용하면, 인자들이 dictinary로 전달됩니다. 그리고 전달된 인자들이 함수 안에서 같은 이름을 가진 dictionary를 생성합니다. 

```python
def intro(**data):
    print("\nData type of argument:",type(data))

    for key, value in data.items():
        print("{} is {}".format(key,value))

intro(Firstname="Sita", Lastname="Sharma", Age=22, Phone=1234567890)
intro(Firstname="John", Lastname="Wood", Email="johnwood@nomail.com", Country="Wakanda", Age=25, Phone=9876543210)
```

```txt
# output
Data type of argument: <class 'dict'>
Firstname is Sita
Lastname is Sharma
Age is 22
Phone is 1234567890

Data type of argument: <class 'dict'>
Firstname is John
Lastname is Wood
Email is johnwood@nomail.com
Country is Wakanda
Age is 25
Phone is 9876543210
```  

## Reference

- [args-and-kwargs][2]
- [Args And Kwargs in Python][3]

**Success Notice:**  
수고하셨습니다. :+1:
{: .notice--success}

[1]: https://docs.djangoproject.com/ko/3.1/ref/urlresolvers/#reverse
[2]: https://www.programiz.com/python-programming/args-and-kwargs
[3]: https://www.scaler.com/topics/python/args-and-kwargs-in-python/
