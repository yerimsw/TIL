## 목차

1. [Mocking 이란](#1-mocking-이란)
2. [Mockito 프레임워크](#2-mockito-framework)
3. [Mockito 적용](#3-mockito-적용)


### 1. Mocking 이란

- 테스트 영역에서 Mock이란 가짜 객체를 의미한다.
- 단위 테스트, 슬라이스 테스트 등에 Mock 객체를 적용하는 것을 Mocking이라고 한다.
- 테스트 대상에 따라 Mocking을 적용해서 연동된 타 계층을 적절히 분리시켜 범위를 정할 수 있다.
<br>

### 2. Mockito Framework

- Mock 객체를 생성하여 진짜 처럼 동작하도록 하는 Mocking 라이브러리다.
- Spring Framework에서 지원하는 Mocking 라이브러리가 바로 Mockito이다.
- Mockito 의 Mocking 기능을 이용해 테스트 대상으로부터 다른 비 테스트 영역들을 분리시킬 수 있다.
<br>

### 3. Mockito 적용

> 컨트롤러 슬라이스 테스팅에서의 테스트 대상을 핸들러 메서드로 제한했다.

- 컨트롤러 슬라이스 테스팅

``` java
// patch HTTP 핸들러 메서드
@PatchMapping("/{member-id}")
public ResponseEntity patchMember(
  @PathVariable("member-id") @Positive long memberId, @Valid @RequestBody MemberDto.Patch requestBody) {
     
     requestBody.setMemberId(memberId); // MemberPatchDto
     
     Member member = memberService.updateMember(mapper.memberPatchToMember(requestBody)); // (1)
     return new ResponseEntity<>(new SingleResponseDto<>(mapper.memberToMemberResponse(member)), HttpStatus.OK); // (2)
}
```
   1. Service의 `updateMember()` 메서드를 통해 Repository의 `save()` 메서드를 호출하여 멤버 객체를 저장한다.<br>
   2. 저장한 멤버 객체를 Mapper를 통해 ResponseDto 응답 객체로 변환해 응답으로 돌려준다.
<br>

``` java
// patch HTTP 핸들러 메서드 테스트
@Test
void patchMemberTest() throws Exception {
  // given - Stub 객체, JSON 데이터 생성
  ...
  given(mapper.memberPatchToMember(any(MemberDto.Patch.class))).willReturn(new Member());
  given(memberService.updateMember(any(Member.class))).willReturn(new Member());
  given(mapper.memberToMemberResponse(any(Member.class))).willReturn(response); // (1)
  
  String content = gson.toJson(patchDto); // (2)
  ...
  
  // when - patch HTTP request 전송
  ResultActions actions = mockMvc.perform(patch(uri) // (3)
                   .accept(MediaType.APPLICATION_JSON)
                   .contentType(MediaType.APPLICATION_JSON) // (4)
                   .content(content)); // (5)
                   
  // then - 검증
  MvcResult result = actions.andExpect(status().isOk())
                            .andReturn();
}
```
   1. given(patch 메서드에서 리턴 객체가 대체 될 메서드).willReturn(Stub 객체); 으로 Stub 객체를 반환한다.
   2. gson 라이브러리의 `toJson()` 메서드로 객체를 JSON 데이터로 변환한다.
   3. mockMvc.perform(patch(uri)) patch HTTP 메서드 uri 요청을 전송한다.
   4. `accept()` 클라이언트 응답으로 JSON 데이터를 받고, `contentType()` 서버에서 사용할 데이터로 JSON을 받는다.
   5. `content()` 메서드로 2번에서 전환한 JSON 데이터를 넘겨준다.
