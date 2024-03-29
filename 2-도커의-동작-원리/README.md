# 도커의 동작 원리

## 도커의 동작 원리

### 도커의 구조

- 물리 서버 -> 운영체제 -> 도커 엔진 -> 컨테이너 -> 프로그램 or 데이터

- 컨테이너 안에는 <strong>운영체제 '비슷한 것'</strong>이 들어있다.
    - 운영체제는 소프트웨어나 프로그램의 명령을 하드웨어에 전달하는 역할을 한다.
    - 운영체제는 '커널'이라는 부분과 '그 외의 주변 부분'으로 구성된다.
    - <strong>주변 부분이 프로그램의 내용을 커널에 전달</strong>하고 커널이 하드웨어를 다룬다.
    - 따라서, 도커에서는 컨터에너 속에 <strong>운영체제의 주변 부분을 넣어</strong> 프로그램의 명령을 운영체제의 커널에 전달한다.
    - 운영체제의 전체를 컨테이너에 넣지 않기 때문에, <strong>도커는 가볍다</strong>.

### 도커는 기본적으로 '리눅스용'이다

- 도커는 밑바탕에서 <strong>리눅스 운영체제가 동작</strong>하는 것을 전제로 하는 구조이다.

- 윈도우나 macOS용 소프트웨어는 컨테이너에 넣어도 동작하지 않는다.
    - 본래 도커의 필요성은 서버 환경의 전제로부터 나왔다.

- 하지만 실제로는 윈도우 또는 macOS 서버에서 도커를 사용할 수 있다.
    - VirtualBox나 VMware 같은 <strong>가상 환경 </strong>위에 리눅스 운영체제를 설치하고 도커를 실행하거나,
    - '윈도우용 또는 macOS용 도커 데스크톱'처럼 <strong>도커를 실행하는 데 필요한 패키지</strong>를 설치해 사용한다.
    - 즉, 어떠한 형태로든 리눅스 운영체제를 갖춰야 한다.

## 도커 허브와 이미지, 그리고 컨테이너

### 이미지와 컨테이너

- 이미지는 <strong>컨테이너의 설계도</strong> 역할을 한다.

- 컨테이너로 이미지를 만들수 있다. 기존 이미지를 수정하고 <strong>수정된 이미지</strong>를 만드는 것이다.

- 이미지를 이용해서 다른 도커 엔진에 쉽게 컨테이너를 생성할 수 있다.

### 도커 허브와 도커 이미지

- 도커 허브는 공식적으로 운영되는 <strong>이미지를 배포하는 서비스</strong>이다.

- 도커 허브는 누구나 자유롭게 이미지를 등록할 수 있기 때문에 안전하지 못한 이미지가 있을 수 있다.

- 가급적이면 공식 이미지를 사용한다.

- 도커를 사용할 때의 원칙 중 하나로, <strong>'한 컨테이너에 한 프로그램'</strong>이라는 것이 있다.

## 도커 컨테이너의 생애주기와 데이터 저장

### 도커 컨테이너는 '쓰고 버리는' 일회용품

- 컨테이너 하나를 업데이터하면서 계속 사용하기보다는 업데이트된 소프트웨어가 들어있는 <strong>새로운 컨테이너를 사용</strong>하는 것이 좋다.

- 컨테이너를 <strong>만들고 -> 실행하고 -> 종료하고 -> 폐기</strong>하는 과정 = 컨테이너의 생애주기

### 데이터 저장

- 컨테이너를 폐기하면 해당 컨테이너 안에서 편집했던 파일은 사라진다.

- 이를 방지하기 위해 도커가 설치된 물리 서버(호스트)의 <strong>디스크를 마운트</strong>해 이 디스크에 파일을 저장한다.

## 도커의 장점과 단점

### 도커의 성질

- <strong>독립된 환경</strong> 덕분에 여러개의 컨테이너를 띄울 수 있다.

- <strong>이미지</strong>를 만들 수 있기 때문에 컨테이너의 교체가 쉽고 업데이트가 쉽다.

- 컨테이너에 <strong>커널</strong>을 포함시킬 필요가 없어 가상환경에 비하면 매우 가볍다.

### 도커의 장점

- 한 대의 물리 서버에 <strong>여러 대의 서버</strong>를 띄울 수 있다.

- <strong>서버 관리가 용이</strong>하다. 항상 최신 상태의 소프트웨어를 유지하기 쉬워지고, 컨테이너의 교체나 수정이 쉽다.

- 서버에 대해 잘 몰라도 쉽게 컨테이너를 사용할 수 있다.

### 도커의 단점

- 컨테이너 안에서는 <strong>리눅스용 소프트웨어</strong>밖에 지원하지 않는다.

- 호스트 서버에 문제가 생기면 <strong>모든 컨테이너에 영향</strong>을 미친다.

- 컨테이너 하나만 사용할 때에는 장점을 느끼기 어렵다. 도커 엔진이 단순한 오버헤드에 지나지 않기 때문이다.

### 도커의 주 용도

- 팀원 모두에게 <strong>동일한 개발환경</strong>
    - 프로젝트별로 컨테이너를 따로 사용한다.
    - 개발환경과 운영환경 차이가 근본적으로 사라진다.


- 새로운 버전의 테스트
    - 새로운 버전뿐만 아니라 변경된 환경에 대한 테스트에도 유용하다.

- 동일한 서버가 여러 대 필요한 경우
    - 컨테이너를 이용해 한 대의 물리 서버에 똑같은 서버를 여러개 생성한다.
    - 물리 서버를 공유하므로 비용을 절약할 수 있다.
