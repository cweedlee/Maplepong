# Core Framework — Technical Documentation

> A React-like SPA framework implemented from scratch in Vanilla JavaScript.  
> 바닐라 JS로 직접 구현한 React 유사 SPA 프레임워크의 기술 문서입니다.

---

## Architecture Overview / 아키텍처 개요

```
myReact.js        — Component lifecycle, state management
myReactDOM.js     — DOM mounting and reconciliation
```

The framework is split into two core modules, mirroring React's separation of `react` and `react-dom`.  
프레임워크는 React의 `react`와 `react-dom` 분리 구조를 참고하여 두 모듈로 나뉩니다.

---

## myReact.js

> Provides core UI primitives: component creation, state management, and lifecycle hooks.  
> 컴포넌트 생성, 상태 관리, 생명주기 등 UI 구성에 필요한 핵심 기능을 제공합니다.

### createElement()

Babel compiles JSX syntax into `createElement()` calls.  
JSX 문법은 Babel 컴파일러에 의해 `createElement()` 호출로 변환됩니다.

**Parameters:** `type`, `props`, `...children`  
**Returns:** `VirtualNode object`

```jsx
// JSX
const App = () => (
  <div id="app">
    <h1>Hello</h1>
    <button onClick={func} />
  </div>
);

// Compiled by Babel → createElement calls
h("div", { id: "app" }, [
  h("h1", {}, ["Hello"]),
  h("button", { onClick: func }, [])
]);
```

---

### VirtualNode

Each `VirtualNode` represents a node in the virtual DOM tree.  
각 `VirtualNode`는 가상 DOM 트리의 노드를 나타냅니다.

```js
export default class VirtualNode {
  constructor(instance) {
    this.instance = instance; // Component instance / 컴포넌트 인스턴스
    this.children = [];       // Child nodes / 자식 노드
    this.sibling = [];        // Sibling nodes / 형제 노드
    this.return = null;       // Parent node / 부모 노드
    this.stateNode = null;    // Linked real DOM node / 연결된 실제 DOM 노드
  }
}
```

**Lifecycle of a VirtualNode / VirtualNode 생명주기:**

| Phase | Behavior |
|-------|----------|
| Creation / 생성 시 | Receives `createElement` output, initializes metadata |
| First render / 첫 렌더 | Creates real DOM node, stores reference in `stateNode` |
| Re-render / 재렌더 시 | Runs diff against previous VirtualNode, queues only changed nodes |
| Deletion / 삭제 시 | TBD |

---

### useState

Manages local component state.  
컴포넌트의 로컬 상태를 관리합니다.

```js
useState(initValue)
```

- Stores current and next state values
- Calling the setter enqueues a re-render
- 세터 호출 시 현재/다음 값을 저장하고 렌더 큐에 추가

---

### useEffect

Handles side effects after render.  
렌더 이후 사이드 이펙트를 처리합니다.

```js
useEffect(callback, [dependencies])
```

- Collected into an array during render, executed after DOM commit
- Only runs when dependencies change
- 렌더 중 수집 후 DOM 커밋 이후에 실행, 의존성 변경 시에만 동작

---

## myReactDOM.js

> Handles mounting React components to the real DOM and managing updates.  
> React 컴포넌트를 실제 DOM에 마운트하고 업데이트를 관리합니다.

```js
myReactDOM.createRoot(root) // Initialize root / 루트 초기화
root.render(element)        // Mount to DOM / DOM에 마운트
root.reconciliation()       // Diff and apply updates / diff 후 실제 DOM 반영
```

---

## State Update Cycle / 상태 업데이트 사이클

```
setState() called
      ↓
Current & next state saved, render enqueued
현재/다음 상태 저장, 렌더 큐에 추가
      ↓
New FiberTree (newVirtualDOM) created
새 FiberTree 생성
      ↓
Diff: oldFiberNode vs newFiberNode
변경된 노드 탐색
      ↓
Changed nodes flagged
변경 노드 플래그 처리
      ↓
ReactDOM reconciliation — real DOM updated
실제 DOM 반영
      ↓
useEffect callbacks executed
useEffect 콜백 실행
```

### Why this approach? / 이 방식을 선택한 이유

- **Batched updates** — multiple `setState()` calls are queued before applying, reducing unnecessary re-renders  
  여러 `setState()` 호출을 배치 처리하여 불필요한 재렌더를 줄임
- **Minimal DOM updates** — diff algorithm ensures only changed nodes are updated  
  diff 알고리즘으로 변경된 노드만 실제 DOM에 반영
- **Functional components over class components** — avoids mutable `this`, improves readability and memory usage  
  mutable `this` 문제 회피, 가독성 및 메모리 효율 향상

---

## Key Design Decisions / 설계 결정 사항

**Functional components only / 함수형 컴포넌트만 사용**

Class components were considered but rejected:  
클래스형 컴포넌트를 검토했으나 다음 이유로 채택하지 않았습니다:

- `this` is mutable — leads to unpredictable behavior / `this`의 mutable 특성으로 인한 예측 불가능한 동작
- Lower readability / 가독성 저하
- Higher memory consumption / 메모리 소비 증가

**FiberNode architecture / FiberNode 아키텍처**

Inspired by React's Fiber architecture to enable:  
React의 Fiber 아키텍처를 참고하여 구현:

- Incremental rendering / 점진적 렌더링
- Efficient tree traversal via `children`, `sibling`, `return` pointers  
  `children`, `sibling`, `return` 포인터를 통한 효율적인 트리 순회