## JavaScript

- 자바스크립트의 값, 자바스크립트의 저장 방법, 가비지 컬렉션
- 원시 값과 가변값
- Shallow Copy와 Deep Copy가 무엇인지, 자바스크립트에서 Shallow Copy와 Deep Copy는 어떤 이유로 발생 되는지
- Call By Value, Call By Reference, Call By Sharing
- 실행 컨텍스트
- var, let, const 차이 왜 나왔고 어떤 점을 보완하는지 어떤 점이 안좋은지
  - var < let < const 순서로 나왔다.
  - var, let은 변수이고 const는 상수로 정의한다.
  - var는 함수 스코프이고 let은 지역적 스코프이다. 따라서 var를 사용하면 의도치 않은 동작들이 발생한다.
  - 참고로 읽을 자료: https://ko.javascript.info/var
- 호이스팅
- 클로저
- 스코프 형성
- 헷갈리는 this 의도하는 대로 this를 컨트롤 할 수 있도록 this의 컨텍스트가 결정되는 상황 알기
- 에러핸들링
- 프로토타입과 자바스크립트에서의 클래스 동작방식, 내부가 어떤식으로 돌아가는지 알기
- 클라이언트-서버간 인터렉션 (초점) 등장 이유와 어떻게 생겨 먹었는지 알기
- XMLHttpRequest
- Fetch API
- Fetch API와 XMLHttpRequest의 차이
- Ajax 라는 개념은 무엇인지
- Axios 라이브러리는 어떻게 이루어져 있을까
- 이벤트 버블링, 캡쳐링 같은 것들 개념과 제어방법
- 비동기 처리
- Callback
- 콜백헬이란 무엇인가?
- Promise 개념과 동작 원리 이전에는 라이브러리로 만들어서 썼는데, 어떻게 구현되어 있는지
- pending, fulfilled 같은 상태를 어떻게 구현 했을까?
- Async/await 개념과 동작 원리, 본체가 어떻게 생겨먹었는지 어떻게 구현되어 있는지, 제너레이터
- 함수 제어
- 코루틴과 제너레이터 위에 async/await 구현체랑 같이 보면 좋음

함수형 프로그래밍
- 함수 확장의 개념에 입각해서 생각하기
- 모나드와 커링
- 순수함수
- 불변성 underscore.js 내부 구현체 같이 보면 좋음

모듈 시스템
- ES6 Module과 Common.js
- Require와 import의 차이 왜 import가 나왔고, Node.js에서 import를 도입하려고 하는지 관련 키워드로 웹팩과 트리쉐이킹도 도움이 됐음
- 객체지향
- 상태와 메시지 객체끼리 어떻게 메시지를 주고 받을지

좋은 참고 블로그
- https://2ality.com/index.html
- https://rinae.dev/

좋았던 글
- [번역] 초보 프론트엔드 개발자들을 위한 Pub-Sub(Publish-Subscribe) 패턴을 알아보기
- https://jeonghwan-kim.github.io/ (개인적으로 좋아해서 블로그랑 강의 챙겨 봄)
- https://www.mattgreer.org/articles/promises-in-wicked-detail/ (프로미스 구현)
- 코드스피츠 및 비사이드 소프트 글과 동영상 (강추)

좋은 책
- 코어 자바스크립트
- JavaScript: The Definitive Guide
- https://www.amazon.com/JavaScript-Definitive-Most-Used-Programming-Language/dp/1491952024/ref=sr_1_1?dchild=1&keywords=javascript+definitive+guide&qid=1591928061&sr=8-1
- 자바스크립트 디자인 패턴 
- Node.js 교과서 
- 실전 리액트 프로그래밍
