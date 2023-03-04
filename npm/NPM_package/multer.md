# NPM package

### multer ( [multer 패키지 설치](https://www.npmjs.com/package/multer) )

<br />

- multer 패키지 설치

  - `npm install --save multer`
  - `multipart/form-data`를 처리하는데 사용할 수 있는 `express.js`용 미들웨어 패키지
  - 파일 업로드를 처리하는데 사용할 수 있는 패키지
  - 특히 파일 이름을 바꾸고 파일 확장자를 유지할 수 있도록 하기 위해서 사용
  - 들어오는 모든 요청에 적용하지 않고, 대신 파일이 업로드 될 것으로 예상되는 라우트에만 적용하게끔 사용
    - 일단 어디까지나 fileupload 웹 사이트에서 사용할 땐 이렇게 사용한다는 뜻
  - dest 객체
    - 경로만을 원함
  - storage 객체
    - 경로와 파일 이름에 대한 자세한 지침이 포함된 완전한 저장소 객체이다.

  ```
  // dest 객체 사용

  const upload = multer({ dest: "images" });


  // storage 객체 사용

  const storageConfig = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, "images");
    },
    filename: function (req, file, cb) {
      cb(null, Date.now() + "-" + file.originalname);
    },
  });

  const upload = multer({ storage: storageConfig });
  ```
