---
title: Django ORM 1+N문제
date: 2021-12-03 23:55:00 +0900
categories: [Tip, Django]
tags: [django]
math: true
toc: true
mermaid: true
comments: true
image:
  width: 800
  height: 500
pin: true
---

# ORM N+1 문제
django ORM은 Lazy-Loading 방식입니다. Lazy-Loading 방식을 사용하면 ORM에서 명령을 실행할 때마다 데이터베이스에서 데이터를 가져오는 것이 아니라 실제로 데이터를 불러와야 할 시점에 데이터베이스에 쿼리를 실행하는 방식입니다.

그래서 다음과 같이 QRM를 통해서 db의 data를 접근하면, `n+1`번 쿼리를 실행하는 것을 확인 할 수 있습니다. 


```python
In [171]: dogs2 = Dog.objects.filter(id__gt=65540)

# owner가 외레키
In [172]: for dog in dogs2:
             print("Excute querry")
     ...:    dog.owner.name
     ...:
```

이 예시에서는 `2`건의 data를 얻을때, 총 `3`번의 쿼리를 실행했습니다 `(2+1)`

한번의 호출로 N개의 모델을 가져오고, for loop를 돌면서 실제 데이터를 불러와야되는 시점에 `1개의 row`를 가져오는 쿼리가 실행되는 것을 확인할 수 있습니다.


```bash
(0.001) SELECT `dogs`.`id`, `dogs`.`owner_id`, `dogs`.`name`, `dogs`.`age` FROM `dogs` WHERE `dogs`.`id` > 65540; args=(65540,)

Excute querry
(0.000) SELECT `owners`.`id`, `owners`.`name`, `owners`.`email`, `owners`.`age` FROM `owners` WHERE `owners`.`id` = 17 LIMIT 21; args=(17,)

Excute querry
(0.000) SELECT `owners`.`id`, `owners`.`name`, `owners`.`email`, `owners`.`age` FROM `owners` WHERE `owners`.`id` = 17 LIMIT 21; args=(17,)
```

그러기 때문에, 이러한 방식은 불러들여야 되는 Row수가 많을 수록 쿼리문 실행 횟수가 많아집니다.


# Eager-Loading ?
Eager-Loading방식은 사전에 쓸 데이터를 포함하여 쿼리를 날리기 때문에 비효율적으로 늘어나는 쿼리 요청을 방지할 수 있습니다.

## select_related
참조하는 대상이 중간테이블이 아닐 시, 쿼리문에서 Join을 이용해서 data를 불러드립니다.

바꿔 말하면, 두 테이블간 join을 할 수 있는 구조에서 사용 가능합니다.

또한 join을 해서 불러드리기 때문에, 쿼리문은 1번 실행됩니다.

마지막으로 중간테이블을 이용해서 관계를 형성하는, `many-to-many 모델`에서는 사용할 수 없습니다.

```python
In [177]: dogs2 = Dog.objects.select_related('owner').filter(id__gt=65540)

In [178]: for dog in dogs2:
     ...:    dog.owner.name
     ...:
```

```bash
(0.073) SELECT `dogs`.`id`, `dogs`.`owner_id`, `dogs`.`name`, `dogs`.`age`, `owners`.`id`, `owners`.`name`, `owners`.`email`, `owners`.`age` FROM `dogs` INNER JOIN `owners` ON (`dogs`.`owner_id` = `owners`.`id`) WHERE `dogs`.`id` > 65540; args=(65540,)
```

## prefetch_related
2개의 테이블을 각각 불러드려서, django내에서 합칩니다.

select_related을 사용할 수 없는 many-to-many모델에서 사용합니다.

> 💡 1:1, 1:N 관계에서도 사용가능합니다.

```python
dogs2 = Dog.objects.prefetch_related('owner').filter(id__gt=65540)
for dog in dogs2:
    dog.owner.name
```
```bash
(0.001) SELECT `dogs`.`id`, `dogs`.`owner_id`, `dogs`.`name`, `dogs`.`age` FROM `dogs` WHERE `dogs`.`id` > 65540; args=(65540,)

(0.000) SELECT `owners`.`id`, `owners`.`name`, `owners`.`email`, `owners`.`age` FROM `owners` WHERE `owners`.`id` IN (17); args=(17,)
```

# 헷갈린 점
다른테이블을 참조안하고 조회 할 때도, N+1문제가 발생한다고 생각해습니다.

다음 예시는 자신의 테이블의 정보만 조회합니다.

그럴시에는 쿼리문이 1번만 실행됩니다.
```python
dogs2 = Dog.objects.filter(id__gt=65540)

In [184]: for dog in dogs2:
     ...:     print("Excute querry")
     ...:     dog.name
     ...:
```

```bash
(0.001) SELECT `dogs`.`id`, `dogs`.`owner_id`, `dogs`.`name`, `dogs`.`age` FROM `dogs` WHERE `dogs`.`id` > 65540; args=(65540,)
Excute querry
Excute querry
```
