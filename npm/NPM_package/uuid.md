# NPM package

### uuid ( [uuid 패키지 설치](https://www.npmjs.com/package/uuid) )

<br />

- uuid 패키지 설치 (uniform unique ID)

  - `npm install uuid`
  - 고유한 ID를 할당해주는 패키지
  - 충돌하는 파일 이름이 없도록 고유한 파일 이름을 갖게한다.
  - 다른 패키지와 마찬가지로 `const uuid = require("uuid")` 참조해주고 사용
  - ex) `restaurant.id = uuid.v4()`
  - 예시의 restaurant.id에서 id는 임의의 이름으로 내가 정한다.
  - v4 메서드,
  - v4()
    - 무작위로 생성되지만 고유함을 보장하는 유용한 고유한 ID를 제공해준다.
    - 문자열로 제공해준다. (일부 텍스트인 문자열)
