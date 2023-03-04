# NPM package

### ejs ( [ejs 패키지 설치](https://www.npmjs.com/package/ejs) )

<br />

- ejs 패키지

  - `npm install ejs`
  - Embedded Javascript의 약자
  - JS가 내장된 HTML파일
  - 보통 express 함수를 호출해서 객체를 생성한 후 가능하다.
  - node express 앱에서 사용할 수 있는 지원되는 보기 엔진 중 하나이다. (view engine)
  - 완성된 HTML 코드가 방문자에게 제공되기 전에 서버 사이트에서 평가될 추가 기능과 플레이스홀더가 있는 HTML 코드를 작성할 수 있게 해주는 추가 패키지
  - HTML 페이지를 동적 콘텐츠로 풍부하게 만들어 줄 수 있다.

  - 사용하는 폴더의 모든 HTML 파일의 이름을 바꿔야한다. (1단계)

    - ex) index.html -> index.ejs
    - 해당 파일의 내용을 변경할 필요는 없지만 모든 확장자를 변경해야 한다.
    - get 메서드로 파일 경로를 생성하는 대신 경로가 발생하고 파일을 수동으로 다시 보내면 `res.render`로 대체할 수 있다.(2단계)

      - 템플릿 엔진을 사용해서 파일을 만들고 HTML로 변환하면 브라우저로 다시 전송된다.

      ```
      app.set("views", path.join(__dirname, "views"));
      app.set("view engine", "ejs");

      app.get("/", function (req, res) {
        // const htmlFilePath = path.join(__dirname, "views", "index.html");
        // res.sendFile(htmlFilePath);
        res.render("index");
      });

      // res.render()를 사용해서 훨씬 간단해진 코드 (위에 주석은 ejs사용전 코드)
      ```

  - EJS를 사용해 동적 콘텐츠 렌더링

    - 여기서 핵심은 `<%= numberOfRestaurants %>`
    - `<%= (모든 placeholer & 변수 지정 가능) %>`
    - `<%= %>` 단일 값을 출력하고 싶을 때 사용
    - for 반복문을 작성하기 위해서는 `<% for () { %>`...`<% } %>`
    - if 문 사용 `<% if () { %>`...`<% } else { %>`...`<% } %>`
    - includes

      - 기본적으로 여러 페이지에서 잠재적으로 사용할 수 있는 페이지의 일부를 포함하는 또 다른 EJS파일이다.
      - includes를 사용해 큰 HTML 파일을 더 작고 관리하기 쉬운 조각으로 나눌 수 있다.
      - `<%- include(경로) %>` 추가로 예시처럼 확장자는 필요없다.
      - ex) `<%- include('includes/header') %>`
      - includes에서 첫번째 키는 경로 두번째 키로 동적 데이터의 키를 매개변수로 가져와서 사용할 수 있다.
      - `<%- include(경로, 매개변수) %>`
      - ex) `<%- include('includes/restaurants/restaurant-item', {restaurant: restaurant}) %>`

    <br />

  - EJS 확장 프로그램 설치
    - ejs 검색 후 **EJS language support** 설치
