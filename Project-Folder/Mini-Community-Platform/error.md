# 미니 커뮤니티 플랫폼 에러 내용

### 배포 중에 발생한 에러

- 0.0.0.0/0에 바인딩 못하는 에러
  - 에러
    - Render에서 배포할 때 프론트엔드만 배포하려고 Root Directory에 "./frontend" 내용을 추가해준 상태로, 백엔드에 구성된 서버에 접근하지 못하게 되면서 발생하는 에러 발생
  - 해결 방법
    - 문제 해결을 위해서 Root Directory에 빈 내용으로 구성해주고, mini-community-platform 내용이 루트로 지정되도록 구성
    - Start Command를 통해서 프론트엔드에 들어가 실행되고, 백엔드에 들어가서 각각 실행되도록 구성을 변경해 주고 해결

<br />

- vite: not found 에러
  - 에러
    - Render에서 배포할 때 vite 관련 내용이 포함되면 자주 발생하는 에러
  - 해결을 위해 테스트한 과정과 방법
    - 방법 1
      - `npm install --production=false`, `npm ci` 같은 내용이 Start Command를 통해서 실행되도록 구성해 테스트했으나 해결되지 않음
        - ex1) `cd frontend && npm install --production=false && npm run build && cd ../backend && npm start`
        - ex2) `cd frontend && npm ci && npm run build && cd ../backend && npm start`
    - 방법 2
      - 로컬 환경에서 `npm run build`를 통해서 dist 폴더를 생성하고, 빌드 파일을 Render에 업로드해 정적 사이트를 생성
        - 이 방법의 문제점은 매번 업데이트할 때마다 로컬에서 코드를 수정하고 `npm run build`를 통해서 다시 빌드하고 새로운 dist 폴더를 생성해 업로드하는 방식으로 진행되므로 번거로운 과정이 반복되어서 적절하지 못함
        - GitHub와 연동해서 커밋할 때마다 Render가 자동으로 빌드하고 배포하도록 자동화된 배포 방식이 필요
    - 방법 3
      - Render는 기본적으로 `production` 모드로 설치하기 때문에 `devDependencies`에 있는 패키지를 설치하지 않아서 vite 에러가 발생하므로 `production` 모드에서도 vite를 설치하게 하거나 `npm install --production=false`, `npm ci` 명령어를 사용해 모든 패키지를 설치하도록 설정이 필요
        - `cd frontend && npm ci && npm run build && cd ../backend && npm start`
          - 이 명령어로는 해결되지 않음
        - `cd frontend && npm install --production=false && npm run build && cd ../backend && npm start`
          - 이 명령어로는 vite build가 되던 와중에 "JavaScript heap out of memory (메모리 부족)" 이라는 새로운 에러가 발생하므로, 추가 조치가 필요함
  - 결과
    - not found에 대한 에러는 `npm install --production=false`, `npm ci` 같은 내용을 추가해 해결이 된 것 같지만, 추가 오류가 발생하기 때문에 이 부분에 대한 수정이 필요

<br />

- "JavaScript heap out of memory (메모리 부족)" 에러
  - 에러
    - vite를 빌드하는 동안 너무 많은 메모리를 사용해서 Node.js가 메모리 한계를 초과해 발생하는 에러
      - 메모리 제한을 늘려줘야 하는데, 이 때 Render에서 웹 서비스를 배포할 때 Node.js의 메모리 제한을 늘리게 되면 배포 자체에 문제가 생길 수 있다고 생각되었지만, 메모리 제한을 늘리는 방법은 주로 빌드 과정에서 발생하는 메모리 부족 문제를 해결하기 위한 것이기 때문에 실제 웹 서비스가 실행되는 데에는 큰 영향을 주지 않으므로 문제되지 않음
  - 해결을 위해 테스트한 과정과 방법
    - 방법 1
      - `npm install --production=false` 명령어를 빼고 `NODE_OPTIONS="--max-old-space-size=4096` 명령어를 추가해 테스트하면 메모리 부족 에러는 뜨지 않고, vite 관련 오류가 다시 발생
        - `cd frontend && NODE_OPTIONS="--max-old-space-size=4096" npm run build && cd ../backend && npm start`
        - 이 명령어로 테스트하면 vite와 같은 개발 의존성이 설치되지 않아 vite 관련 오류가 다시 발생하므로 `npm install --production=false` 명령어를 포함해 다시 시도해야 함
    - 방법 2
      - `cd frontend && npm install --production=false && NODE_OPTIONS="--max-old-space-size=4096" npm run build && cd ../backend && npm start`
      - 이 명령어로 테스트하면 nodemon 관련 오류가 발생하기 때문에 백엔드에서도 `npm install --production=false` 명령어를 포함해 설치를 시도해봐야 함
  - 결과
    - `NODE_OPTIONS="--max-old-space-size=4096` 명령어를 `npm install --production=false` 명령어와 함께 추가해 테스트하면, vite not found 에러와 메모리 부족 에러가 뜨지 않는 것을 확인
    - 하지만 저 두 개의 명령어로 테스트하면 앞에서 발생한 오류는 해결되지만, 새로운 오류인 nodemon 관련 오류가 발생하기 때문에 백엔드에서 추가 작업이 필요함

<br />

- nodemon 관련 에러
  - 에러
    - nodemon 관련 오류는 nodemon이 devDependencies에 포함되어 있는데, 백엔드에 개발 의존성 내용을 함께 설치하지 않아서 발생하는 에러
  - 해결 과정
    - `cd frontend && npm install --production=false && NODE_OPTIONS="--max-old-space-size=4096" npm run build && cd ../backend && npm install --production=false && npm start`
      - 이 명령어로 시도했을 때 배포에 성공했지만, 메인 페이지에서 리디렉션한 횟수가 너무 많다는 메시지와 "ERR_TOO_MANY_REDIRECTS" 메시지가 뜨는데, 이 내용은 무한 리디렉션 루프가 발생하므로 추가 수정 필요
  - 결과
    - 백엔드에 개발 의존성 내용이 추가되지 않아서 nodemon 에러가 발생하기 때문에 `npm install --production=false` 내용을 추가해 문제를 해결해 주었지만, 무한 리디렉션 루프 문제가 발생하기 때문에 추가 테스트를 진행해서 오류를 고쳐줘야 함

### 배포 후에 발생한 에러
