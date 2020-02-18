## Spring Request Response 애노테이션 정리.

1. @RequestBody 
   - 이 애노테이션은 클라이언트에서 요청을 하는 부분에 써야한다.(예를 들자면 Postman을 통한 JSON BODY 전송 등)
   - 요청 본문은 HttpMessageConverter가 처리한다.
   - 요청 내용의 종류에 따른 인수 선택 사항은 자동으로 매칭된다.
   - 유효성 검사는 @Valid를 통해 적용가능하다. 
   - Spring에서는 @RequestBody를 통해 JSON으로 데이터가 넘어올 경우 Jackson2HttpMessageConverter의 ObjectMapper를 통해서 Setter가 없이도
   값이 할당되어 데이터베이스로 들어간다.
   

   


