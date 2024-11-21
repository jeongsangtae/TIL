# 미니 커뮤니티 플랫폼 에러 내용

### 배포 중 발생한 에러

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

<br />

- 무한 리디렉션 오류와 프론트엔드 내용이 보이지 않고, 백엔드 내용이 보여지는 것에 대한 에러
  - 에러
    - 배포한 페이지에 접속하면 무한 리디렉션 루프로 인해 제대로 내용이 출력되지 않음
    - 다른 페이지에서도 리디렉션 관련 문제가 발생하는 지 확인하기 위해 "/posts"로 이동하고 확인한 결과, 프론트엔드 내용이 아닌 백엔드 내용이 보여지는 문제를 확인
  - 해결을 위해 테스트한 과정과 방법
    - 프론트엔드 내용이 보여지기 위해 시도한 방법 1
      - 백엔드 코드에서 프론트엔드 정적 파일을 서빙하는 부분이 빠져있기 때문에 이와 관련된 내용을 추가
        - `express.static()` 내용을 사용해서 "frontend/dist" 폴더에서 빌드된 정적 파일들을 서빙하도록 설정
        - [코드 내용](https://github.com/jeongsangtae/mini-community-platform/commit/05f54de51d0f65d25293a66d0a9e6ac764a3e482)
        - [코드 내용](https://github.com/jeongsangtae/mini-community-platform/commit/778d65e0849a01d0d37803258b58841a91a1d7eb)
      - 이 방법으로 제대로 해결이 안되기 때문에 다른 방법이 필요
    - 프론트엔드 내용이 보여지기 위해 시도한 방법 2
      - Render에서 빌드 스크립트를 사용
        - Build Command로 npm run build를 설정하고, Start Command로 백엔드 서버를 시작하도록 설정
        - Build Command
          - `cd frontend && npm install --production=false && NODE_OPTIONS="--max-old-space-size=4096" npm run build` 내용으로 구성
        - Start Command
          - `cd backend && npm install --production=false && npm start` 내용으로 구성
      - 이 방법으로도 제대로 해결이 안되기 때문에 다른 방법 필요
  - 결과
    - 첫 번째 방법 결과
      - 백엔드에서 프론트엔드 정적파일을 서빙하는 내용을 추가하고 테스트해도 여전히 백엔드 내용이 보여지는 것을 확인
        - dist 파일이 있음에도 문제가 발생
        - 프론트엔드 경로에는 문제가 없는 것으로 판단
        - 라우팅 설정 문제가 있을 수 있다고 예상
          - "/posts" 경로가 백엔드 API와 충돌할 수 있으므로, API 경로와 프론트엔드 경로를 구분하는 방법도 생각해야 함
    - 두 번째 방법 결과
      - Build Command와 Start Command를 나눠서 명령어를 추가하고 테스트해봐도 여전히 "/posts"에서 백엔드 내용이 보여지므로 다른 방법을 사용해야 함
        - 라우팅 문제
          - 프론트엔드와 백엔드의 라우팅이 겹치거나 잘못 설정되어서, 원하는 동작이 이루어지지 않을 수 있음
          - 이 문제는 유력하다고 생각
        - 빌드 설정 문제
          - 빌드 과정에서 백엔드 파일이 포함되거나, 환경 변수가 잘못 설정되었을 수 있음
          - 이 문제는 확실하지 않음
        - 서버 설정
          - Render에서 서버 설정이 잘못되어 프론트엔드와 백엔드가 같은 포트에서 실행되거나, 잘못된 경로로 요청을 처리할 수 있음
          - 하나의 폴더 안에서 프론트엔드와 백엔드가 포함된 채로 배포하려고 시도했기 때문에 이런 문제가 생긴다고 생각이 됨
    - 테스트해 볼 해결책
      - 프론트엔드와 백엔드를 완전히 분리하여 배포하는 방법
        - 각각의 서비스가 독립적으로 동작하며, 서로 영향 없이 관리할 수 있음
        - 분리해서 배포할 경우, CORS 설정과 API 요청 경로를 명확히 설정해야 함
        - 같은 레포지토리에서 분리 배포 방식을 사용해 볼 생각
        - 프론트엔드 배포
          - Static Site를 선택하고, 같은 레포지토리를 연결
          - Root Directory를 "./frontend"로 설정
          - Build Command에 프론트엔드 빌드를 수행하는 명령어를 입력
            - `cd frontend && npm install --production=false && NODE_OPTIONS="--max-old-space-size=4096" npm run build`
        - 백엔드 배포
          - 웹 서비스로 설정하고, 레포지토리를 "mini-community-platform" 깃허브 레포지토리로 연결
          - Root Directory를 "./backend"로 설정
          - Start Command에 백엔드 서버를 시작하는 명령어를 입력
            - `cd backend && npm install --production=false && npm start`
        - 프론트엔드와 백엔드가 같은 레포지토리 내에서 각각 독립적으로 배포되도록 구성해 볼 생각
      - 라우트 경로가 겹치므로, API 요청에 대한 경로를 수정해서 확실하게 분리하는 것을 고려해야 함
        - 프론트엔드에서 API 경로를 변경하거나, 백엔드에서 API 경로를 변경하는 방향으로 수정해야 함
        - 이건 확실하지 않지만, 프론트엔드에서 API 경로에 대한 수정이 필요할 것으로 예상
  - 해결 방법
    - Render를 통해 프론트엔드와 백엔드 분리 배포
    - 프론트엔드 배포
      - Static site를 선택하고 mini-community-platform 레포지토리를 선택
      - Root Directory를 frontend로 구성
      - Build Command를 구성
        - `npm install --production=false && NODE_OPTIONS="--max-old-space-size=4096" npm run build`
      - Publish directory를 dist로 구성
      - 배포
    - 백엔드 배포
      - Web service를 선택하고 mini-community-platform 레포지토리를 선택
      - Root Directory를 backend로 구성
      - Start Command를 구성
        - `npm install --production=false && npm start`
      - Environment에 두 가지 내용을 추가
        - CORS_URL과 MONGODB_URI 추가
        - CORS_URL은 배포하는 프론트엔드 주소로 구성
        - MONGODB_URI는 연결한 Mongo Atlas 내용을 입력
      - 배포

### 배포 후 발생한 에러

- 로컬 환경의 프론트엔드에서 구성한 fetch 함수가 localhost로 연결되어 발생하는 오류
  - 에러
    - fetch 함수에서 구성한 API 경로가 동적으로 구성되어 있지 않고, localhost로 구성되어 있는 상태로 배포를 했기 때문에 배포한 사이트에서도 로컬 환경의 경로로 연결되어서 오류 발생
  - 해결을 위해 테스트한 과정과 방법
    - 배포한 프론트엔드 정적 사이트의 환경 변수와 로컬 환경의 프론트엔드 내용에서 환경 변수를 추가해 구성
      - 배포한 프론트엔드 정적 사이트에서 환경 변수 추가
        - REACT_APP_API_URL을 추가해서 백엔드와 연결되는 API 서버 주소를 추가
        - 백엔드 주소인 `https://mini-community-platform-backend.onrender.com` 내용을 저장
      - 로컬 환경의 프론트엔드 내용에서 환경 변수 추가
        - env 파일을 추가해 기존의 localhost 내용을 넣어놓고, 테스트를 위해 게시글 페이지에서 환경 변수 내용을 사용할 수 있도록 구성
        - REACT_APP_API_URL을 .env 파일에 추가하고, `http://localhost:3000` 내용이 들어가도록 구성
          - `REACT_APP_API_URL = http://localhost:3000`
        - 게시글 페이지의 fetch 함수 내용에서 환경 변수를 사용할 수 있게, apiURL 라는 변수를 추가하고 `process.env.REACT_APP_API_URL` 내용으로 구성
          - `const apiURL = process.env.REACT_APP_API_URL`
        - fetch 함수 내용의 API 경로에서 localhost 내용 대신에 apiURL 변수를 사용해서 구성
          - ex) `${apiURL}/posts`
  - 결과
    - 배포한 프론트엔드 정적 사이트에서 환경 변수도 추가하고, 그에 맞게 로컬 환경의 프론트엔드 내용에서 환경 변수를 구성해 테스트해 봤지만, 여전히 API 경로를 제대로 읽지 못하기 때문에 추가 수정이 필요

<br />

- 구성한 환경 변수 오류 1, 2
  - 에러
    - 환경 변수 오류 1
      - 게시글 페이지의 loader 함수 내에서 변수를 구성하지 않고 바깥에서 구성한 후, 사용해서 오류 발생
    - 환경 변수 오류 2
      - console.log를 추가해서 오류를 확인해보니, 환경 변수 자체를 읽지 못하는 오류가 발생
  - 해결을 위해 테스트한 과정과 방법
    - 환경 변수 오류 1
      - 문제를 해결하기 위해서 loader 함수 내에서 apiURL 변수를 구성해서 사용할 수 있도록 변경
      - 추가로 .env 파일에서 구성한 환경 변수 내용에 띄어쓰기를 사용하면 문제가 되기 때문에 띄어쓰기가 사용되지 않도록 변경
        - `REACT_APP_API_URL=http://localhost:3000`
    - 환경 변수 오류 2
      - Vite로 구성된 React 앱에서는 create-react-app으로 구성한 React앱과는 다르게 환경 변수 설정을 해줘야 제대로 읽을 수 있기 때문에 Vite 방식으로 환경 변수를 구성
      - .env 파일에서 Vite 방식으로 변경
        - `VITE_API_URL=http://localhost:3000`
      - apiURL 변수도 Vite 방식으로 변경
        - `const apiURL = import.meta.env.VITE_API_URL;`
      - Vite 방식으로 변경하고 확인해 보면 `console.log(import.meta.env)`도 문제없이 작동되어 콘솔에서 확인됨
  - 해결 방법
    - 환경 변수 오류 1
      - 게시글 페이지의 환경 변수 문제는 loader 함수 내에서 변수를 구성해 주면서 해결
      - .env 파일에서 구성한 환경 변수 내용에 띄어쓰기가 포함되지 않도록 변경해 미리 오류 예방
    - 환경 변수 오류 2
      - 구성된 환경 변수 방식을 Vite 방식으로 변경해주면서, 환경 변수를 읽을 수 있게 되고 오류가 해결됨
        - `VITE_API_URL=http://localhost:3000`
        - `const apiURL = import.meta.env.VITE_API_URL;`
      - 일반적인 React와 Vite에서 환경 변수 접근 방식
        - React
          - create-react-app에서는 환경 변수를 `REACT_APP_`으로 시작해야 함
          - REACT_APP_API_URL 라는 네이밍으로 사용 가능
          - `process.env`는 일반적으로 Node.js 환경에서 사용할 수 있고, create-react-app 같은 CRA 도구에서도 환경 변수를 읽는 데 사용되지만, Vite에서는 불가능
            - `process.env.REACT_APP_API_URL` 내용으로 환경 변수를 불러올 수 있음
          - CRA에서는 `REACT_APP_` 접두사를 가진 환경 변수만 읽을 수 있지만, Vite는 `VITE_` 접두사를 가진 변수만 읽을 수 있음
        - Vite
          - Vite에서는 환경 변수를 "VITE\_"로 시작해야 함
          - VITE_API_URL 라는 네이밍으로 사용 가능
          - `import.meta`를 사용하여 모듈 메타 정보를 제공하고, 환경 변수에 접근할 수 있게 해줌
          - `import.meta.env`는 Vite에 의해 제공되는 특수한 객체로, 이 객체를 통해 변수에 접근 가능
            - `import.meta.env.VITE_API_URL` 내용으로 환경 변수를 불러올 수 있음
