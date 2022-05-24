# 12. 클래스

## 12.1 프로퍼티 & 메서드 선언

클래스의 프로퍼티, 메서드, 매개변수 선언 시 `: 타입` 형태의 **타입 주석(type annotation)**을 표기함으로써 타입을 명시한다.

<br>

> 예제 12-1
> 

```tsx
class Calendar {
  month: string; // 속성을 미리 정의하고 타입을 명시해야 한다.

  constructor(month: string) {
    this.month = month;
  }

  getMonth(): string {
    return this.month;
  }
}

const calendar = new Calendar("May");
console.log(calendar.getMonth());  //result: May
```

<br>

> 자바스크립트와의 차이점
> 

```jsx
class Calendar {
	Month; // 있어도 되고, 없어도 된다.

  constructor(Month) {
    this.Month = Month;
  }

  getMonth() {
    return this.Month;
  }
}
```

자바스크립트에서는 멤버 변수(여기서는 name)를 미리 선언해도 되고 하지 않아도 문제가 되지 않는다. 하지만 타입스크립트에서는 미리 선언하지 않으면 에러가 발생하므로 주의하자.

<br>

그러다 생길 수 있는 궁금증. 그렇다면 이렇게 선언하면 안 될까?

```tsx
class Calendar {

  constructor(month: string) {
    this.month: string = month; // error!
  }

  getMonth(): string {
    return this.month;
  }
}
```

클래스 멤버 변수가 아닌, 일반적인 변수를 선언할 때 `const name: string = 'Maria'`와 같이 선언이 가능하기 때문에 위의 경우에도 가능할 것처럼 보인다. 하지만 `this`를 사용하여 멤버변수를 선언하기 때문에 위와 같이 사용하는 것을 불가능하다.

<br><br>

## 12.2 접근 제한자 (Access Modifier)

프로퍼티, 메서드, 매개변수 선언 시, 맨 앞에 접근 제한자를 표기함으로써 접근 범위를 제한 할 수 있다. `public`, `private`, `protected` 3가지의 키워드가 있으며, 접근 범위는 아래와 같다.

접근 제한자는 프로퍼티, 메서드, 매개변수의 가장 맨 앞에 표기한다. (다른 키워드랑 같이 사용 시에도 가장 맨 앞에 표기)

<br>

- `public` : 클래스 **내부/외부** **어디에서나** 모두 접근할 수 있다. default값이므로 접근 제한자를 쓰지않는 경우 public으로 선언된다.
- `private` : **해당 클래스 내부에서만** 접근할 수 있다.
- `protected` : **해당 클래스 내부와 상속받은 클래스 내부에서만** 접근할 수 있다.

<br>
<br>


보기 편하게 표로 정리하면 아래와 같다.

|  | public (default) | private | protected |
| --- | --- | --- | --- |
| 해당 클래스 내부 | O | O |O |
| 해당 클래스 외부 | O | X |X |
| 상속받은 클래스 내부 | O | X | O |

> 예제 12-2
> 

```tsx
class Calendar {
  private month: string;

  public constructor(Month: string) {
    this.month = month;
  }

  private getMonth(): string {
    return this.month;
  }
}

const calendar  = new Calendar("May");
console.log(calendar.getMonth()); // error: Property 'getMonth' is private and only accessible within class 'Calendar'.
```

<br><br>

## 12.3 readonly 읽기 전용

`readonly`는 프로퍼티 이름 앞에 표기함으로써  **값을 변경하지 못하도록 읽기 전용으로 설정**해주는 기능으로 `interface`와 `type`의 정의에서 사용할 수 있다.  

이와 같은 기능은 함수의 매개변수가 전달받은 인자 값이 변경되면 안 되는 경우 적합하게 사용할 수 있으며, **의도치 않은 값의 변경으로 인해 생기는 오류를 방지**한다.

<br>

> 예제 12-3
> 

```tsx
class Calendar {
  private readonly month: string;

  public constructor(month: string) {
    this.month = month;
  }

  private getMonth(): string {
    return this.month;
  }
}

const calendar  = new Calendar("May");
console.log(calendar.month); // error : Property 'month' is private and only accessible within class 'Calendar'.
calendar.month = 'Jun' // error : Cannot assign to 'month' because it is a read-only property.
```

<br><br>

## 12.4 extends 상속

extends는 상속받고자 하는 부모 클래스를 명시할 때 사용하는 키워드이다.

부모 클래스의 프로퍼티와 메소드를 그대로 받아오기 때문에 자식 클래스의 인스턴스에서 부모의 것을 자유롭게 사용 가능하다. (단, private는 사용 불가능.) 

<br>

> 예제 12-4
> 

```tsx
class User {
	public constructor(public name: string, public age: number) {}
    
	public getName(): string {
        return this.name;
    }
	public getAge(): number {
        return this.age;
    }
}

class newUser extends User {
    public constructor(name: string, age: number) {
    	super(name, age);
    }
}
let user1 = new newUser("영희", 20);
console.log(user1.getName()); // result: 영희
console.log(user1.getAge()); // result:20
```

<br><br>

## 12.5 interface

8장의 가명&인터페이스 파트에서 먼저 포함된 내용이며, 이곳에서 다시 한번 짚어보고자 한다.

- 우선 **interface**는 **type**과 비슷하게 새로운 타입을 정의하는 또 다른 방법이다. 객체, 함수, 함수의 파라미터, 클래스 등에 사용할 수 있으며 상호 간에 정의한 약속 혹은 규칙을 의미한다.
- **interface**는 **내용 구현 없이 이름과 타입만 정의한 추상화된 프로퍼티나 메서드만** 가질 수 있다.
- 클래스는 `**implements**` 키워드를 통해 미리 추상화된 `**interface**`를 채택하여 사용할 수 있다.
- **implements**는 상속(extends)과 다르게 추상화된 프로퍼티나 메서드의 **모양만** 받아오는 것이기 때문에 클래스 내에서 구현(**오버라이드)**해서 사용해야 한다.

<br>

<aside>
💡 여기서의 핵심 포인트!!
**interface**로 정의한 프로퍼티나 메서드는 **해당 클래스**에 필수적으로 들어가야 하며, **오버라이드**해서 내용을 구현해야한다.
</aside>

<br>

> 예제 12-5
> 

```tsx
interface User {
    name: string;
    age: number;
    getAge(age: number): void;
}

class newUser implements User {
    name: string = "철수";
    age: number = 20;
    getAge(age: number) {
        console.log(this.age);
    }
    constructor() {}
}
let user1 = new newUser();
user1; // result: {name: "철수", age: 20}
```

<br>

> 예제 12-6
> 

```tsx
interface User {
    name: string;
    age: number;
    getAge(age: number): void;
}

class newUser implements User {
    constructor(public name: string, public age: number) {}
    getAge(age: number) {
        console.log(this.age);
    }
}
let user1 = new newUser("영희", 20);
user1; // result: {name: "영희", age: 20}
```

<br>

**다중 상속**이 가능하므로 **interface**가 여러 개일 경우 예시와 같이 **쉼표(,)**로 나열할 수 있다. 

ex) `class shape implements Circle, Triangle, Square`

<br><br>

## 12.6 추상 클래스

추상 클래스란 **일반 메서드나 추상 메서드를 포함**할 수 있는 클래스를 뜻하며, `class` 앞에 `abstract`라고 표기하여 정의한다.

추상 메서드란 구현된 내용없이 이름과 타입만 선언된 메서드를 뜻하며,  메소드명 앞에 ****`abstract`라고 표기한다.

추상 클래스는 **상속용**으로만 가능하기 때문에 일반 클래스와 달리 **객체 인스턴스 생성이 불가능**하며, **상속받은 클래스에서는 해당 추상 메서드를 반드시 구현(오버라이드)**해야한다.

<br>

> 예제 12-7
> 

```tsx
abstract class Circle {
  public abstract getArea(): number;
  
  public printArea(): string {
  	return `${this.getArea()}`;
  }
}

class ShapeArea extends Circle {
  public constructor(protected readonly radius: number) {
    super();
  }
  
  public getArea(): number {
  	return Math.PI * this.radius * this.radius;
  }
}

const myCircle = new ShapeArea(10);

console.log(myCircle.printArea());  // result: 314.1592653589793
```

<br>

### 12.5.1 interface vs 추상 클래스

둘 다 추상적인 메서드를 정의한다는 공통점이 있다. 하지만 interface는 일반 메서드를 포함 할 수 없고, 추상 클래스는 일반 메서드를 포함 할 수 있다는 점에서 차이를 갖는다.
|  |  interface | 추상 클래스 |
| :---: | :---: | :---: |
| 추상 메서드 | O | O |
| 일반 메서드 | X | O |