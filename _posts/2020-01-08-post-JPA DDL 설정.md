---
layout: post
title: "JPA DDL 설정"
categories:
  - Post
tags:
  - SPRING
  - JPA
last_modified_at: 
---


`
    JPA 란 ?
`
```
Java Persistence API의 약자로 현재 자바 진영의 ORM 기술 표준으로, 인터페이스의 모음이다

가장 대표적이고 실무에서도 많이 사용하는 Hibernate이다.

장고에서만 ORM이 되는 줄 알았는데 자바에서도 됐더니 처음 들었을 때 많이 충격을 받았다.

교육에서는 Mybatis를 사용해서만 구현했었는데 jpa를 공부하면서 그 편리함에 큰 충격을 받았다.

```
---
jpa 설정의 기본인 DDL 설정을 알아보도록 하겠습니다.


**spring.jpa.generate-ddl 속성**

* true로 설정 시, Entity 어노테이션(@Entity)이 명시된 클래스를 찾아서 ddl을 생성하고 실행


**spring.jpa.hiberante.ddl-auto 속성**


* 옵션
  * none: 자동 생성하지 않음
  * create: 항상 다시 생성한다 다시 생성할 때 기존 삭제
  * create-drop: 시작 시 생성 후 종료 할 때는 제거
  * update: 시작 시 Entity 클래스와 DB 스키마 구조를 비교해서 DB쪽에 생성되지 않은 테이블, 컬럼 추가 (제거는 하지 않음)
  * validate: 시작 시 Entity 클래스와 DB 스키마 구조를 비교해서 같은지만 확인 (다르면 예외 발생)


---

**예시 CODE**

```java
@Entity
public class Post extends CommonDateEntity { }


@Entity // jpa entity임을 알립니다.
@Table(name = "user") // 'user' 테이블과 매핑됨을 명시
public class User implements UserDetails { }
```

예시처럼 위에 Entity로 선언 된 클래스를 탐색 후 DDL 설정에 맞춰 다시 생성하거나 업데이트 해준다.

( 코드를 테스트 할 때 DB를 다시 생성한 후 테이블을 생성해주고 테스트를 할 때도 많았고
 DB 업데이트 될때마다 다시 Mapper들을 바꿔주던것을 생각하면 정말 엄청난 기능이라고 생각한다. )

**- Reference**

---
* https://gmlwjd9405.github.io/2019/08/04/what-is-jpa.html)