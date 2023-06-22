# ES7

### class (클래스)

<br />

- class (클래스)

  - ES7에서는 클래스에 바로 프로퍼티를 할당할 수 있다.
  - 생성자 함수를 호출하지 않아도 된다.
  - 뒤에서 생성자 함수를 사용해서 변환되지만, 코딩하는 시간을 절약할 수 있다. 메소드의 경우에도 비슷하다.
  - 메소드는 값으로 함수를 저장하는 프로퍼티라고 생각하면 된다.
  - 프로퍼티의 값으로 화살표 함수를 사용하기 때문에 this 키워드를 사용하지 않아도 된다.
  - this 키워드를 생성자 함수에서는 빠지지만 뒤에서 사용해서 변환은 해주어야한다.
  - constructor인 생성자 함수도 빠지고, 상속받는 클래스에서 `super();`가 필요없게 된다.

  ```
  ES6

  constructor() {
    this.myProperty = "value"
  }

  myMethod () { ... }


  ES7

  myProperty = "value"

  myMethod = () => { ... }
  ```

  ```
  ES6

  class Human {
    constructor() {
      this.gender = "male";
    }

    printGender() {
      console.log(this.gender);
    }
  }

  class Person extends Human {
    constructor() {
      super();
      this.name = "Jeong";
      this.gender = "femail";
    }

    printMyname() {
      console.log(this.name);
    }
  }

  const person = new Person();
  person.printMyname(); // 결과 : "Jeong"
  person.printGender(); // 결과 : "femail"



  ES7

  class Human {
    gender = "male";

    printGender = () => {
      console.log(this.gender);
    }
  }

  class Person extends Human {
    name = "Jeong";
    gender = "femail";

    printMyname = () => {
      console.log(this.name);
    }
  }

  const person = new Person();
  person.printMyname(); // 결과 : "Jeong"
  person.printGender(); // 결과 : "femail"
  ```
