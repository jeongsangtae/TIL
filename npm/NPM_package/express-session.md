# NPM package

### express session ( [express session 패키지 설치](https://www.npmjs.com/package/express-session) )

<br />

- express session 패키지

  - `npm install express-session`
  - resave: false
    - 세션의 데이터가 실제로 변경된 경우에만 DB에서 업데이트 되는데 영향을 미친다.
  - resave: true
    - 세션 데이터가 변경되지 않은 경우에도 들어오는 모드 요청에 관해 새 세션이 DB에 저장된다.
    - 단점이 있는데 동일한 클라이언트가 짧은 시간에 많은 요청을 보내는 경우 이전세션 상태 저장이 아직 완료되지 않았을 수 있고 잘못된 새로운 빈 상태로 덮어쓸 수 있다. 그래서 resave는 false로 설정
  - saveUninitialized: false
    - 세션이 실제로 DB에만 저장되도록 하거나 데이터가 있을 때 어디든지에 저장되도록 한다.
