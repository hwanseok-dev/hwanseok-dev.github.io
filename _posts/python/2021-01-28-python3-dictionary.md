---
title: "[Python] 쉽게 정리한 Dictionary"
excerpt: "list를 dict의 key로 사용할 수 없는 이유, 그 대안"
date: 2021-01-28
last_modified_at:
categories:
  - Python
tags:
  - Python
  - dictionary
  - frozenset
  - tuple
  - dict2str
toc : true
toc_label: "Table of contents"
toc_icon: "list"  # corresponding Font Awesome icon name (without fa prefix)
toc_sticky: true
---

# Dictionary

dict는 어떤 값이 존재하는지 여부를 판단하기 위해서 사용합니다. key:value 쌍으로 값이 존재하는지와, 존재한다면 어떤 값을 가지는지 찾을 수 있습니다.  

dict의 key로는 integer, string, tuple을 사용할 수 있습니다. 하지만 list는사용할 수 없습니다.  

## list를 dict의 key로 사용하기 

list를 dict의 key로 사용하고 싶은데 아래와 같이 에러가 발생합니다.  

```python
t = {}
a = [1, 2, 3]
t[a] = 1
print(t) # TypeError: unhashable type: 'list'
```  

dict는 어떤 값이 존재하는지 판단하기 위해서 hash값을 이용한 $O(1)$ search를 합니다. list는 mutable ojbects이기 때문에 hash를 생성하기 위한 dict의 key로 사용할 수 없습니다.  

list는 값을 append하고 delete할 수 있습니다. list의 hash값을 생성한 이후 list의 element가 append,delete된다면 hash 값이 달라져서 hash 값을 찾을 수 없게 됩니다.  

## 대안

### str(dict)

```python
t = {}
a = [1, 2, 3]
t[str(a)] = 1
print(t)  # {'[1, 2, 3]': 1}
```

### tuple(dict)

```python
t = {}
a = [1, 2, 3]
t[tuple(a)] = 1
print(t)  # {(1, 2, 3): 1}
```

### frozenset(dict)

```python
t = {}
a = [1, 2, 3]
t[frozenset(a)] = 1
print(t)  # {frozenset({1, 2, 3}): 1}
```

# Reference

- <https://www.geeksforgeeks.org/how-to-use-a-list-as-a-key-of-a-dictionary-in-python-3/>
- <https://www.scaler.com/topics/python-dictionary-keys/>

**Success Notice:**
수고하셨습니다. :+1:
{: .notice--success}
