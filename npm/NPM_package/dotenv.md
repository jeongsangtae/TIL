# NPM package

### dotenv ( [dotenv 패키지 설치](https://www.npmjs.com/package/dotenv) )

<br />

- dotenv 패키지

  - `npm install dotenv`
  - node.js에서 환경 변수를 로드하기 위한 모듈이다.
  - 보통 개발환경에서 DB 접속 정보나 API Key와 같은 중요한 정보를 코드에 하드코딩하지 않고, 환경 변수로 관리하는 것이 보안성과 유지보수성 측면에서 더 좋다.
  - `.env` 파일에서 저장된 환경 변수를 자동으로 로드하여 `process.env` 객체에 추가한다. 이후에는 `process.env` 객체를 통해 `.env` 파일에 저장된 환경 변수에 접근할 수 있다.
  - 예로 `.env` 파일에 SECRET_KEY=1234 라는 환경 변수가 있다면, `dotenv.config()`를 호출한 이후에 `process.env.SECRET_KEY`를 통해 해당 환경 변수에 접근할 수 있다.
