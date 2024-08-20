# javascript를 위한 레포

## javascript 코드가 실행되는 과정

### 1. **JavaScript 엔진**

JavaScript는 브라우저나 Node.js 환경에서 실행되며, 이때 JavaScript 코드를 해석하고 실행하는 프로그램이 **JavaScript 엔진**입니다. 가장 유명한 JavaScript 엔진은 Google의 **V8 엔진**입니다.

JavaScript 엔진은 주로 다음과 같은 단계로 작동합니다:

- **파싱(Parsing)**: JavaScript 코드는 먼저 파서(parser)에 의해 파싱됩니다. 여기서 코드가 구문 트리(Syntax Tree)로 변환됩니다.
- **컴파일(Compilation)**: V8과 같은 최신 JavaScript 엔진은 파싱된 코드를 바이트코드로 컴파일하고, 최적화된 기계어로 변환합니다.
- **실행(Execution)**: 컴파일된 코드가 실행되면서, JavaScript의 각 문장(statement)들이 순차적으로 실행됩니다.

### 2. **메모리 관리: 스택(Stack)과 힙(Heap)**

JavaScript는 메모리를 자동으로 관리하는 **가비지 컬렉션**(Garbage Collection) 기능을 가지고 있습니다. 메모리는 주로 두 가지 주요 영역인 **스택(Stack)**과 **힙(Heap)**으로 나누어 관리됩니다.

#### **스택(Stack)**

스택은 **고정 크기**의 메모리 영역으로, 주로 **함수 호출, 기본 데이터 타입(숫자, 문자열 등)의 메모리 할당**에 사용됩니다. 스택은 LIFO(Last In, First Out) 방식으로 작동합니다.

- **실행 컨텍스트**: JavaScript에서 함수가 호출될 때마다 새로운 실행 컨텍스트(Execution Context)가 생성되고, 스택에 푸시됩니다. 이 컨텍스트는 해당 함수의 변수, 스코프, `this` 등의 정보를 포함합니다.
- **콜 스택(Call Stack)**: 자바스크립트 엔진은 실행 중에 콜 스택(Call Stack)을 사용해 함수 호출을 관리합니다. 함수가 호출되면 콜 스택에 쌓이고, 함수 실행이 끝나면 콜 스택에서 제거됩니다.

#### **힙(Heap)**

힙은 **동적 메모리 할당**이 이루어지는 영역입니다. 힙은 크기가 고정되어 있지 않으며, **객체, 배열, 함수** 등 크기가 가변적인 데이터가 저장됩니다.

- **객체와 배열**: JavaScript에서 객체와 배열은 크기가 동적이기 때문에 힙에 할당됩니다. 힙에 할당된 메모리는 자바스크립트 엔진의 가비지 컬렉터(Garbage Collector)에 의해 관리되며, 더 이상 참조되지 않는 객체는 자동으로 해제됩니다.

### 3. **커널과 메모리 관리**

JavaScript 엔진은 운영체제(OS)의 커널(Kernel)을 통해 메모리와 CPU 리소스를 관리합니다. 커널은 다음과 같은 역할을 합니다:

- **메모리 할당**: 커널은 자바스크립트 엔진이 요청한 메모리를 할당하고, 스택과 힙 메모리 영역을 관리합니다.
- **프로세스와 스레드 관리**: 자바스크립트 엔진은 단일 스레드에서 실행되며, 커널은 이 스레드의 실행을 관리합니다. 또한, 자바스크립트 엔진에서 비동기 작업(I/O 작업, 네트워크 요청 등)이 발생하면, 커널은 이를 처리하고 이벤트 루프와 협력해 작업을 완료합니다.

### 4. **콜 스택(Call Stack)과 이벤트 루프(Event Loop)**

JavaScript는 단일 스레드로 작동하기 때문에 한 번에 하나의 작업만 실행할 수 있습니다. 그러나 비동기 작업을 효과적으로 처리하기 위해 **콜 스택**과 **이벤트 루프**가 협력합니다.

- **콜 스택(Call Stack)**: JavaScript 엔진이 함수 호출을 관리하는 구조입니다. 각 함수 호출은 콜 스택에 추가되고, 함수 실행이 끝나면 스택에서 제거됩니다.
- **이벤트 루프(Event Loop)**: 비동기 작업(예: 타이머, 네트워크 요청)이 완료되면, 그 콜백 함수는 이벤트 큐(Event Queue)에 추가됩니다. 이벤트 루프는 콜 스택이 비어 있을 때 이벤트 큐에서 대기 중인 콜백을 꺼내어 실행합니다.

### **예시로 보는 JavaScript 실행 흐름**

```javascript
function first() {
  console.log("First function starts");
  second();
  console.log("First function ends");
}

function second() {
  console.log("Second function starts");
  setTimeout(() => {
    console.log("Asynchronous task completed");
  }, 1000);
  console.log("Second function ends");
}

first();
```

#### **실행 흐름**:

1. **`first()` 함수 호출**:

   - `first()` 함수가 콜 스택에 푸시됩니다.
   - `console.log("First function starts")` 실행.

2. **`second()` 함수 호출**:

   - `second()` 함수가 콜 스택에 푸시됩니다.
   - `console.log("Second function starts")` 실행.

3. **비동기 작업 (setTimeout)**:

   - `setTimeout` 함수 호출이 콜 스택에 푸시됩니다.
   - `setTimeout`이 비동기 작업을 이벤트 루프에 전달하고, 콜 스택에서 제거됩니다.
   - `console.log("Second function ends")` 실행 후 `second()` 함수가 콜 스택에서 제거됩니다.

4. **`first()` 함수 종료**:

   - `console.log("First function ends")` 실행 후, `first()` 함수가 콜 스택에서 제거됩니다.
   - 이 시점에서 콜 스택은 비어 있습니다.

5. **이벤트 루프 처리**:
   - 1초 후, `setTimeout`의 콜백 함수가 이벤트 큐에서 꺼내져 콜 스택으로 이동하여 실행됩니다.
   - `console.log("Asynchronous task completed")`이 실행됩니다.

### **정리**

- **메모리 구조**: JavaScript는 메모리를 스택과 힙으로 나누어 관리합니다. 스택은 함수 호출과 관련된 실행 컨텍스트를 관리하고, 힙은 동적 데이터(객체, 배열 등)를 관리합니다.
- **단일 스레드와 비동기성**: JavaScript는 단일 스레드에서 실행되지만, 비동기 작업을 통해 여러 작업을 동시 처리하는 것처럼 보이게 합니다. 이벤트 루프는 이 비동기 작업의 완료를 관리하며, 콜 스택이 비어 있을 때 콜백을 실행합니다.
- **커널의 역할**: JavaScript 엔진은 OS의 커널을 통해 메모리와 프로세스를 관리하며, I/O 작업과 같은 비동기 작업이 처리되는 동안 이벤트 루프와 협력합니다.

이와 같은 구조 덕분에 JavaScript는 비동기 프로그래밍을 효율적으로 수행할 수 있습니다.
