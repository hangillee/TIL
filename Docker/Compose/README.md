## Docker Compose

### Docker Compose란?

**Compose**는 다중 컨테이너로 이루어진 Docker 어플리케이션들을 정의하고 실행하기 위한 도구. 즉, 사용자가 컨테이너 기반의 플랫폼 독립적인 어플리케이션을 정의할 수 있게 만들어줌.

여기서 **어플리케이션**은 시스템 자원과 소통 채널을 적절하게 공유하며 **함께 동작해야하는 컨테이너의 집합**으로 설계된 것을 말함. 예를 들어, 웹 서버 컨테이너와 데이터베이스 컨테이너는 함께 동작해야 하는 대표적인 컨테이너 집합.

즉, **서로 다른 컨테이너들이 함께 동작해야 한다면 하나의 어플리케이션으로 정의하고 간편하게 실행**시키는 것이 Compose.

### Docker Compose 설치하기

Compose는 3가지 방식으로 설치할 수 있음.

- Docker Desktop 설치 (가장 권장되는 방법)
- Docker Compose 플러그인 설치
- Docker Compose Standalone 설치 (비추천)

Docker Compose를 실행하기 위해서는 Docker Engine과 Docker CLI 설치가 필수 조건이기 때문에 모두 자동으로 설치되는 Docker Desktop 설치를 추천함.

### Docker Compose 사용하기

**YAML** 파일을 사용해 어플리케이션의 서비스들을 구성하고 명령어 한 줄로 YAML에 적어둔 구성 정보대로 모든 서비스들이 생성, 시작됨. **YAML 파일의 이름은 `compose.yml`이 가장 권장**됨. 물론, `docker-compose.yaml`이나 `docker-compose.yml`도 사용 가능.

```yaml
# compose.yml
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

```zsh
docker compose up
```

Compose는 운영, 스테이징, 개발, 테스트, CI Workflow 등의 모든 개발 환경에서 동작하며 어플리케이션의 전체 생명주기를 관리하기 위한 명령어를 가짐.

- `docker compose up` : 빌드, 재빌드, 어플리케이션 실행
  - `-d` : 백그라운드에서 어플리케이션 실행
- `docker compose run` : 일회성 명령을 포함한 어플리케이션 실행
- `docker image ls` : 로컬 이미지 목록 확인
- `docker compose ps` : 실행 중인 컨테이너 목록 확인
- `docker compose stop` : 컨테이너 정지
- `docker compose down` : 정지 후 컨테이너 및 네트워크 삭제
  - `--volumes` : 컨테이너에 할당된 볼륨도 함께 삭제

**볼륨**은 데이터베이스와 같이 컨테이너가 값을 영구적으로 저장해야하는 경우 할당해주는 공간으로, 지정해주지 않으면 컨테이너가 삭제되면 컨테이너가 관리하던 모든 데이터도 함께 삭제.

### Docker Compose 어플리케이션 구성

- `services` : 컨테이너 이미지와 설정을 통해 실제 실행되는 서비스
- `networks` : 서비스들이 서로 통신하기 위한 네트워크
- `volumes` : 데이터를 영구히 저장하고 공유하기 위한 파일시스템
- `configs` : 서비스에서 필요로하는 설정 정보
- `secrets` : 서비스에 제공해야하는 민감한 보안 정보

위의 목록들을 Docker Compose 파일(YAML)에 작성하여 원하는대로 Docker 어플리케이션을 구성하고 실행할 수 있음. 이 중에서 `services`는 필수로 작성해야함.

### Docker Compose 핵심 기능

- **단일 호스트에서 여러 독립적인 개발 환경을 가짐**
  **프로젝트 이름**을 통해 각 환경을 서로로부터 독립시킴.
- **컨테이너 생성 시 볼륨 데이터를 보존**
  `docker compose up` 실행 시, 이전 실행으로부터 생성된 예전 컨테이너를 찾아 그 볼륨을 새 컨테이너에 복제
- **변경된 컨테이너만 재생성**
  설정이 변경되지 않은 서비스를 재시작하면 기존에 있던 컨테이너를 재활용
- **변수와 개발 환경 간의 구성 이동을 지원**
  다른 환경이나 사용자에 맞게 구성을 커스텀할 수 있음

### Docker Compose가 자주 사용되는 사례

- 개발 환경
- 자동 테스트 환경
- 단일 호스트 배포

### p.s.

Docker Compose의 버전이 V2가 되면서 V1에 대한 지원이 중단될 예정. 따라서 레거시 코드들의 V1 기준 Docker Compose 사용 예시를 참고할 때는 주의해야함!
