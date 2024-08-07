# 도커 컴포즈를 익히자

## 도커 컴포즈란?

- 시스템 구축과 관련된 명령어를 하나의 정의 파일(YAML 포맷)에 기재
- 명령어 한번에 시스템 전체를 실행하고 종료와 폐기까지 한번에 하도록 도와주는 도구
- 컨테이너나 볼륨을 `어떠한 설정으로 만들지`에 대한 항목이 정의

### 도커 컴포즈의 구조

- `up` 커맨드:
  - 정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행한다.
  - 정의 파일에는 네트워크나 볼륨과 같은 주변 환경을 기재할 수 있다.
- `down` 커맨드:
  - 컨테이너와 네트워크를 정지 및 삭제한다.
  - 볼륨과 이미지는 삭제하지 않는다.

### 도커 컴포즈 vs Dockerfile vs 쿠버네티스
<img src=from_docker-image_to_docker-container.png alt="image1" width="400">

- Dockerfile은 이미지를 만드는 데 쓰임
- 도커 컴포즈는 컨테이너, 네트워크, 볼륨을 만드는 데 쓰임
- 쿠버네티스는 컨테이너를 관리하는 도구


<br />

## 도커 컴포즈 파일을 작성하는 법
- 정의 파일은 YAML 형식을 따른다.
- 파일 이름은 `docker-compose.yml`이라고 짓는다. 
  - -f 옵션을 사용해 다른 이름을 사용할 수 있다.
- 컴포즈 파일은 맨 앞에 `컴포즈 버전`을 적고, 그 뒤로 `services`, `networks`, `volumes`를 차례로 기재한다.
  ```yaml
  # 컴포즈 파일의 작성 예
  version: "3"  # 버전 기재

  services:     # 컨테이너 관련 정보
    CONTAINER_NAME1:
      image: IMAGE_NAME1
      networks:
        - NETWORK_NAME1
      ports:
        - PORT_SETTING
    CONTAINER_NAME2:
      image: IMAGE_NAME2

  networks:     # 네트워크 관련 정보
    NETWORK_NAME1:

  volumes:      # 볼륨 관련 정보
    VOLUME_NAME1:
    VOLUME_NAME2:
  ```

### 컴포즈 파일의 항목
#### 주 항목
| 항목     | 내용                         |
|----------|------------------------------|
| services | 컨테이너를 정의한다.         |
| networks | 네트워크를 정의한다.         |
| volumes  | 볼륨을 정의한다.             |

#### 자주 나오는 정의 내용
| 항목           | docker run 커맨드의 해당 옵션 또는 인자 | 내용                                           |
|----------------|-----------------------------------------|------------------------------------------------|
| environment    | -e                                      | 환경변수 설정                                  |
| depends_on     | 없음                                    | 다른 서비스에 대한 의존관계를 정의             |
| restart        | 없음                                    | 컨테이너 종료 시 재시작 여부를 설정            |
| command        | 커맨드 인자                             | 컨테이너 시작 시 기존 커맨드를 오버라이드       |
| container_name | --name                                  | 실행할 컨테이너의 이름을 명시적으로 설정        |
| dns            | --dns                                   | DNS 서버를 명시적으로 지정                     |
| env_file       | 없음                                    | 환경설정 정보를 기재한 파일을 로드             |
| entrypoint     | --entrypoint                            | 컨테이너 시작 시 ENTRYPOINT 설정을 오버라이드   |
| external_links | --links                                 | 외부 링크를 설정                               |
| extra_hosts    | --add-host                              | 외부 호스트의 IP 주소를 명시적으로 지정        |
| logging        | --log-driver                            | 로그 출력 대상을 설정                          |
| network_mode   | --network                               | 네트워크 모드를 설정                           |

- `depends_on`은 `다른 서비스에 대한 의존관계`를 나타낸다.
  - 컨테이너를 생성하는 순서나 연동 여부를 정의한다.
    - e.g., penguin 컨테이너의 정의에 `depends_on: -namgeuk`라는 내용이 포함됐다면 namgeuk 컨테이너를 생성한 다음에 penguin 컨테이너를 만든다.
- `restart`는 컨테이너 종료 시 재시작 여부를 설정한다.

#### restart의 설정값
| 설정값         | 내용                                                           |
|----------------|----------------------------------------------------------------|
| no             | 재시작하지 않는다.                                             |
| always         | 항상 재시작한다.                                               |
| on-failure     | 프로세스가 0 외의 상태로 종료됐다면 재시작한다.                |
| unless-stopped | 종료 시 재시작하지 않음. 그 외에는 재시작한다.                 |

<br />

## 도커 컴포즈 실행
### 도커 컴포즈 커맨드