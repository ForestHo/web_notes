# react-hooks 学习笔记

- UI = f(data),函数 f 将数据(data)**映射**到**用户界面**(UI)

### 状态是数据吗？

### 状态(state)是什么?

- 状态有一个隐含的意思，就是存在**改变状态的行为**(behavior)
- 背后有一个状态机，这个状态机就是改变状态的行为，让状态发生变化
- 例如:点赞数(likes-状态)**隐含**了增加一个赞(addLike-行为)，而这个
  行为(点赞)只有在某种上下文(context)中才会存在
- 什么是上下文？聊历史人物，上下五千年文化就是上下文
- 我们需要，重新再思考 UI 是什么？......

### 什么是 React Hooks

- React 16.8 的新增特性，可以让你在不写 class 的情况下使用 state 等 react 特性
- hooks 是对函数式组件的极大加强

### 如何描述 UI(User Interface)

数据->视图->消息->重算->数据...
数据->视图->行为...->数据...

这个行为有同步和异步的行为

数据拆分:不变的是属性，变化的是状态

状态映射了行为，因此行为可以封装在状态内...

- 作用是一个状态和视图之外的，需要一个上下文，就是这个 UI
- 状态与视图，作用与视图，上下文与视图，它们都是关联的关系，我们称之为 hooks

### 什么是 Hooks？

- 重新定义 UI 界面是什么，有哪些要素:
- state hook(状态的 hook，状态的行为)
- effect hook(作用的 hook，作用的行为)
- context hook(上下文的 hook，上下文的行为)
- 函数 V = f(props,state), UI = V usehook1(), userhook2()......
- 我们可以把 UI 定义为一个使用了 hook1,hook2,hook3...的视图 V
- 点赞的视图，使用了点赞的 hook，点赞的视图只是根据点赞数量 把视图画出来，
  而点赞的 hook 提供了点赞的能力，提供了点赞的状态，这就是我们说的 UI,这就是 React Hook 重新定义的 UI

- 三个基础的 hook:状态、作用和上下文

### 状态

- 在某个上下文中(用户界面)数据和改变数据的行为

```javascript
// 计数器例子，下面是在定义一个hook
const [count, setCount] = useState(0);
// count-状态，setCount-行为，useState-hooks API
// React hooks帮助大家将数据和行为绑定
```
