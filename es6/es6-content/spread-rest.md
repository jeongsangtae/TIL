# ES6

### spread, rest

<br />

- spread, rest operators (스프레드, 레스트 연산자)

  - `...` 점 3개로 이루어진 연산자
  - 스프레드 연산자는 배열의 원소나 객체의 프로퍼티를 나누는데 사용한다. 그래서 배열이나 객체를 펼쳐 놓는다.
  - 스프레드는 모든 원소와 프로퍼티를 가져와서 새 배열이나 객체 또는 내가 사용하는 어떤 것에 전달하는 방식
  - 레스트 연산자도 스프레드 연산자와 같은 연산자이지만 약간 다르게 사용된다. 함수의 인수 목록을 배열로 합치는데 사용된다.
  - 레스트 연산자는 함수의 인수 목록에서 사용한다.
  - 레스트 연산자는 사용 빈도는 낮지만 함수에서 사용된다. ES6의 화살표 함수도 사용가능하다.

  ```
  spread

  const newArray = [...oldArray, 1, 2]
  // oldArray 배열에 있는 모든 원소들을 새로운 배열에 추가하고, 거기에 원소 1과 2를더 추가하는 구문
  const newObject = { ...oldObject, newProp:5 }
  // oldObject의 모든 프로퍼티와 값을 꺼내서 새 객체의 키 값을 추가한다.
  // 만약 새로운 프로퍼티를 갖게 되면 위에 있는 newProp:5로 덮어쓰여진다.
  // 내가 갖고 있는 프로퍼티가 우선권을 갖는다.


  spread 배열 다른 예시

  const numbers = [1, 2, 3];
  const newNumbers = [...numbers, 4];

  console.log(newNumbers); // 결과 값: [1, 2, 3, 4]


  spread가 없을 때는 이렇게 출력

  const numbers = [1, 2, 3];
  const newNumbers = [numbers, 4];

  console.log(newNumbers); // 결과 값:[[1, 2, 3], 4]


  spread 객체 다른 예시

  const person = {
    name: "Jeong"
  };

  const newPerson = {
    ...person,
    age: 28
  }

  console.log(newPerson); // 결과 값

  [object Object] {
    age: 28,
    name: "Jeong"
  }



  rest

  function sortArgs(...args){
    return args.sort()
  }

  // 여기서 sortArgs는 매개변수를 무제한으로 받는다.
  // 1개 이상의 매개변수를 갖게 되어도 모두 배열로 통합된다.
  // 그 다음 매개변수 목록에 배열 메소드를 적용하거나 무엇이든 원하는 편한 방법으로 매개변수를 저장할 수 있다.


  rest 다른 예시

  function myFunction(...rest) {
    console.log(rest);
  }

  myFunction(1, 2, 3); // 결과 값: [1, 2, 3]


  rest 연산자와 화살표 함수를 함께 사용

  const filter = (...rest) => {
    return rest.filter(el => el === 1);
  }

  console.log(filter(1, 2, 3)); // 결과 값: [1]
  ```
