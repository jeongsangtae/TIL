# NPM package

### nodemon ( [nodemon 패키지 설치](https://www.npmjs.com/package/nodemon) )

<br />

- nodemon

  - `npm install --save-dev nodemon`
  - 개발 중에만 필요한 패키지로 save-dev는 실제로 코드를 감시하고 코드가 변경될 때마다 노드 서버를 다시 시작한다.
  - 이 패키지가 없을 경우엔 무언가를 변경할 때마다 수동으로 서버를 종료했다가 다시 시작해야 하고 약간 성가시기 때문에 필요하다
  - 노드가 아닌 노드몬으로 실행해야 한다.
  - 노드몬으로 실행하면 이전처럼 서버가 시작된다. 뿐만 아니라 코드 파일과 프로젝트에 있을 수 있는 다른 코드 파일을 지켜보고 코드 파일, 특히 JS 파일을 변경할 때마다 서버를 다시 시작하고, 변경 사항을 저장한다.
  - `package.json - "scripts"` 부분에 `"start": "nodemon app.js"`를 추가해 `app.js` 파일을 시작해서 서버를 시작할 수 있게 한다.
  - `--save-dev`
    - 패키지를 개발 종속성이라 표시하는 것
    - 개발 중에만 사용하는 패키지라는 사실을 알려주는 용도
  - 실행할 떄 `nodemon app.js`로는 작동하지않는다.
    - 전체 컴퓨터에서 전역적으로 사용 가능한 도구가 아니라 해당 프로젝트에 패키지로만 설치되었기 때문이다.
  - package.json 폴더에서 변경해야할 사항
    - 스크립트 자동화를 위해서 "scripts" 에서 변경해준다.
    - `start`로 하는게 간편하다.
    - 임의적으로 정한다면 "(myscriptname)": "nodemon (myapp.js)"
    ```
    "scripts": {
      "start": "nodemon app.js"
      // "myscriptname": "nodemon myapp.js"
    }
    ```
  - npm start로 실행
    - 사실상 npm run start와 다름이 없다.
  - 임의적으로 정한 이름으로 실행
    - npm run (myscriptname)
