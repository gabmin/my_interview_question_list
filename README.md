# 내가 보려고 만든 프론트엔드 면접 질문 리스트

## 기술

### 브라우저의 렌더링 원리에 대해 설명해주세요.

브라우저 렌더링은 브라우저(크롬, 사파리 등)가 HTML, CSS, JavaScript로 작성된 텍스트 문서에 대해 파싱(해석)하고 이를 화면에 그리는 일련의 과정을 말합니다. 중요 렌더링 경로(Critical Rendering Path, CRP)라고도 하며 이를 최적화 하는 것은 렌더링 성능을 향상시킵니다.

1. HTML를 파싱 후, DOM트리를 구축합니다.
2. CSS를 파싱 후, CSSOM트리를 구축합니다.
3. Javascript를 실행합니다. 자바스크립트 파일을 파싱하여 AST(추상적 구문 트리)를 생성하고 바이트 코드로 변환한다.
   이때 자바스크립트는 DOM API에 의해 DOM과 CSSOM을 변경한다.
4. DOM과 CSSOM을 조합하여 렌더트리를 구축합니다.
   주의! display: none 속성과 같이 화면에서 보이지도 않고 공간을 차지하지 않는 것은 렌더트리로 구축되지 않습니다.
5. 뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기를 계산합니다. (Layout 단계)
6. 계산한 위치/크기를 기반으로 화면에 그립니다. (Paint 단계)

<br/>

### Reflow와 Repaint가 무엇이며 실행되는 시점은 언제인지 설명해주세요.

- Reflow
  요소의 위치와 크기를 다시 계산하는 것을 의미합니다. DOM에 변경이 발생하면 요소의 위치와 크기를 다시 계산하고 렌더트리를 재생성합니다.
  특정 요소에 리플로우가 발생하면 부모, 자식 요소에도 영향으로 주어 비용이 큰 작업입니다.

DOM 노드를 추가, 제거, 업데이트 하는 경우, 브라우저 사이즈가 변경되는 경우, 텍스트 내용이나 이미지 등 컨텐츠가 동적으로 변경되는 경우 발생한다.

`display:none` 같은 경우는 노드에서 아예 제거되기 때문에 reflow가 발생한다.

- RePaint
  RePaint는 요소의 스타일을 변경하지만 레이아웃에는 영향을 미치지 않는 경우에 발생합니다.

`visibility:hidden`은 보이지만 않게 하여 repaint가 발생한다.

<br/>

### 이벤트 루프 설명해주세요.

자바스크립트는 싱글 스레드 기반의 언어이기에 동시에 한 가지 일만 처리할 수 있습니다.
자바스크립트 엔진은 Memory Heap(메모리 할당)과 Call Stack(코드가 실행될 때 마다 스텍이 쌓이는 곳)으로 구성되어 있다.
비동기 작업이 발생하면 Web APIs에 의해서 콜백함수를 Task Queue로 이동시키며 Event Loop에 의해서 Call Stack과 Task Queue의 상태를 체크하여, Call Stack이 빈 상태가 되면, Task Queue의 첫번째 콜백을 Call Stack으로 밀어넣어 줍니다.

setTimeout과 같은 함수를 태스크 큐(매크로 테스크 큐)로 이동하고 Promise 후속처리 메서드같은 경우는 마이크로 테스크 큐로 이동된다.
이때 마이크로 테스크 큐가 더 높은 우선순위를 갖고 먼저 실행된다.

<br/>

### 호이스팅에 대해 설명해주세요.

호이스팅이란 변수 및 함수 선언문이 스코프 내의 최상단으로 끌어올려지는 것처럼 동작하는 현상을 말합니다.
let은 선언 단계와 초기화 단계로 분리되어 실행되고 런타임 이전에 자바스크립트 엔진에 의해서 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도딜했을 때 실행된다. 이떄 선언 단계와 초기화 단계 사이에 변수를 참조할 수 없는 구간을 일시적 사각지대(TDZ)라고 합니다.

- var : 중복 선언 가능, 함수 레밸 스코프
- let : 중복 불가능, 블록 레밸 스코프
- const : 선언과 초기화가 동시에 일어남

<br/>

### 클로저(Closure)란?

함수가 선언될 때의 외부 변수들을 기억하고, 나중에 그 함수가 실행될 때도 그 기억을 유지하는 기능이다. 클로저는 생성 될 때 상위 스코프 렉시컬 환경을 기억하고 있어 외부 함수의 실행이 끝나서 실행 컨텍스트에서 소멸된 이후에도 외부함수의 렉시컬 환경을 내부함수가 참조하고 있기 때문에 내부 함수가 외부 함수 변수에 접근이 가능합니다.

내부 함수가 참조하지 않은 외부함수의 변수는 가비지 컬렉터에 등록되고, 참조하고 있다면 등록되지 않습니다.

전역 변수를 줄일 수 있고 재사용률을 높일 수 있다는 장점이 있지만, 메모리에 계속 남아있어 직접 해재해야한다는 단점이 있습니다.

- 렉시컬 스코프(정적 스코프): 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정하는 방식을 렉시컬 스코프라고 한다.

<br/>

### 스코프

스코프는 변수에 접근할 수 있는 범위를 말합니다. 자바스크립트에는 전역 스코프(Global Scope)와 지역 스코프(Local Scope)가 있습니다.
또한 자바스크립트는 렉시컬 스코프를 따릅니다.

<br/>

### 실행 컨택스트

코드가 실행되기 위한 모든 환경 정보를 담고 관리하는 공간입니다. 실행 컨텍스트는 스택 구조로 쌓이며, 변수, 함수, this 등을 기억하고 실행 순서를 관리합니다.

1. 코드가 실행되기 전에 실행 컨텍스트가 만들어져서 변수/함수 선언을 미리 등록합니다. (호이스팅)
2. 함수 호출 시마다 새로운 실행 컨텍스트가 생성되어 스택에 쌓입니다.
3. 실행이 끝나면 해당 컨텍스트는 스택에서 제거됩니다.
4. 변수나 함수는 현재 컨텍스트 → 외부 컨텍스트 순서로 탐색합니다. (스코프 체인)

### 자바스크립트 this란?

this는 현재 실행 중인 문맥(Context)을 참조하는 키워드입니다.
어떤 방식으로 함수를 호출했느냐에 따라 this가 가리키는 값이 달라집니다.

call, apply, bind를 사용하면 this를 직접 지정할 수 있습니다.

1. 일반 함수 호출 = 전역 객체
2. 메서드 호출 = 메서드를 호출한 객체
3. 생성자 함수 = 새로 생성된 객채
4. 명시적 바인딩(call, apply, bind) = 원하는 객체 강제 지정
5. 화살표 함수 = this를 바인딩 하지 않아 undefined

call, apply, bind의 차이점은 함수를 즉시 호출하는지 인자를 어떻게 넘기는지의 차이점이 있습니다.
call → this 지정하고 바로 실행 (인자 나열)
apply → this 지정하고 바로 실행 (인자 배열)
bind → this 지정하고 새 함수 반환 (나중에 실행)

<br/>

### 브라우저 저장소에 대해서 설명해 보세요.

Cookie는 서버와 함께 전송되고, LocalStorage는 브라우저에 영구 저장, SessionStorage는 탭이 닫히면 삭제됩니다.

1. Cookie (4KB)

- 용도: 로그인 인증, 세션 유지
- 특징: 요청할 때 자동으로 서버에 전송됨
- 단점: 크기 제한(작음), 네트워크 트래픽 증가 가능

2. LocalStorage (5MB)

- 용도: 장기 저장 (예: 테마, 장바구니 상태)
- 특징: 영구 저장, 탭을 닫아도 유지됨
- 단점: 동기적 처리 → 데이터가 많으면 렌더링 성능 영향 가능

3. SessionStorage (5MB)

- 용도: 임시 저장 (예: 로그인 중 입력 데이터, 페이지 이동 간 데이터 유지)
- 특징: 창/탭을 닫으면 삭제, 탭별로 분리됨
- 단점: 탭을 새로 열면 데이터 초기화됨

<br/>

### 비동기 처리방식에 대해 설명해주세요.

자바스크립트는 싱글 스레드로 동작하지만 이벤트 루프를 통해서 비동기 작업을 관리합니다.
비동기 처리는 콜백 -> 프로미스 -> async/await 순으로 발전해왔습니다.
콜백헬 문제로 프로미스가 등장했으며 await을 통해 비동기 코드를 동기 코드처럼 작성할 수 있다.
에러처리는 try catch 문으로 처리할 수 있습니다.

<br/>

### GET과 POST의 차이, PUT과 PATCH의 차이점은?

GET은 서버로부터 데이터를 받아오기 위한 메서드이고, POST는 서버에 데이터를 전송하기 위한 메서드입니다. PUT은 리소스의 모든 것을 업데이트하지만, PATCH는 리소스의 일부를 업데이트합니다.

<br/>

### 이벤트 캡처링과 버블링에 대해 설명해주세요.

자바스크립트는 이벤트가 발생하는 동시에 이벤트 전파(Event Propagation)가 발생합니다.
캡처링은 이벤트가 가장 바깥쪽 부모 요소부터 시작해서, 이벤트가 발생한 실제 타깃 요소까지 내려오는 과정입니다.
버블링은 이벤트가 실제 타깃 요소에서 발생한 다음, 다시 부모 요소들을 따라 바깥으로 올라가는 과정입니다.

이벤트 전파는 중복 코드를 줄이고 효율저긴 이벤트 처리와 구조적인 이벤트 관리를 위해 만들어졌습니다.

<br/>

### RESTful API란?

REST 아키텍처 스타일을 따르는 API를 의미합니다. REST(Representational State Transfer)는 웹의 리소스를 '자원(Resource)' 개념으로 보고, 이를 HTTP 메서드를 통해 명확하게 다루는 방식을 말합니다.

- REST API 특징

1. 자원의 식별: 모든 자원은 URI(Uniform Resource Identifier)로 고유하게 식별합니다.
2. 표준 HTTP 메서드 사용: GET, POST, PUT, PATCH, DELETE
3. 무상태성: 서버는 클라이언트의 상태를 저장하지 않습니다. 매 요청은 독립적이고, 필요한 모든 정보를 요청에 담아야 합니다.
4. 계층 구조(계층화): 클라이언트는 서버에 직접 연결되지 않고 중간 서버(프록시, 게이트웨이)를 거칠 수 있습니다.

<br/>

### CORS에 대해 알려주세요.

CORS는 Cross-Origin Resource Sharing의 줄임말로, 서버와 브라우저 간에 서로 통신을 할 때, 서버는 브라우저가 자원에 접근하는 것에 대해 권한을 부여하고 이를 알려줘야 합니다. 그러지 않을 시 브라우저 측에선 해당 자원에 접근하는 것이 안전하지 않다고 판단하여 통신을 차단해버리는데, 이 때 발생하는 것이 CORS 오류입니다.

`Access-Control-Allow-Origin`과 같은 CORS 정택을 통해 예외적으로 교차 출처 요청을 허용할 수 있습니다.

<br/>

### JWT방식과 Session방식의 차이

Session은 서버의 메모리를 사용하여 데이터를 저장하고, 로컬에는 세션 ID만을 쿠키에 저장하여 이를 서버로 보내 데이터를 받아오는 방식이므로, 매번 유저가 요청할 때마다 전체 데이터를 보내주어 서버에 과부하가 생길 위험이 큽니다. JWT는 필요한 정보를 payload에 담고, 암호화/복호화 데이터를 header에 넣어 한 번에 암호화하고, 이 토큰을 서버에 보내 검증 여부만 서버에서 전송받으므로 서버에 무리가 덜하지만 토큰을 탈취당하면 보안상 문제가 발생할 수 있습니다.

<br/>

### 주소창에 URL을 입력하면 어떤 일이 생길까요?

1. 브라우저가 URL을 해석해서 도메인을 찾고,
2. DNS 조회를 통해 서버 IP를 알아냅니다. (로컬 캐시 → OS 캐시 → 라우터 → ISP 캐시 → 최종 DNS 서버 순으로 조회)
3. 서버와 TCP 3-way 핸드셰이크를 통해TCP 연결을 맺고, HTTPS라면 TLS(SSL) 암호화도 합니다.
4. 그 다음 서버에 HTTP 요청을 보내고, 서버가 HTML 파일을 응답합니다.
5. 브라우저는 HTML을 파싱해서 DOM을 만들고, CSS, JS 같은 추가 리소스를 가져와서 최종적으로 화면에 페이지를 렌더링합니다.

- TCP 3-way 핸드셰이크
  서버와 클라이언트가 통신을 시작하기 전에 연결을 확립하는 과정입니다. 즉, 서로 통신할 준비가 됐는지 확인해서 신뢰성 있는 연결을 만들기 위해 필요합니다.

<br/>

### 크로스 브라우징이란?

### DOM과 가상 DOM 이란

### OAuth란? OAuth의 동작 방식은?

### Recoil, Jotai, React-Query 등 다양한 상태 관리 패턴을 사용한 경험이 있나요? 각 패턴의 장단점에 대해 설명해 주세요.

### Pre-rendering과 Hydration이란 무엇인가요? 각각의 과정에서 발생할 수 있는 문제점은 무엇인가요?

### 깊은 복사, 얕은 복사에 대해서 설명해주세요. 그리고 예시를 들어주세요.

### FSD 아키텍처란 무엇이며, 기존의 다른 아키텍처 패턴(MVC, MVVM 등)과 비교했을 때 어떤 장점이 있나요?

<br/>

## 경험

### 주도적으로 프로젝트를 진행한 경험이 있나요? 그 과정에서 어떤 어려움이 있었고, 어떻게 해결하셨나요?

### UI/UX 개발에 대한 관심과 경험이 있다고 하셨는데, 최근에 주목한 UI/UX 트렌드가 있다면 무엇인가요?

<br/>

## 일반

### 자기소개

### 지원동기와 이 포지션이 적합하다고 생각하시는 이유를 말씀해 주세요.

### 퇴사 사유가 뭔가요??

### 개발을 시작하게 된 계기와 프론트엔드를 선택한 이유

### 최근에 관심있는 기술이 있을까요?

### 본인은 어떤 방식으로 성장하나요?

### 입사하게 되면 하고싶은 일이나 기대하는 점은 무엇일까요?

### 자신의 장점과 단점에 대해 말해주세요.

### 프런트엔드 개발은 지속적으로 학습해야 하는 분야인데 어떤식으로 학습을 하고 있는지?
