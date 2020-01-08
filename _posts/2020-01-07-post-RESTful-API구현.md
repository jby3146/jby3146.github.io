---
layout: post
title: "REST API 구현"
categories:
  - Post
tags:
  - SPRING
  - Angular
last_modified_at: 
---

`
    REST API 란 ?
`

 Representational State Transfer라는 용어의 약자

자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.
```
GET : GET를 통해 해당 리소스를 조회합니다. 리소스를 조회하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.
POST : POST를 통해 해당 URI를 요청하면 리소스를 생성합니다.
PUT : PUT를 통해 해당 리소스를 수정합니다.
DELETE : DELETE를 통해 리소스를 삭제합니다.
```

**- SPRING**

---

```java

  
    @ApiOperation(value = "게시판 정보 조회", notes = "게시판 정보를 조회한다.")
    @GetMapping(value = "/{boardName}")
    public SingleResult<Board> boardInfo(@PathVariable String boardName) {
        //생략
    }

    @ApiOperation(value = "게시판 글 리스트", notes = "게시판 게시글 리스트를 조회한다.")
    @GetMapping(value = "/{boardName}/posts")
    public ListResult<Post> posts(@PathVariable String boardName) {
        //생략
    }

    @ApiImplicitParams({
            @ApiImplicitParam(name = "X-AUTH-TOKEN", value = "로그인 성공 후 access_token", required = true, dataType = "String", paramType = "header")
    })
    @ApiOperation(value = "게시판 글 작성", notes = "게시판에 글을 작성한다.")
    @PostMapping(value = "/{boardName}")
    public SingleResult<Post> post(@PathVariable String boardName, @Valid @ModelAttribute ParamsPost post) {
        //생략
    }

    @ApiOperation(value = "게시판 글 상세", notes = "게시판 글 상세정보를 조회한다.")
    @GetMapping(value = "/post/{postId}")
    public SingleResult<Post> post(@PathVariable long postId) {
        //생략
    }

    @ApiImplicitParams({
            @ApiImplicitParam(name = "X-AUTH-TOKEN", value = "로그인 성공 후 access_token", required = true, dataType = "String", paramType = "header")
    })
    @ApiOperation(value = "게시판 글 수정", notes = "게시판의 글을 수정한다.")
    @PutMapping(value = "/post/{postId}")
    public SingleResult<Post> post(@PathVariable long postId, @Valid @ModelAttribute ParamsPost post) {
       //생략
    }

    @ApiImplicitParams({
            @ApiImplicitParam(name = "X-AUTH-TOKEN", value = "로그인 성공 후 access_token", required = true, dataType = "String", paramType = "header")
    })
    @ApiOperation(value = "게시판 글 삭제", notes = "게시판의 글을 삭제한다.")
    @DeleteMapping(value = "/post/{postId}")
    public CommonResult deletePost(@PathVariable long postId) {
        //생략
    }

```

**- Angular2**

---

```ts
    getPosts(boardName: string): Promise<Post[]> {
    const getPostsUrl = this.getBoardUrl + '/' + boardName + '/posts';
    return this.http.get<ApiReponseList>(getPostsUrl)
      .toPromise()
      .then(this.apiValidationService.validateResponse)
      .then(response => {
        return response.list as Post[];
      })
      // 예외 생략
      });
  }
  addPost(boardName: string, author: string, title: string, content: string, msrl : number): Promise<Post> {
    const postUrl = this.getBoardUrl+'/'+boardName;
    const params = new FormData();
    params.append('author', author);
    params.append('title', title);
    params.append('content', content);
    params.append('msrl' , msrl.toString());
    return this.http.post<ApiReponseSingle>(postUrl, params)
      .toPromise()
      .then(this.apiValidationService.validateResponse)
      .then(response => {
        return response.data as Post;
      })
      //예외 생략
  }
  viewPost(postId: number): Promise<Post> {
    const getPostUrl = this.getBoardUrl + '/post/' + postId;
    return this.http.get<ApiReponseSingle>(getPostUrl)
      .toPromise()
      .then(this.apiValidationService.validateResponse)
      .then(response => {
        return response.data as Post;
      })
      //예외 생략
  }
  modifyPost(post: Post): Promise<Post> {
    const postUrl = this.getBoardUrl + '/post/' + post.postId;
    const params = new FormData();
    params.append('author', post.author);
    params.append('title', post.title);
    params.append('content', post.content);
    return this.http.put<ApiReponseSingle>(postUrl, params)
      .toPromise()
      .then(this.apiValidationService.validateResponse)
      .then(response => {
        return response.data as Post;
      })
      //예외 생략
  }
  deletePost(postId: number): Promise<boolean> {
    const deletePostUrl = this.getBoardUrl + '/post/' + postId;
    return this.http.delete<ApiReponseSingle>(deletePostUrl)
      .toPromise()
      .then(this.apiValidationService.validateResponse)
      .then(response => {
        return true;
      })
      //예외 생략
  }

```

**- Reference**

---
* https://livedata.tistory.com/3
* https://disqus.com/by/disqus_Iw723GnZPj/