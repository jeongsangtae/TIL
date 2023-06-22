# ES6

### export & import (Modules)

<br />

- export & import

  - 내보내고 가져올 수 있다.
  - 다른 파일에서 컨텐츠를 불러올 수 있다.
  - default 키워드를 사용하는데 default는 파일에서 어떤 것을 가져오면 항상 default export가 내보낸 것을 기본값으로 가져온다는 것을 의미한다. (person.js 예시)
    - 아래 예시의 'prs' 부분은 단지 개발자가 임의적으로 변수 이름을 정하는 것뿐이므로 'p', 'ps' 이런 식으로도 얼마든지 가능 하지만 구분하기 위해서 명확한 이름을 해주는게 좋다.
  - 두 개의 다른 상수를 불러오는데 그 파일에서 특정한 어던 것을 정확하게 가리키기 위해 export 구문에서 중괄호를 사용한다. (utility.js 예시)
    - 위와 같은 방법을 named export라고 한다. 이름으로 어떤 것을 불러오기 때문, 어떤 것도 default로 지정하지 않았기 때문이다.
    - 가리키는 것을 정확하게 JS가 알게 하려면 정확한 이름을 중괄호 사이에 입력해야 한다. (방법 1)
    - 중괄호 안에서 `,`를 넣어서 한 문장으로도 가능하다. (방법 2)
  - 다른 방법으로 import하는 파일에서 `as` 키워드를 사용해 자유롭게 선택할 수 있는 별칭을 할당할 수 있다.
    - 파일에 다수의 named export가 있는 경우에는 `*`를 사용해서 모든 것을 import할 수 있다.

  ```
  // person.js

  const person = {
    name: "Jeong"
  }

  export default person


  // utility.js

  export const clean = () => {...내보내는 내용}

  export const baseData = 10;


  // app.js

  import person from "./person.js"
  import prs from "./person.js"

  import { baseData } from "./utility.js"
  import { clean } from "./utility.js"  // 방법 1

  import { baseData, clean } from "./utility.js"  // 방법 2


  // as 키워드 사용

  import { smth as Smth } from "./utility.js"


  // * 키워드 사용

  import * as bundled from "./utility.js"

  // bundled.baseData, bundled.clean과 같이 사용하면 된다.
  ```
