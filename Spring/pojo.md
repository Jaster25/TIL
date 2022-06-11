# POJO란?

**Plain Old Java Object**, 오래된 방식의 간단한 자바 오브젝트를 의미한다.
프레임워크들을 사용하게 되면서 해당 프레임워크에 종속되어 점점 무거워지는 객체에 반발해서 사용되게 된 용어이다.

<br>

## POJO의 특징

- 특정 규약에 종속되지 않는다.
- 특정 환경에 종속되지 않는다.
- 객체지향적

<br>

아래 객체는 기본적인 자바 기능인 **Getter, Setter** 메서드만 갖으며,
**특정 기술에 종속되지 않는 순수 자바 객체**이기 때문에 POJO라고 할 수 있다.

```java
public class CommentDto {

  private Long commentId;
  private String content;

  public Long getCommentId() {
    return commentId;
  }

  public void setCommentId(Long commentId) {
    this.commentId = commentId;
  }

  public String getContent() {
    return content;
  }

  public void setContent(String content) {
    this.content = content;
  }
}
```

<br>

## 참고

- https://doing7.tistory.com/81

- https://yoo11052.tistory.com/133
