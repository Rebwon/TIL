## Chap1(웹 프로그래밍 기초) 정리

Browser의 동작
- 웹을 통해서 전달되는 데이터는 어딘가에서 해석돼야 합니다. 
서버에서 전송한 데이터(HTML과 같은)가 클라이언트에 도착해야 할 곳은 'Browser'입니다.
Browser에는 *데이터를 해석*해주는 *파서*와 *데이터를 화면에 표현*해주는 *렌더링엔진*이 포함되어 있습니다.

[브라우저 동작 정리 링크](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/#Introduction)

렌더링 엔진
- 브라우저마다 다른 렌더링 엔진을 사용합니다. Internet Explorer는 Trident를, Firefox는 Gecko를, Safari는 WebKit을 사용합니다. 
Chrome 및 Opera (버전 15부터)는 WebKit의 포크 인 Blink를 사용합니다.
- WebKit은 Linux 플랫폼 용 엔진으로 시작하여 Mac과 Windows를 지원하도록 Apple에서 수정 한 오픈 소스 렌더링 엔진입니다.

렌더링 엔진은 네트워크 계층에서 요청된 문서의 내용을 가져온다. 보통 8kb 청크 파일로 이루어진다.

Webkit 메인 플로우
![webkit](images/1.PNG) 

생각해보기
1. 우리가 흔히 브라우저 탐색을 할 때 스크롤을 하거나, 어떤 것을 클릭하면서 화면의 위치를 바꿀 때, 브라우저는 어떻게 다시 화면을 그릴까요?
위에서 표현된 그림처럼 다시 렌더링 되지 않을까요? 

답변: 브라우저를 스크롤링하거나 링크를 클릭하는 등의 동작을 진행할 때, 브라우저는 Web Page의 구성 요소인
Render Tree를 저장하고 있다가 정보만 가져와서 painting 작업을 진행한다. 따라서 다시 렌더링되지 않는다.