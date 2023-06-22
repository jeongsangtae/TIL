# ES6

### class (클래스)

<br />

- class (클래스)

  - 객체를 위한 핵심 청사진
  - 클래스는 JS 객체를 위한 청사진이라는 것을 이해해야 한다.
  - 클래스는 생성자 함수와 비슷하고 상속은 프로토타입과 비슷하다.
  - 프로퍼티와 메소드를 가진다.
  - 메소드는 클래스에 정의한 함수, 프로퍼티는 클래스에 정의한 변수
  - 메소드는 클래스와 객체에 추가되는 함수 같은 것
  - 프로퍼티는 클래스와 객체에 추가되는 변수같은 것
  - new 키워드로 클래스의 인스턴스를 생성
  - 다른 클래스에 있는 프로퍼티와 메소드를 상속하면 잠재적으로 새로운 프로퍼티와 메소드를 추가한다는 뜻
  - 만약 다른 클래스에서 프로퍼티와 메소드를 상속해서 사용하려면 super 생성자를 호출해서 사용해야한다. `super();`

  ```
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
  // Person 클래스에서 확장되었기 때문에 femail로 뜬다.
  ```
