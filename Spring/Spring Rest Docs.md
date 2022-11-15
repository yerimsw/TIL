### 1. Spring Rest Docs란?

> REST API 문서를 자동으로 생성해주는 Spring 하위 프로젝트

- 애플리케이션 코드(Controller, DTO 등)에 API 문서 생성을 위한 애너테이션을 추가하지 않고 테스트 클래스에 추가한다.
- Test Case의 API 문서 정보와 Controller의 실제 API 스펙 정보가 일치하지 않으면 Test failed 한다.
- 즉 Test를 passed로 만들면 API 스펙 정보와 동일한 API 문서가 생성된다.
- 수기로 작성했을 때의 불편함을 개선하고, API 스펙 정보불일치 문제를 해결할 수 있다.
<br>

### 2. 문서화 테스트 코드

> Spring Rest Docs를 생성할 때에는 mockMvc.perform()의 매개변수로 RestDocumentationRequestBuilders 클래스의 http 메서드를 작성해야한다.

``` java
// when
ResultActions actions = mockMvc.perform(patch("uri")
            .accept(MediaType.APPLICATION_JSON)
            .contentType(MediaType.APPLICATION_JSON)
            .content(jsonContent);
// then
actions
      .andExpect(status().isCreated())
      .andDo(document( // (0)
               "patch-member", // (1)
               preprocessRequest(prettyPrint()),
               preprocessResponse(prettyPrint()), // (2)
               pathParameters(parameterWithName("member-id").description("회원 식별자")), // (3)
               requestFields(
                  List.of(
                      fieldWithPath("필드 명").type(JsonFieldType.NUMBER).description("설명"),
                      fieldWithPath("필드 명").type(JsonFieldType.NUMBER).description("설명").ignored()
                  )
               ), // (4)
               responseFields(
                  List.of(
                      fieldWithPath("필드 명").type(JsonFeildType.OBJECT).description("설명"),
                      fieldWithPath("필드 명").type(JsonFeildType.OBJECT).description("설명")
                  )
               ) // (4)
        ));
```
0. `andDo(document())` 메서드로 API 스펙 정보를 추가해 문서화 작업을 수행한다.
1. 문서화된 API 호출의 식별자다.
2. API 문서의 생성 전 전처리를 수행한다. JSON 데이터 출력 형식을 예쁘게 감싼다.
3. `pathParameters()`는 uri 경로 상의 pathVariable 의 정보를 전달한다.
   - `requestParameters()` 으로는 쿼리 변수 정보를 전달할 수 있다.
   - 전달하는 변수의 개수가 두개 이상일 경우 `List.of()` 리스트로 묶어 매개변수로 넘겨야 한다.
4. 요청, 응답 데이터에 있는 필드 변수 정보를 전달한다.
   - `.ignored()` : API 스펙에서 정보를 무시한다.
   - `.optional()` : API 스펙에서 필수정보로 분류하지 않고 선택적으로 정보를 전달할 수 있다.
<br>

### 3. Asciidoc

- Spring Rest Docs으로 문서 스니핏을 생성하면, 스니핏을 모아 템플릿 문서를 생성할 수 있다.
- Asciidoctor을 이용해 html 템플릿 문서를 생성할 수 있다.

``` java
== 제목
.curl-request // (1)
include::{snippets}/post-member/http-request.adoc[] // (2)
```

1. `.` 스니핏 제목
2. `include` : Asciidoctor에서 스니핏을 템플릿 문서에 포함시킬 때 사용하는 macro이다.
   - {snippets} 는 스니핏이 생성된 디폴트 경로를 나타낸다.
   - 이 값은 build.gradle 파일의 `snippetDir` 변수를 참조한다.
