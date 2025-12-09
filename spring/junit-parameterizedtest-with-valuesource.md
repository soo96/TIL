### 1. @ParameterizedTest

여러 개의 변수를 테스트해야 할 때 직접 입력하기보다 인자값으로 설정하여 간단하게 테스트할 수 있다.</br>
인자값을 이용하여 테스트할 때 사용한다.</br>
`@ParameterizedTest`는 단독으로는 사용할 수 없으며 인자값을 넣어주는 다른 어노테이션과 같이 사용할 수 있다.

### 2. @ValueSource

@ValueSource는 테스트하려는 인자값을 배열로 받아 함수 인자값으로 전달할 수 있다.</br>
테스트하려는 인자값이 string인 문자열이라면 strings로 설정하면 된다.

**🔍 예시 코드**

```java
@ParameterizedTest
@ValueSource(strings = {
        "image.jpg",
        "menu_01.jpeg",
        "menu_02.png",
        "menu_02.PNG",
        "profile.webp",
        "profile.wEbP",
        "profile.JPeG",
        "a/b/c/d/e.png"
})
@DisplayName("유효한 이미지 파일 이름은 검증을 통과해야 한다.")
public void testValidFilenames(String fileName) throws Exception {
    //given
    ImageFileType mockFileType = ImageFileType.JPEG;
    PresignedUrlRequest presignedUrlRequest = new PresignedUrlRequest(fileName, mockFileType);

    //when
    Set<ConstraintViolation<PresignedUrlRequest>> violations = validator.validate(presignedUrlRequest);

    //then
    assertThat(violations).isEmpty();
}
```
