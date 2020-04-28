# 栈

栈是一种遵从 **后进先出**（LIFO）原则的有序集合。新添加或待删除的元素都保存在栈的同一端，称作栈顶，另一端就叫栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底。

## 实现

```typescript
class Stack<T> {
  #stack: T[];
  constructor(initStack = []) {
    this.#stack = initStack;
  }

  // push element(s) to stack
  push(...elems: T[]): void {
    this.#stack.push(...elems);
  }

  // pop element(s) to stack
  pop(l = 1): T[] | undefined {
    let _l = l;
    let popElems: T[];

    if (l > this.size()) {
      throw new RangeError(
        "The number of elements that need to be popped must be less than the size of stack."
      );
    }
    if (l < 1) {
      throw new RangeError(
        "The number of elements that need to be popped must be greater than 1."
      );
    }
    popElems = [];
    while (_l--) {
      popElems.push(this.#stack.pop() as T);
    }
    return popElems;
  }

  // find the element at the top of stack
  peek(): T {
    if (this.isEmpty()) {
      throw new Error("the stack have no element.");
    }
    return this.#stack[this.size() - 1];
  }

  // check if the stack is empty
  isEmpty(): boolean {
    return this.size() === 0;
  }

  // clear all the elements in stack
  clear(): void {
    this.#stack = [];
  }

  // get the length of stack
  size(): number {
    return this.#stack.length;
  }
}
```

## 实战

### 进制转换

```typescript
function baseConverter(decNumber: number): string {
  const remStack = new Stack();
  const digits = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"; // {6}
  let number = decNumber;
  let rem;
  let baseString = "";

  if (!(base >= 2 && base <= 36)) {
    return "";
  }

  while (number > 0) {
    rem = Math.floor(number % base);
    remStack.push(rem);
    number = Math.floor(number / base);
  }

  while (!remStack.isEmpty()) {
    baseString += digits[remStack.pop()]; // {7}
  }

  return baseString;
}
```
