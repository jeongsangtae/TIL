# ES6

### arrow function (화살표 함수)

<br />

- arrow function (화살표 함수)

  - 기존의 함수는 `function name() {}` 이런 형태를 가지고 있다.
  - 화살표 함수는 `const name = () => {}` 이런 형태를 갖는다.
  - 화살표 함수의 구문은 function 키워드를 생략햇기 때문에 일반적인 함수보다 짧다.
  - JS에서 갖고 있던 this 키워드로 인해 생겼던 많은 문제들을 해결해주는 장점을 지니고 있다.
  - 화살표 함수 안에 this를 사용하면, 항상 정의한 객체를 나타내고, 실행 중에 갑자기 바뀌지않는다.
  - 화살표 함수에서 만약에 1개의 매개변수만 갖는다면 주변에 있는 괄호를 없앨 수 있다.
    - `const myName = (name) => { console.log(name) }` 변경 전
    - `const myName = name => { console.log(name) }` 변경 후
    - 이 방법은 매개변수가 1개인 경우에만 사용할 수 있는 방법이다.
    - 만약 전달할 매개변수가 없다면 `const myName = () => { console.log("Jeong") }` 이런식으로 정의되어야 한다. 빈 괄호 쌍이 입력되어야 함
    - `const myName =  => { console.log("Jeong") }` 이런식의 정의 방법은 문법상 유효하지 않다.
    - 1개 이상의 매개변수를 갖는다면 `const myName = (name, age) => { console.log(name, age) }` 이런식으로 반드시 괄호를 감싸줘야 한다.
    - 1개 혹은 그 이상의 매개변수를 취해서 값을 반환하는 함수를 작성하는 아주 짧고 함축적인 방법도 사용가능하다.
    - 매개변수가 여러개인 경우나 반환문이 복잡한 경우에는 괄호와 'return'을 생략할 수 없다.

  ```
  // return을 사용해서 반환
  const multiply = (number) => {
    return number * 2;
  }

  console.log(multiply(2));

  결과 값 = 4

  // return과 괄호를 제거해서 더 짧게 줄임 (매개변수가 하나이기 때문에생략 가능)
  const multiply = number => number * 2;

  console.log(multiply(2));

  결과 값 = 4
  ```
