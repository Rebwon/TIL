## Spring Request Response 애노테이션 정리.

1. @RequestBody 
   - 클라이언트에서 요청을 하는 부분에 써야한다.(예를 들자면 Postman을 통한 JSON BODY 전송 등)
   - 요청 본문은 HttpMessageConverter가 처리한다.
   - 요청 내용의 종류에 따른 인수 선택 사항은 자동으로 매칭된다.
   - 유효성 검사는 @Valid를 통해 적용가능하다. 
   - Spring에서는 @RequestBody를 통해 JSON으로 데이터가 넘어올 경우 Jackson2HttpMessageConverter의 ObjectMapper를 통해서 Setter가 없이도
   값이 할당되어 데이터베이스로 들어간다.

2. @RequestParam
   - 클라이언트에서 요청을 하는 부분에 써야한다.(예를 들자면 URL상에서 GET 요청으로 보내는 Query String구조이다.)
   - 하나 이상의 타입에 적용이 가능하다. (스프링에서 지원가능한 타입 전부 가능)
   - 주의할 점은 지정한 키값이 존재하지 않다면 BadRequest에러를 발생시킨다.
   - required, DefaultValue 설정이 가능하다.
   - 파라미터가 많아질 경우, Map이나 DTO로 간단하게 받아올 수 있다.
   
3. @PathVariable
   - 요청 URL 경로에 변수를 넣어주는 것이다.(예를 들면 특정 상품의 id값을 통해 GET요청을 보내서 상품 정보를 확인할 경우)
   - {변수명}으로 넣어주는데, 이때 인자로 설정한 변수명과 동일해야 한다.
   
4. @RequestHeader
   - 클라이언트 요청 헤더 값을 컨트롤러 메서드의 파라미터로 전달한다.
   - 헤더가 존재하지 않으면 에러가 발생하고, required 속성으로 필수 여부를 설정할 수 있다.
   - defaultValue를 통해 기본 값 설정도 가능하다.
   
5. @ResponseStatus
   - 서버에서 응답을 보내줄 때, 어떤 응답을 보내줘야하는지 설정이 가능하다.
   - HTTP 상태 코드로 응답을 보내줘야 한다.
   - HttpStatus를 인자로 받는다.
   
6. @RequestPart
   - multipart/form-data 요청의 일부를 메서드의 인수와 연관시키는데 사용한다.
   - JSON/XML 같은 복잡한 요청을 처리하는데 사용한다.