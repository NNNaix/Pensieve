# 队列

队列是遵循 **先进先出**（FIFO，也称为先来先服务）原则的一组有序的项。队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。

## 实现

```typescript
class Queue<T> {
  #queue: T[];
  constructor(initQueue = []) {
    this.#queue = initQueue;
  }

  // add elements to the end of queue
  enqueue(...elems: T[]): void {
    this.#queue.push(...elems);
  }

  // remove elements from the start of queue
  dequeue(l: number): T[] | undefined {
    let _l = l;
    let popElems: T[];

    if (l > this.size()) {
      throw new RangeError(
        "The number of elements that need to be dequeued must be less than the size of queue."
      );
    }
    if (l < 1) {
      throw new RangeError(
        "The number of elements that need to be dequeued must be greater than 1."
      );
    }
    popElems = [];
    while (_l--) {
      popElems.push(this.#stack.shift() as T);
    }
    return popElems;
  }

  // find the fisrt element in the queue
  peek(): T {
    if (this.isEmpty()) {
      throw new Error("the queue have no element.");
    }
    return this.#stack[0];
  }

  // check if the stack is empty
  isEmpty(): boolean {
    return this.size() === 0;
  }

  // clear all the elements in stack
  clear(): void {
    this.#queue = [];
  }

  // get the length of stack
  size(): number {
    return this.#queue.length;
  }
}
```

## 变种实现：双端队列

**双端队列**（deque，或称 double-ended queue）是一种允许我们同时从前端和后端添加和移除元素的特殊队列。

```typescript
class DoubleEndedQueue<T> {
  #queue: T[];
  constructor(initQueue = []) {
    this.#queue = initQueue;
  }
  // add elements to the end of queue
  addFront(...elems: T[]): void {
    this.#queue.unshift(...elems);
  }

  // add elements to the end of queue
  addBack(...elems: T[]): void {
     this.#queue.push(...elems);
  }

  // remove elements from the start of queue
  removeFront()(l: number): T[] | undefined {
    let _l = l;
    let popElems: T[];

    if (l > this.size()) {
      throw new RangeError(
        "The number of elements that need to be dequeued must be less than the size of queue."
      );
    }
    if (l < 1) {
      throw new RangeError(
        "The number of elements that need to be dequeued must be greater than 1."
      );
    }
    popElems = [];
    while (_l--) {
      popElems.push(this.#stack.shift() as T);
    }
    return popElems;
  }

 // remove elements from the start of queue
  removeBack()(l: number): T[] | undefined {
    let _l = l;
    let popElems: T[];

    if (l > this.size()) {
      throw new RangeError(
        "The number of elements that need to be dequeued must be less than the size of queue."
      );
    }
    if (l < 1) {
      throw new RangeError(
        "The number of elements that need to be dequeued must be greater than 1."
      );
    }
    popElems = [];
    while (_l--) {
      popElems.push(this.#stack.pop() as T);
    }
    return popElems;
  }

  // find the fisrt element in the queue
  peekFront(): T {
    if (this.isEmpty()) {
      throw new Error("the queue have no element.");
    }
    return this.#stack[0];
  }

  // find the last element in the queue
  peekBack(): T {
    if (this.isEmpty()) {
      throw new Error("the queue have no element.");
    }
    return this.#stack[this.size()-1];
  }

  // check if the stack is empty
  isEmpty(): boolean {
    return this.size() === 0;
  }

  // clear all the elements in stack
  clear(): void {
    this.#queue = [];
  }

  // get the length of stack
  size(): number {
    return this.#queue.length;
  }
}
```

## 实战

### 击鼓传花

```typescript
function hotPotato(elementsList, num) {
  const queue = new Queue(); // {1}
  const elimitatedList = [];

  for (let i = 0; i < elementsList.length; i++) {
    queue.enqueue(elementsList[i]); // {2}
  }

  while (queue.size() > 1) {
    for (let i = 0; i < num; i++) {
      queue.enqueue(queue.dequeue()); // {3}
    }
    elimitatedList.push(queue.dequeue()); // {4}
  }

  return {
    eliminated: elimitatedList,
    winner: queue.dequeue(), // {5}
  };
}

const names = ["John", "Jack", "Camila", "Ingrid", "Carl"];
const result = hotPotato(names, 7);

result.eliminated.forEach((name) => {
  console.log(`${name}在击鼓传花游戏中被淘汰。`);
});

console.log(`胜利者： ${result.winner}`);

// Camila在击鼓传花游戏中被淘汰。
// Jack在击鼓传花游戏中被淘汰。
// Carl在击鼓传花游戏中被淘汰。
// Ingrid在击鼓传花游戏中被淘汰。
// 胜利者：John
```

### 回文数检查

维基百科对回文数的定义：

> 回文是正反都能读通的单词、词组、数或一系列字符的序列，例如 madam 或 racecar。

```typescript
function palindromeChecker(aString) {
  if (
    aString === undefined ||
    aString === null ||
    (aString !== null && aString.length === 0)
  ) {
    // {1}
    return false;
  }
  const deque = new Deque<string>(); // {2}
  const lowerString = aString.toLocaleLowerCase().split(" ").join(""); // {3}
  let isEqual = true;
  let firstChar, lastChar;

  for (let i = 0; i < lowerString.length; i++) {
    // {4}
    deque.addBack(lowerString.charAt(i));
  }

  while (deque.size() > 1 && isEqual) {
    // {5}
    firstChar = deque.removeFront(); // {6}
    lastChar = deque.removeBack(); // {7}
    if (firstChar !== lastChar) {
      isEqual = false; // {8}
    }
  }

  return isEqual;
}
```

## 延伸

既然我们在书中使用的是 JavaScript，何不探索一下这门语言本身呢？

当我们在浏览器中打开新标签时，就会创建一个任务队列。这是因为每个标签都是单线程处理所有的任务，称为事件循环。浏览器要负责多个任务，如渲染 HTML、执行 JavaScript 代码、处理用户交互（用户输入、鼠标点击等）、执行和处理异步请求。如果想更多地了解事件循环，可以访问 [Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/).
