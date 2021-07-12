---
title: '타입스크립트 STACK 자료구조 구현해보기'
date: 2021-07-07 18:21:13
category: 'typescript'
draft: false
---

타입스크립트로 Stack 솔루션 구현하기

설계구조 <br />

Stack? 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO(Last In First Out) 형식의 자료 구조. <br />

여러가지 접근 방법이 있겠지만, 1번박스 2번박스 3번박스 스택을 쌓는다고 생각할때, <br />

아무것도 넣지 않거나 1번 박스만 있을 때를 제외하고 1번-2번-3번 박스를 하나의 줄을 연결한다고 생각하여 <br />

마지막 요소에 항상 이전 dom의 정보를 주입하여 last에 대한 표식(코드에선 next라고 표기하였다)을 남기는 방식으로 진행

```tsx
interface Stack {
  readonly size: number
  push(value: string): void
  pop(): string
}

type StackNode = {
  readonly value: string
  readonly next?: StackNode
}

class StackImpl implements Stack {
  private _size: number = 0
  private head?: StackNode

  constructor(private capacity: number) {}
  get size() {
    return this._size
  }
  push(value: string) {
    if (this.size === this.capacity) {
      throw new Error('Stack이 가득 찼습니다.')
    }
    const node: StackNode = { value, next: this.head }
    this.head = node
    this._size++
  }
  pop(): string {
    if (this.head == null) {
      throw new Error('Stack이 비었습니다.')
    }
    const node = this.head
    // next에 이전 값을 기억한 원리로 node를 이전값으로 변경시키는 방식으로 pop구현
    this.head = node.next
    this._size--
    return node.value
  }
}

const stack = new StackImpl(10)
stack.push('1번박스')
stack.push('2번박스')
stack.push('3번박스')
while (stack.size !== 0) {
  console.log(stack.pop())
  // 마지막것부터 차례로 뽑아내고 stack의 길이가 0이 되면 중지
}

stack.pop()
```

https://codesandbox.io/s/ts-stack-9q3ec
