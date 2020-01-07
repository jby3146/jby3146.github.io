---
layout: post
title: "Swagger를 활용한 RESTful-API 문서 만들기"
categories:
  - Post
tags:
  - SPRING
last_modified_at: 
---


`
    Swagger 란 ?
`
```
Swagger의 주된 목적은 RESTful API를 문서화시키고 관리하는 것이다.
Swagger를 사용하면 자동화할 수 있다.

내가 생각하는 가장 큰 강점은 보기 쉬운 UI와 테스트 환경 제공인거 같다.

실무에서 사용해 보지는 않았지만 유지보수할 때도 쉽게 확인 가능하다는 것도 장점인거 같다.

```

![1](/images/swagger1.PNG) 

swagger를 적용시킨 화면인데 각각 이름을 지정해 줄 수 있어 훨씬 보기 편해지게 된다. 또한 밑에 코드에 각각 기능에 대한 명시를 하게되면서 훨씬 다른사람들도 알아보기 쉽게 되었다.

**예시 CODE**

---

```java
    @Api(tags = {"3. Board"})
    //@Api(tags = {"3. Board"}) 로 API 소제목 입력
    @RequiredArgsConstructor
    @RestController
    @RequestMapping(value = "/v1/board")
    public class BoardController { }

    
    @ApiOperation(value = "게시판 정보 조회", notes = "게시판 정보를 조회한다.")
    //각 기능의 제목과 note로 상세정보를 기록가능하다
    @GetMapping(value = "/{boardName}")
    public SingleResult<Board> boardInfo(@PathVariable String boardName) {
        return responseService.getSingleResult(boardService.findBoard(boardName));
    }

    @ApiOperation(value = "게시판 글 리스트", notes = "게시판 게시글 리스트를 조회한다.")
    @GetMapping(value = "/{boardName}/posts")
    public ListResult<Post> posts(@PathVariable String boardName) {
        return responseService.getListResult(boardService.findPosts(boardName));
    }

    @ApiImplicitParams({
            @ApiImplicitParam(name = "X-AUTH-TOKEN", value = "로그인 성공 후 access_token", required = true, dataType = "String", paramType = "header")
    })

```

![1](/images/swagger2.PNG) 
위 사진을 보면 변수명까지 설정이 가능해 만든 사람도 더 쉽게 확인 가능하지만 같이 협업하는 사람들에게 명확한 정보를 제공해 줄 수 있다는 점에서 좋은 것 같다.

`
happydaddy님의 강의를 따라하면서 평소 지나쳤던 부분과 새로운 내용들을 많이 접해보면서 많은 도움이 된거 같다.
`

**- Reference**

---
* https://sanghaklee.tistory.com/50 
* https://disqus.com/by/disqus_Iw723GnZPj/