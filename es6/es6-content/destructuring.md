# ES6

### destructuring (구조분해할당)

<br />

- destructuring (구조분해할당)

  - 배열의 원소나 객체의 프로퍼티를 추출해서 변수에 저장할 수 있도록 해준다.
  - 원소나 프로퍼티를 하나만 가져와서 변수에 저장한다.
  - 배열에서 특정 원소, 객체에서 특정 속성을 추출하는 편한 방법

  ```
  const numbers = [1, 2, 3];
  [num1, num2] = numbers;
  console.log(num1, num2); // 1, 2

  const numbers = [1, 2, 3];
  [num1, , num3] = numbers;
  console.log(num1, num3); // 1, 3


  // 또 다른 예시

  // 객체 분해 할당
  const person = { name: 'John', age: 30 };
  const { name, age } = person;
  console.log(name); // 'John'
  console.log(age); // 30

  // 배열 분해 할당
  const numbers = [1, 2, 3];
  const [first, second, third] = numbers;
  console.log(first); // 1
  console.log(second); // 2
  console.log(third); // 3
  ```
