# 📚 나의 질문과 그에 대한 ChatGPT의 답변

## 🚀 **1. JavaScript의 특징과 유의사항**

### 📝 **Q: JavaScript의 특징에 대해 설명해줘. 숙련된 JavaScript/TypeScript 개발자가 유의해야 할 사항은? 성능적으로 신경 써야 하는 부분이 있어? JavaScript만의 독특한 메서드나 함수는?**

**A:**

- **JavaScript의 주요 특징:**

  - 동적 타입 언어
  - 비동기 처리 (Event Loop, Call Stack, Web API, Task Queue)
  - 객체 기반 언어
  - 클라이언트와 서버 양쪽에서 실행 가능

- **숙련된 개발자가 유의해야 할 사항:**

  - `this`의 동작 원리
  - 클로저로 인한 메모리 누수 방지
  - 동기 vs 비동기 코드 이해
  - `call`, `apply`, `bind`의 차이점

- **성능 최적화 포인트:**

  - 이벤트 리스너의 과도한 사용 방지
  - `for` vs `forEach` 성능 비교
  - 메모리 누수 방지 (클로저, 전역 변수 남용 주의)

- **독특한 메서드/함수:**
  - `Object.freeze`, `Object.seal`
  - `WeakMap`, `WeakSet`
  - `Array.prototype.map`, `filter`, `reduce`

---

## 🧠 **2. WeakMap과 WeakSet**

### 📝 **Q: WeakMap과 WeakSet은 무엇이고, 어떤 용도로 사용되나? 약한 참조가 무슨 의미야? Map과 WeakMap의 차이는?**

**A:**

- **WeakMap:** 키로 객체만 사용할 수 있으며, 객체에 대한 약한 참조를 유지해 가비지 컬렉션의 대상이 될 수 있음.
- **WeakSet:** 객체만 저장 가능하며, 객체가 더 이상 접근할 수 없을 때 메모리에서 해제됨.

- **약한 참조란?**

  - 객체가 강한 참조 없이 약한 참조만 유지되면 가비지 컬렉터가 메모리에서 해당 객체를 해제할 수 있음.

- **Map vs WeakMap 비교:**
  | **Map** | **WeakMap** |
  |---------|-----------|
  | 모든 값이 키로 사용 가능 | 객체만 키로 사용 가능 |
  | 강한 참조 | 약한 참조 |
  | 반복 가능 (`keys`, `values`, `entries`) | 반복 불가 |
  | 명시적 메모리 해제 필요 | 자동 메모리 해제 |

**사용 사례:**

- **Map:** 일반적인 데이터 저장
- **WeakMap:** DOM 요소, 임시 객체 캐싱

---

## 🔗 **3. 클로저(Closure)**

### 📝 **Q: 클로저란 무엇이며, 데이터 은닉에 어떻게 사용되나? 클로저로 인해 발생할 수 있는 메모리 누수의 예시는?**

**A:**

- **클로저란?**

  - 함수가 자신이 생성될 때의 렉시컬 환경(Lexical Environment)을 기억하는 특성.
  - 외부 함수의 변수에 접근할 수 있음.

- **데이터 은닉:**
  - 외부에서 접근할 수 없는 변수를 만들어 민감한 데이터를 보호.

```javascript
function createCounter() {
  let count = 0;
  return {
    increment: () => count++,
    getCount: () => count,
  };
}
```

- **메모리 누수 예시:**

```javascript
function init() {
  let largeArray = new Array(1000000);
  return function () {
    console.log(largeArray.length);
  };
}
let leak = init();
```

- `largeArray`는 `leak`을 통해 계속 참조되기 때문에 메모리 해제가 안 됨.

**해결법:**

- 필요하지 않은 참조는 `null`로 설정.

---

## ⚙️ **4. this와 관련된 메서드 (call, apply, bind)**

### 📝 **Q: this와 call, apply, bind의 차이점은? 예시를 들어 설명해줘.**

**A:**

- `this`: 실행 컨텍스트에 따라 달라짐 (전역, 함수, 객체, 클래스 등)
- `call`: 함수 호출과 동시에 `this`를 명시적으로 설정
- `apply`: `call`과 비슷하지만, 인수를 배열로 받음
- `bind`: `this`를 고정하고 새로운 함수를 반환

**예시:**

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const user = { name: "Alice" };

greet.call(user, "Hello"); // Hello, Alice
greet.apply(user, ["Hi"]); // Hi, Alice
const boundGreet = greet.bind(user);
boundGreet("Hey"); // Hey, Alice
```

---

## 🎯 **5. 이벤트 리스너 최적화**

### 📝 **Q: 이벤트 리스너 최적화 방법에 대해 설명해줘.**

**A:**

1. **이벤트 위임 (Event Delegation)**
   - 많은 자식 요소에 이벤트 리스너를 개별로 추가하지 않고, 상위 요소에 추가.
2. **이벤트 리스너 제거**
   - 불필요한 리스너는 `removeEventListener`로 제거.
3. **스로틀링(Throttling)과 디바운싱(Debouncing)**
   - **Throttling:** 일정 시간 간격으로 함수 실행
   - **Debouncing:** 마지막 이벤트가 발생한 후 일정 시간 후 실행
4. **패시브 이벤트 리스너 (Passive Event Listener)**
   - 스크롤 이벤트 최적화에 사용

```javascript
document.addEventListener("scroll", handleScroll, { passive: true });
```

---

## 📊 **6. forEach vs for 성능 비교**

### 📝 **Q: forEach보다 for가 빠를 때는 언제야?**

**A:**

- `for` 루프는 반복 중 언제든 `break`로 중단할 수 있어 성능적으로 유리.
- `forEach`는 반복을 중간에 중단할 수 없음.
- 반복 횟수가 매우 많을 때 `for`가 더 빠를 수 있음.

**예시:**

```javascript
let arr = Array(100000).fill(0);

console.time("for");
for (let i = 0; i < arr.length; i++) {}
console.timeEnd("for");

console.time("forEach");
arr.forEach(() => {});
console.timeEnd("forEach");
```

---
