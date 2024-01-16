# Implements vs Extends

## Extends

Extends 키워드는 자바스크립트에서도 사용할 수 있으며, 부모 클래스에 있는 프로퍼티나 메서드를 상속해서 사용할 수 있게 만들어 준다.

```tsx
class Car {
  milege = 0;
  price = 100;
  color = 'white';

  drive() {
    return 'drive!';
  }

  brake() {
    return 'brake';
  }
}

class Ford extends Car {
  
}

const myFordCar = new Ford();
```

## Implements

Implements 키워드는 새로운 클래스의 타입 체크를 위해서 사용되며, 그 클래스의 모양을 정의할 때 사용한다.

부모 클래스의 프로퍼티와 메서드를 상속 받아서 사용하는 것이 아니다.

```tsx
class Car {
  milege = 0;
  price = 100;
  color = 'white';

  drive() {
    return 'drive!';
  }

  brake() {
    return 'brake';
  }
}

interface Part {
  seats: number;
  tire: number;
}

class Ford implements Car, Part {
  milege = 1;
  price = 2;
  color = 'white';
  seats = 2;
  tire = 3;

  drive() {
    return 'drive!';
  }

  brake() {
    return 'brake';
  }
}

const myFordCar = new Ford();
```

---

**참고 자료**

- <https://fastcampus.co.kr/dev_online_frontend>
