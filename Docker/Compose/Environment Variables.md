## Environment Variables

> Docker Compose에서 각 서비스에 환경변수를 설정하는 방법

Compose에서는 컨테이너들의 런타임 중에 설정 정보를 넘기기 위한 환경 변수를 설정할 수 있음. 주로 어플리케이션이 실행될 때 필요한 정보나 각 개발 환경 별로(개발, 테스트, 배포 등) 필요한 설정을 따로 지정하기 위해 사용.

### 환경 변수 설정 방법

- `.env` 파일 사용
- Compose 파일에서 `environment` 속성 사용
- Compose 파일에서 `env_file` 속성 사용
- `shell`이나 호스트에서 환경 변수 직접 불러오기
- Docker CLI에서 `-e` 옵션 사용

주의할 점은 여러 곳에서 같은 이름의 환경 변수를 설정하면 Docker에서 지정한 우선순위 규칙에 따라 자동으로 결정하기 때문에 의도치 않은 결과가 발생할 수 있음.

또한, 비밀번호와 같은 민감한 정보는 환경 변수대신 Compose 파일의 `secrets` 속성을 사용하는 것이 좋음.

### 환경 변수 우선 순위

1. `docker compose run -e` 옵션으로 설정한 환경 변수
2. 호스트의 `shell`에서 가져온 환경 변수
3. Compose 파일의 `environment` 속성
4. `docker compose --env-file` 옵션으로 `.env`에서 가져온 환경 변수
5. Compose 파일의 `env_file` 속성으로 지정한 `.env`에서 가져온 환경 변수
6. 프로젝트 디렉토리에 `compose.yaml`과 함께 둔 `.env` 파일
7. `Dockerfile`에서 `ENV`나 `ARG`로 설정한 환경 변수 (4, 5, 6이 없을 경우에만 동작)
