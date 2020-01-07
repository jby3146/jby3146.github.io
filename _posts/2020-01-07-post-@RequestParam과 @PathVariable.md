---
layout: post
title: "@RequestParam과 @PathVariable"
categories:
  - Post
tags:
  - SPRING
last_modified_at: 
---

## @RequestParam과 @PathVariable ##

`
    @RequestParam
`

주소에 변수명과 값이 들어있는 경우
http://192.168.0.1:8080/?id=3

`
    @PathVariable
`

URL 경로에 변수가 들어있는 경우
http://192.168.0.1:8080/3



**@PathVariable 예시 CODE**

---

```java

    @ApiOperation(value = "게시판 글 수정", notes = "게시판의 글을 수정한다.")
    @PutMapping(value = "/post/{postId}")
    public SingleResult<Post> post(@PathVariable long postId, @Valid @ModelAttribute ParamsPost post) {
       //생략
    }


```


```ts
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
```