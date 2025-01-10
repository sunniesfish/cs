# 디자인 패턴: 생성, 구조, 행위 패턴 (JavaScript 예제 포함)

## 1. 생성 패턴 (Creational Patterns)

### 1.1 Factory Method (팩토리 메서드)

**사용 이유:**

- 객체 생성 과정을 캡슐화하여 클라이언트 코드가 구체적인 클래스에 의존하지 않도록 합니다.

**장단점:**

- **장점:**
  - 객체 생성 로직을 분리하여 코드 재사용성 증가.
  - 새로운 클래스 추가 시 기존 코드를 수정하지 않아도 됨.
- **단점:**
  - 클래스가 많아질 수 있어 관리가 어려워질 수 있음.

**사용 상황:**

- 객체 생성이 복잡하거나 객체 유형이 동적으로 결정되는 경우.
- 예: 다양한 결제 수단(PayPal, CreditCard)을 처리하는 시스템.

```javascript
class Payment {
  createPaymentMethod() {
    throw new Error("Method not implemented");
  }
}

class PayPalPayment extends Payment {
  createPaymentMethod() {
    return "PayPal Payment Method";
  }
}

class CreditCardPayment extends Payment {
  createPaymentMethod() {
    return "Credit Card Payment Method";
  }
}

function paymentFactory(type) {
  if (type === "paypal") {
    return new PayPalPayment();
  } else if (type === "creditcard") {
    return new CreditCardPayment();
  }
  throw new Error("Invalid payment type");
}

const payment = paymentFactory("paypal");
console.log(payment.createPaymentMethod()); // PayPal Payment Method
```

---

### 1.2 Abstract Factory (추상 팩토리)

**사용 이유:**

- 관련된 객체 군을 생성하는 인터페이스를 제공하며, 구체적인 클래스에 의존하지 않음.

**장단점:**

- **장점:**
  - 다양한 객체를 생성하는데 일관성을 유지.
  - 코드 변경 없이 다양한 객체 군을 쉽게 추가.
- **단점:**
  - 구조가 복잡해질 수 있음.

**사용 상황:**

- 플랫폼에 따라 다른 객체를 생성해야 하는 경우.
- 예: Windows와 macOS에서 서로 다른 UI 컴포넌트를 생성.

```javascript
class MacButton {
  render() {
    console.log("Rendering Mac Button");
  }
}

class WinButton {
  render() {
    console.log("Rendering Windows Button");
  }
}

class MacFactory {
  createButton() {
    return new MacButton();
  }
}

class WinFactory {
  createButton() {
    return new WinButton();
  }
}

function createUI(factory) {
  const button = factory.createButton();
  button.render();
}

const macFactory = new MacFactory();
createUI(macFactory); // Rendering Mac Button

const winFactory = new WinFactory();
createUI(winFactory); // Rendering Windows Button
```

---

### 1.3 Builder (빌더)

**사용 이유:**

- 복잡한 객체를 단계별로 구성하며, 생성 과정과 표현을 분리.

**장단점:**

- **장점:**
  - 객체 생성 코드와 표현 코드의 분리.
  - 동일한 생성 과정으로 다양한 표현 가능.
- **단점:**
  - 빌더 클래스가 늘어나면 관리가 어려움.

**사용 상황:**

- 복잡한 객체를 생성해야 하는 경우.
- 예: HTML 문서 생성기.

```javascript
class DocumentBuilder {
  constructor() {
    this.document = "";
  }

  addTitle(title) {
    this.document += `<h1>${title}</h1>\n`;
    return this;
  }

  addParagraph(paragraph) {
    this.document += `<p>${paragraph}</p>\n`;
    return this;
  }

  build() {
    return this.document;
  }
}

const builder = new DocumentBuilder();
const document = builder
  .addTitle("Hello World")
  .addParagraph("This is a paragraph.")
  .build();

console.log(document);
// <h1>Hello World</h1>
// <p>This is a paragraph.</p>
```

---

### 1.4 Prototype (프로토타입)

**사용 이유:**

- 기존 객체를 복제하여 새로운 객체를 생성.

**장단점:**

- **장점:**
  - 객체 생성 비용을 줄임.
  - 복잡한 객체를 쉽게 생성 가능.
- **단점:**
  - 깊은 복사와 얕은 복사를 구분해야 함.

**사용 상황:**

- 초기화 비용이 높은 객체를 생성해야 하는 경우.
- 예: 게임 캐릭터의 초기 상태 복제.

```javascript
const prototype = {
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  },
};

const obj1 = Object.create(prototype);
obj1.name = "John";
obj1.greet(); // Hello, my name is John

const obj2 = Object.create(prototype);
obj2.name = "Jane";
obj2.greet(); // Hello, my name is Jane
```

---

### 1.5 Singleton (싱글톤)

**사용 이유:**

- 클래스의 인스턴스가 하나만 존재하도록 보장.

**장단점:**

- **장점:**
  - 전역 상태를 관리하기 쉬움.
- **단점:**
  - 전역 상태를 남용하면 코드가 복잡해질 수 있음.

**사용 상황:**

- 설정 관리, 데이터베이스 연결 등 전역적으로 하나의 인스턴스만 필요한 경우.

```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    Singleton.instance = this;
    this.data = "Singleton Data";
  }
}

const instance1 = new Singleton();
const instance2 = new Singleton();

console.log(instance1 === instance2); // true
```

---

## 2. 구조 패턴 (Structural Patterns)

### 2.1 Adapter (어댑터)

**사용 이유:**

- 인터페이스가 서로 다른 객체를 연결.

**장단점:**

- **장점:**
  - 기존 코드를 수정하지 않고 재사용 가능.
- **단점:**
  - 코드 복잡도가 증가.

**사용 상황:**

- 새로운 인터페이스를 기존 시스템과 통합해야 할 때.
- 예: 구식 API와 현대적 API 통합.

```javascript
class OldSystem {
  oldMethod() {
    return "Old Method";
  }
}

class NewSystem {
  newMethod() {
    return "New Method";
  }
}

class Adapter {
  constructor(oldSystem) {
    this.oldSystem = oldSystem;
  }

  newMethod() {
    return this.oldSystem.oldMethod();
  }
}

const oldSystem = new OldSystem();
const adapter = new Adapter(oldSystem);

console.log(adapter.newMethod()); // Old Method
```

---

### 2.2 Bridge (브릿지)

**사용 이유:**

- 추상화와 구현을 분리하여 독립적으로 변화할 수 있게 함.

**장단점:**

- **장점:**
  - 추상화와 구현 간의 결합도를 낮춤.
  - 확장성과 유지보수성 향상.
- **단점:**
  - 구조가 복잡해질 수 있음.

**사용 상황:**

- 플랫폼 간의 차이를 추상화해야 하는 경우.
- 예: 여러 플랫폼에서 작동하는 장치 관리 시스템.

```javascript
class Device {
  turnOn() {
    throw new Error("Method not implemented");
  }

  turnOff() {
    throw new Error("Method not implemented");
  }
}

class TV extends Device {
  turnOn() {
    console.log("TV is ON");
  }

  turnOff() {
    console.log("TV is OFF");
  }
}

class RemoteControl {
  constructor(device) {
    this.device = device;
  }

  togglePower() {
    console.log("Toggling power");
    this.device.turnOn();
    this.device.turnOff();
  }
}

const tv = new TV();
const remote = new RemoteControl(tv);
remote.togglePower();
// Toggling power
// TV is ON
// TV is OFF
```

---

## 2.3 Composite (컴포지트)

**사용 이유:**

- 객체를 트리 구조로 구성하여 부분-전체 관계를 표현.

**장단점:**

- **장점:**
  - 클라이언트가 단일 객체와 복합 객체를 동일하게 처리 가능.
- **단점:**
  - 트리 구조가 복잡할 경우 성능 저하.

**사용 상황:**

- 계층적 구조(예: 파일 시스템, GUI 구성 요소).

```javascript
class Component {
  operation() {
    throw new Error("Method not implemented");
  }
}

class Leaf extends Component {
  constructor(name) {
    super();
    this.name = name;
  }

  operation() {
    console.log(`Leaf: ${this.name}`);
  }
}

class Composite extends Component {
  constructor(name) {
    super();
    this.name = name;
    this.children = [];
  }

  add(component) {
    this.children.push(component);
  }

  operation() {
    console.log(`Composite: ${this.name}`);
    this.children.forEach((child) => child.operation());
  }
}

const leaf1 = new Leaf("Leaf 1");
const leaf2 = new Leaf("Leaf 2");
const composite = new Composite("Composite 1");
composite.add(leaf1);
composite.add(leaf2);

composite.operation();
// Composite: Composite 1
// Leaf: Leaf 1
// Leaf: Leaf 2
```

---

### 2.4 Decorator (데코레이터)

**사용 이유:**

- 객체에 동적으로 새로운 행동을 추가.

**장단점:**

- **장점:**
  - 상속 없이 객체의 기능 확장.
- **단점:**
  - 많은 데코레이터가 사용되면 코드 가독성이 떨어질 수 있음.

**사용 상황:**

- 동적으로 기능을 추가하거나 제거해야 하는 경우.
- 예: 문서 편집기의 텍스트 스타일링.

```javascript
class Text {
  render() {
    return "Plain Text";
  }
}

class BoldDecorator {
  constructor(component) {
    this.component = component;
  }

  render() {
    return `<b>${this.component.render()}</b>`;
  }
}

class ItalicDecorator {
  constructor(component) {
    this.component = component;
  }

  render() {
    return `<i>${this.component.render()}</i>`;
  }
}

const plainText = new Text();
const boldText = new BoldDecorator(plainText);
const italicBoldText = new ItalicDecorator(boldText);

console.log(italicBoldText.render()); // <i><b>Plain Text</b></i>
```

---

### 2.5 Facade (퍼사드)

**사용 이유:**

- 복잡한 시스템에 단순화된 인터페이스 제공.

**장단점:**

- **장점:**
  - 복잡한 하위 시스템을 쉽게 사용할 수 있음.
- **단점:**
  - 퍼사드에 지나치게 의존하면 유연성이 줄어들 수 있음.

**사용 상황:**

- 복잡한 시스템을 단순하게 사용하고자 할 때.
- 예: 외부 API 호출 간소화.

```javascript
class SubsystemA {
  operationA() {
    return "SubsystemA: OperationA";
  }
}

class SubsystemB {
  operationB() {
    return "SubsystemB: OperationB";
  }
}

class Facade {
  constructor() {
    this.subsystemA = new SubsystemA();
    this.subsystemB = new SubsystemB();
  }

  operation() {
    return `${this.subsystemA.operationA()}\n${this.subsystemB.operationB()}`;
  }
}

const facade = new Facade();
console.log(facade.operation());
// SubsystemA: OperationA
// SubsystemB: OperationB
```

---

### 2.6 Flyweight (플라이웨이트)

**사용 이유:**

- 메모리 사용량을 줄이기 위해 공유 가능한 객체를 활용.

**장단점:**

- **장점:**
  - 메모리 사용량 감소.
- **단점:**
  - 코드 복잡도 증가.

**사용 상황:**

- 다수의 객체가 동일한 데이터를 공유할 수 있을 때.
- 예: 그래픽 에디터의 도형 관리.

```javascript
class Flyweight {
  constructor(sharedState) {
    this.sharedState = sharedState;
  }

  operation(uniqueState) {
    console.log(`Shared: ${this.sharedState}, Unique: ${uniqueState}`);
  }
}

class FlyweightFactory {
  constructor() {
    this.flyweights = {};
  }

  getFlyweight(sharedState) {
    if (!this.flyweights[sharedState]) {
      this.flyweights[sharedState] = new Flyweight(sharedState);
    }
    return this.flyweights[sharedState];
  }
}

const factory = new FlyweightFactory();
const flyweight1 = factory.getFlyweight("State1");
const flyweight2 = factory.getFlyweight("State1");

flyweight1.operation("Unique1");
flyweight2.operation("Unique2");
console.log(flyweight1 === flyweight2); // true
```

---

### 2.7 Proxy (프록시)

**사용 이유:**

- 다른 객체에 대한 접근을 제어.

**장단점:**

- **장점:**
  - 접근 제어, 로깅, 캐싱 등의 기능 제공.
- **단점:**
  - 추가적인 클래스가 필요.

**사용 상황:**

- 접근 제어나 캐싱이 필요한 경우.
- 예: 가상 프록시를 이용한 이미지 로딩.

```javascript
class RealSubject {
  request() {
    console.log("RealSubject: Handling request");
  }
}

class Proxy {
  constructor(realSubject) {
    this.realSubject = realSubject;
  }

  request() {
    console.log("Proxy: Checking access");
    this.realSubject.request();
  }
}

const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);
proxy.request();
// Proxy: Checking access
// RealSubject: Handling request
```

---

### 3. 행위 패턴

행위 패턴은 객체들 간의 상호작용과 책임 분배를 다룹니다.

---

### 3.1 Chain of Responsibility (책임 연쇄)

**사용 이유:**

- 요청을 처리할 수 있는 객체를 동적으로 결정.

**장단점:**

- **장점:**
  - 요청을 처리할 객체를 동적으로 변경 가능.
  - 요청 처리 객체 간의 결합도를 줄임.
- **단점:**
  - 디버깅이 어려울 수 있음.

**사용 상황:**

- 여러 객체 중 하나가 요청을 처리할 때.
- 예: 이벤트 처리 체인.

```javascript
class Handler {
  setNext(handler) {
    this.nextHandler = handler;
    return handler;
  }

  handle(request) {
    if (this.nextHandler) {
      return this.nextHandler.handle(request);
    }
    return null;
  }
}

class ConcreteHandlerA extends Handler {
  handle(request) {
    if (request === "A") {
      return `HandlerA: Handled ${request}`;
    }
    return super.handle(request);
  }
}

class ConcreteHandlerB extends Handler {
  handle(request) {
    if (request === "B") {
      return `HandlerB: Handled ${request}`;
    }
    return super.handle(request);
  }
}

const handlerA = new ConcreteHandlerA();
const handlerB = new ConcreteHandlerB();
handlerA.setNext(handlerB);

console.log(handlerA.handle("A")); // HandlerA: Handled A
console.log(handlerA.handle("B")); // HandlerB: Handled B
console.log(handlerA.handle("C")); // null
```

---

### 3.2 Command (커맨드)

**사용 이유:**

- 요청을 객체로 캡슐화하여 다양한 요청, 대기열 또는 로깅을 지원.

**장단점:**

- **장점:**
  - 요청과 실행 로직을 분리.
  - 요청의 로깅 및 취소 가능.
- **단점:**
  - 클래스 수 증가.

**사용 상황:**

- 요청을 파라미터화하거나 대기열/취소가 필요할 때.
- 예: 버튼 클릭 이벤트 처리.

```javascript
class Command {
  execute() {
    throw new Error("Method not implemented");
  }
}

class Light {
  turnOn() {
    console.log("Light is On");
  }

  turnOff() {
    console.log("Light is Off");
  }
}

class LightOnCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.turnOn();
  }
}

class LightOffCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.turnOff();
  }
}

class RemoteControl {
  setCommand(command) {
    this.command = command;
  }

  pressButton() {
    this.command.execute();
  }
}

const light = new Light();
const lightOn = new LightOnCommand(light);
const lightOff = new LightOffCommand(light);
const remote = new RemoteControl();

remote.setCommand(lightOn);
remote.pressButton(); // Light is On

remote.setCommand(lightOff);
remote.pressButton(); // Light is Off
```

---

### 3.3 Interpreter (인터프리터)

**사용 이유:**

- 언어의 문법을 정의하고 해석.

**장단점:**

- **장점:**
  - 간단한 문법을 쉽게 구현 가능.
- **단점:**
  - 복잡한 문법을 처리하기에는 비효율적.

**사용 상황:**

- 간단한 스크립팅 언어나 명령어 해석.

```javascript
class Context {
  constructor(input) {
    this.input = input;
  }
}

class Expression {
  interpret(context) {
    throw new Error("Method not implemented");
  }
}

class NumberExpression extends Expression {
  interpret(context) {
    return parseInt(context.input, 10);
  }
}

class AddExpression extends Expression {
  constructor(left, right) {
    super();
    this.left = left;
    this.right = right;
  }

  interpret(context) {
    return this.left.interpret(context) + this.right.interpret(context);
  }
}

const context = new Context("");
const left = new NumberExpression();
const right = new NumberExpression();
context.input = "3";
const leftValue = left.interpret(context);
context.input = "4";
const rightValue = right.interpret(context);
const addExpression = new AddExpression(left, right);

console.log(addExpression.interpret(context)); // 7
```

---

### 3.4 Iterator (이터레이터)

**사용 이유:**

- 컬렉션의 요소를 순차적으로 접근.

**장단점:**

- **장점:**
  - 컬렉션의 내부 표현을 노출하지 않음.
- **단점:**
  - 여러 이터레이터가 동시에 사용되면 복잡도가 증가.

**사용 상황:**

- 리스트, 배열, 기타 컬렉션의 요소를 순차적으로 접근해야 할 때.

```javascript
class Iterator {
  constructor(collection) {
    this.collection = collection;
    this.index = 0;
  }

  hasNext() {
    return this.index < this.collection.length;
  }

  next() {
    return this.collection[this.index++];
  }
}

const numbers = [1, 2, 3, 4, 5];
const iterator = new Iterator(numbers);

while (iterator.hasNext()) {
  console.log(iterator.next());
}
```

---

### 3.5 Mediator (미디에이터)

**사용 이유:**

- 객체 간의 상호작용을 중앙 집중화.

**장단점:**

- **장점:**
  - 객체 간의 의존성 감소.
- **단점:**
  - Mediator 클래스가 복잡해질 수 있음.

**사용 상황:**

- 여러 객체가 서로 직접 참조 없이 협력해야 할 때.
- 예: 채팅 애플리케이션의 메시지 라우팅.

```javascript
class Mediator {
  notify(sender, event) {
    throw new Error("Method not implemented");
  }
}

class ConcreteMediator extends Mediator {
  constructor(component1, component2) {
    super();
    this.component1 = component1;
    this.component2 = component2;

    this.component1.setMediator(this);
    this.component2.setMediator(this);
  }

  notify(sender, event) {
    if (event === "EventA") {
      console.log("Mediator reacts on EventA and triggers Component2");
      this.component2.doSomething();
    }

    if (event === "EventB") {
      console.log("Mediator reacts on EventB and triggers Component1");
      this.component1.doSomething();
    }
  }
}

class BaseComponent {
  setMediator(mediator) {
    this.mediator = mediator;
  }
}

class Component1 extends BaseComponent {
  doSomething() {
    console.log("Component1 does something.");
    this.mediator.notify(this, "EventA");
  }
}

class Component2 extends BaseComponent {
  doSomething() {
    console.log("Component2 does something.");
    this.mediator.notify(this, "EventB");
  }
}

const component1 = new Component1();
const component2 = new Component2();
const mediator = new ConcreteMediator(component1, component2);

component1.doSomething();
component2.doSomething();
```

---

---

### 3.6 Memento (메멘토)

**사용 이유:**

- 객체의 상태를 저장하고 복원.

**장단점:**

- **장점:**
  - 객체의 내부 상태를 캡슐화하면서 저장 가능.
  - 복잡한 객체의 상태를 쉽게 복원 가능.
- **단점:**
  - 메멘토 객체가 많아지면 메모리 사용량 증가.

**사용 상황:**

- 되돌리기 기능이 필요한 애플리케이션.
- 예: 텍스트 편집기의 Undo 기능.

```javascript
class Memento {
  constructor(state) {
    this.state = state;
  }

  getState() {
    return this.state;
  }
}

class Originator {
  setState(state) {
    this.state = state;
    console.log(`State set to: ${state}`);
  }

  save() {
    return new Memento(this.state);
  }

  restore(memento) {
    this.state = memento.getState();
    console.log(`State restored to: ${this.state}`);
  }
}

class Caretaker {
  constructor() {
    this.mementos = [];
  }

  addMemento(memento) {
    this.mementos.push(memento);
  }

  getMemento(index) {
    return this.mementos[index];
  }
}

const originator = new Originator();
const caretaker = new Caretaker();

originator.setState("State1");
caretaker.addMemento(originator.save());

originator.setState("State2");
caretaker.addMemento(originator.save());

originator.restore(caretaker.getMemento(0));
```

---

### 3.7 Observer (옵저버)

**사용 이유:**

- 객체 상태 변경 시 관련 객체들에게 알림을 전달.

**장단점:**

- **장점:**
  - 주체와 관찰자 간의 결합도를 줄임.
- **단점:**
  - 관찰자가 많아질수록 성능 저하 가능.

**사용 상황:**

- 이벤트 기반 시스템.
- 예: 웹 애플리케이션에서 데이터 바인딩.

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }

  attach(observer) {
    this.observers.push(observer);
  }

  detach(observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }

  notify() {
    this.observers.forEach((observer) => observer.update());
  }
}

class Observer {
  update() {
    throw new Error("Method not implemented");
  }
}

class ConcreteObserver extends Observer {
  constructor(name) {
    super();
    this.name = name;
  }

  update() {
    console.log(`${this.name} has been notified.`);
  }
}

const subject = new Subject();
const observer1 = new ConcreteObserver("Observer1");
const observer2 = new ConcreteObserver("Observer2");

subject.attach(observer1);
subject.attach(observer2);
subject.notify();
```

---

### 3.8 State (상태)

**사용 이유:**

- 객체 상태에 따라 다른 동작을 수행.

**장단점:**

- **장점:**
  - 상태 별로 동작을 캡슐화.
  - 상태 전환 로직을 간단히 유지.
- **단점:**
  - 상태가 많아질수록 클래스 수 증가.

**사용 상황:**

- 상태에 따라 행동이 달라지는 객체.
- 예: UI 컴포넌트 상태 관리.

```javascript
class State {
  handle(context) {
    throw new Error("Method not implemented");
  }
}

class ConcreteStateA extends State {
  handle(context) {
    console.log("State A handling request.");
    context.setState(new ConcreteStateB());
  }
}

class ConcreteStateB extends State {
  handle(context) {
    console.log("State B handling request.");
    context.setState(new ConcreteStateA());
  }
}

class Context {
  constructor() {
    this.state = new ConcreteStateA();
  }

  setState(state) {
    this.state = state;
  }

  request() {
    this.state.handle(this);
  }
}

const context = new Context();
context.request();
context.request();
```

---

### 3.9 Strategy (전략)

**사용 이유:**

- 알고리즘을 동적으로 선택 가능.

**장단점:**

- **장점:**
  - 런타임에 알고리즘 변경 가능.
  - 코드 중복 감소.
- **단점:**
  - 전략 클래스가 많아질 수 있음.

**사용 상황:**

- 다양한 알고리즘이 필요할 때.
- 예: 정렬 알고리즘 선택.

```javascript
class Strategy {
  execute(data) {
    throw new Error("Method not implemented");
  }
}

class ConcreteStrategyA extends Strategy {
  execute(data) {
    return data.sort((a, b) => a - b);
  }
}

class ConcreteStrategyB extends Strategy {
  execute(data) {
    return data.sort((a, b) => b - a);
  }
}

class Context {
  setStrategy(strategy) {
    this.strategy = strategy;
  }

  executeStrategy(data) {
    return this.strategy.execute(data);
  }
}

const context = new Context();
const data = [3, 1, 4, 1, 5];

context.setStrategy(new ConcreteStrategyA());
console.log(context.executeStrategy(data)); // [1, 1, 3, 4, 5]

context.setStrategy(new ConcreteStrategyB());
console.log(context.executeStrategy(data)); // [5, 4, 3, 1, 1]
```

---

### 3.10 Template Method (템플릿 메서드)

**사용 이유:**

- 알고리즘의 뼈대를 정의하고 일부 단계는 서브클래스에서 구현.

**장단점:**

- **장점:**
  - 코드 재사용성 증가.
- **단점:**
  - 서브클래스에 의존.

**사용 상황:**

- 알고리즘의 일부 단계가 고정적일 때.

```javascript
class AbstractClass {
  templateMethod() {
    this.stepOne();
    this.stepTwo();
    this.stepThree();
  }

  stepOne() {
    throw new Error("Method not implemented");
  }

  stepTwo() {
    throw new Error("Method not implemented");
  }

  stepThree() {
    console.log("Step Three is the same for all subclasses.");
  }
}

class ConcreteClass extends AbstractClass {
  stepOne() {
    console.log("Step One implemented by ConcreteClass.");
  }

  stepTwo() {
    console.log("Step Two implemented by ConcreteClass.");
  }
}

const instance = new ConcreteClass();
instance.templateMethod();
```

---

### 3.11 Visitor (비지터)

**사용 이유:**

- 객체 구조와 별개로 새로운 동작을 추가.

**장단점:**

- **장점:**
  - 새로운 동작을 쉽게 추가 가능.
- **단점:**
  - 객체 구조가 변경되면 Visitor도 수정 필요.

**사용 상황:**

- 객체 구조가 고정되어 있고 새로운 작업이 자주 추가될 때.

```javascript
class Visitor {
  visit(element) {
    throw new Error("Method not implemented");
  }
}

class ConcreteVisitor extends Visitor {
  visit(element) {
    console.log(`Visiting element: ${element.constructor.name}`);
  }
}

class Element {
  accept(visitor) {
    throw new Error("Method not implemented");
  }
}

class ConcreteElementA extends Element {
  accept(visitor) {
    visitor.visit(this);
  }
}

class ConcreteElementB extends Element {
  accept(visitor) {
    visitor.visit(this);
  }
}

const visitor = new ConcreteVisitor();
const elements = [new ConcreteElementA(), new ConcreteElementB()];

elements.forEach((element) => element.accept(visitor));
```

---
