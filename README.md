<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="命名速查表" />
  </a>
</p>

这里是 [Naming cheatsheet](https://github.com/kettanaito/naming-cheatsheet#naming-cheatsheet) 的中文翻译版本。

# 命名速查表

- [使用英语](#使用英语)
- [约定命名规则](#约定命名规则)
- [S-I-D](#s-i-d)
- [避免缩写](#避免缩写)
- [避免上下文重复](#避免上下文重复)
- [反映预期结果](#反映预期结果)
- [函数命名](#函数命名)
  - [A/HC/LC 模式](#ahclc-模式)
    - [动作](#动作)
    - [上下文](#上下文)
    - [前缀](#前缀)
- [单复数](#单复数)

---

给变量和函数命名是很难的，这张表试图让它变得更容易。

尽管这些建议可以适用于任何编程语言，但我将用 JavaScript 的例子来解释和说明。

## 使用英语

在命名变量和函数时要使用英语。

```js
/* 不好的 */
const primerNombre = 'Gustavo'
const amigos = ['Kate', 'John']

/* 好的 */
const firstName = 'Gustavo'
const friends = ['Kate', 'John']
```

> 不管你喜欢与否，英语是编程中的主流语言：所有编程语言的语法都是用英语写的，无数的文档和教学资料也是如此。通过使用英语编写代码，你可以极大地提高代码的凝聚力。

## 约定命名规则

挑选**一个**命名规则并遵循它。它可以是 `camelCase`、`PascalCase`、`snake_case` 或其他任何规则，只要代码中保持一致即可。许多编程语言在命名规则方面有着自己的约定，你可以查看编程语言文档或研究 GitHub 上的一些流行的存储库。

```js
/* 不好的 */
const page_count = 5
const shouldUpdate = true

/* 好的 */
const pageCount = 5
const shouldUpdate = true

/* 同样好的 */
const page_count = 5
const should_update = true
```

## S-I-D

名字必须 _简短_、 _直观_ 和具有 _描述性_。

- **简短（Short）**：名字必须不需要花很长时间来输入，因此也不需要被记住。
- **直观（Intuitive）**：名字必须读起来自然，尽可能地接近常用说法。  
- **描述性（Descriptive）**：名字必须以最有效的方式反映它所做的/所拥有的。

```js
/* 不好的 */
const a = 5 // "a" 可以指代任何东西
const isPaginatable = a > 10 // "Paginatable" 听起来不自然
const shouldPaginatize = a > 10 // paginatize 是自创的动词

/* 好的 */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // 和 hasPagination 二选一
```

## 避免缩写

避免使用缩写，它们只会降低代码的可读性。找到一个简短的、具有描述性的名字可能很难，但这不是使用缩写的借口。

```js
/* 不好的 */
const onItmClk = () => {}

/* 好的 */
const onItemClick = () => {}
```

## 避免上下文重复

名字不应该与它所定义的上下文重复。如果删除上下文不降低名字的可读性，就一定要把它从名字中删除。

```js
class MenuItem {
  /* 方法名与上下文（"MenuItem"）重复 */
  handleMenuItemClick = (event) => { ... }

  /* `MenuItem.handleClick()` 看起来舒服很多 */
  handleClick = (event) => { ... }
}
```

## 反映预期结果

名字应该反映出预期的结果。

```js
/* 不好的 */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* 好的 */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

## 函数命名

### A/HC/LC 模式

在给函数命名时有一个可用的公式可以遵循：

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

在下面的表格中看看这个模式如何被应用。

| 名字                   | 前缀     | 动作 (A)    | 强语境 (HC)        | 弱语境 (LC)      |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **注意**：上下文的顺序会影响变量的含义。例如，`shouldUpdateComponent` 意味着你要手动更新组件，而`shouldComponentUpdate` 告诉你组件会自动更新，而你只是控制更新的时间。  
> 换句话说，**强语境强调了变量的含义**。

---

### 动作

函数名的动词部分，它是函数名中最重要的部分，负责描述这个函数 _做_ 了什么。

#### `get`

立即访问数据（例如：内部数据的速记器）。

```js
function getFruitCount() {
  return this.fruits.length
}
```

> 另见 [compose](#compose)。

在执行异步操作时，你也可以使用 **get**。

```js
async function getUser(id) {
  const user = await fetch(`/api/user/${id}`)
  return user
}
```

#### `set`

以声明的方式设置一个变量，将 **A** 设置为 **B**。

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

#### `reset`

将一个变量设置为它的初始值或状态。

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

#### `remove`

从某个地方移走某样东西。

举个例子，如果你在搜索页面上有一个所选过滤器的集合，从集合中移除其中一个过滤器就是 `removeFilter`，而**不是** `deleteFilter`（这也是英语中的自然表达）：

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> 另见 [delete](#delete)。

#### `delete`

从真实的空间完全擦除某样东西。

想象一下，你是一个内容编辑者。那儿有一个臭名昭著的帖子，你想把它删掉。一旦你点击了一个闪亮的“删除帖子”按钮，CMS 执行的是 `deletePost` 动作，而不是 `removePost`。

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> 另见 [remove](#remove)。

> **`移除` 还是 `删除`?**
>
> 当 `remove` 和 `delete` 之间的区别对你来说不是那么明显时，我建议你看看它们的相反动作—— `add` 和 `create`。
> `add` 和 `create` 的关键区别是 `add` 需要一个目的地，而 `create` **不需要目的地**。你 `add` 某样东西 _去某个地方_，不是 “`create` 它 _去某个地方_”。
> 你只需知道 `remove` 与 `add` 是一对，`delete` 与 `create` 是一对。
>
> [这里](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962)有详细的解释。

#### `compose`

从现有的数据中创建新的数据。主要适用于字符串、对象或函数。

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + '-' + pageId
}
```

> 另见 [get](#get)。

#### `handle`

处理一个动作。通常在命名回调方法的时候使用。

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

### 上下文

函数所操作的域。

函数通常是对 _某物_ 的操作。重要的是要说明它可操作的域，或者至少说明预期的数据类型是什么。

```js
/* 使用原语操作的纯函数 */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* 对文章精确处理的函数 */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> 在一些特定的语言中可能允许省略上下文。例如，在 JavaScript 中，`filter` 通常是对 Array 的操作。添加明确的 `filterArray` 就没有必要了。

---

### 前缀

前缀增强了变量的含义。它很少在函数名中使用。

#### `is`

描述了当前环境的特征或状态（通常是布尔值）。

```js
const color = 'blue'
const isBlue = color === 'blue' // 特征
const isPresent = true // 状态

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

#### `has`

描述了当前环境是否拥有某个值或某种状态（通常变量值是布尔类型）。

```js
/* 不好的 */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* 好的 */
const hasProducts = productsCount > 0
```

#### `should`

反映了一个积极的条件语句与某种行动的结合（通常变量值是布尔类型）。

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

#### `min`/`max`

代表最小值或最大值。在描述边界或限制时使用。

```js
/**
 * 根据最小/最大的边界范围，渲染随机数量的帖子。
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

#### `prev`/`next`

表明变量在当前环境中的上一个或下一个状态。在描述状态转换时使用。

```js
async function getPosts() {
  const prevPosts = this.state.posts

  const latestPosts = await fetch('...')
  const nextPosts = concat(prevPosts, latestPosts)

  this.setState({ posts: nextPosts })
}
```

---

### 单复数

像前缀一样，变量名可以是单数或是复数，这取决于变量是有一个值还是多个值。

```js
/* 不好的 */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* 好的 */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
