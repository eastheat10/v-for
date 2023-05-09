---
marp: true
---

# 리스트 렌더링

 ---

## v-for

---

### `v-for` 디렉티브를 사용하여 일반 배열, 객체 배열 등 리스트 렌더링을 할 수 있다

```html
<tr v-for="fruit in fruits">
  {{ fruit }}
</tr>
```

---

### 숫자 범위에 활용

```html
<span v-for="i in 10">
    {{ i }}
</span>
```

---

### v-if와 v-for

`v-if`와 `v-for`를 함께 사용할 수 있지만 권장되지 않는다.  
`v-if`가 `v-for`보다 우선순위가 높기 때문에 `v-if`에서 `v-for`변수에 접근할 수 없다.

```html
<!-- bad practice --> 
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo.name }}
</li>

<!-- good practice -->
<li v-for="todo in todos">
  <span v-if="!todo.isComplete"> 
    {{ todo.name }}
  </span>
</li>
```

---

## `key`를 통한 상태유지

---

Vue가 `v-for`로 렌더링 된 리스트를 업데이트할 때, 기본적으로 **in-place patch** 전략을 사용한다.  
리스트 아이템의 순서가 변경된 경우, 아이템의 순서와 일치하도록 DOM 엘리먼트를 이동하는 대신, 변경이 필요한 인덱스의 엘리먼트들을 제자리에서 패치(patch)해 아이템을 렌더링 하도록 한다.

이런 기본 동작은 효율적이지만, 리스트 렌더링 출력이 자식 컴포넌트 상태 또는 임시 DOM 상태 (ex: 양식 입력 값)에 의존하지 않는 경우에만 유효하다.

<br />

```plain text
This default mode is efficient, but only suitable when your list render output does not rely on child component state or temporary DOM state (e.g. form input values).
```

<br />

---

![w:850](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdImqBw%2Fbtsep61al1D%2FkfLKx2aQwrp12suJnKSBn0%2Fimg.png)

`!(자식 컴포넌트에 의존) && 임시 DOM 상태`

---

### Default

```html
<template>
<div>
  <h1>v-bind:key="None"</h1>
  <table>
    <thead>
      <tr>
        <th>None</th>
        <th>Name</th>
        <th>Value</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="fruit in fruits0"> <!-- eslint 빨간줄 -->
        <td> - </td>
        <td>{{ fruit.name }}</td>
        <td>
          <input type="number" />
        </td>
      </tr>
    </tbody>
  </table>
  <button v-on:click="shift0">shift</button>
</div>
<template>

<script setup>
  import { ref } from "vue";

  const fruits0 = ref([
    { id: 1, name: "apple" },
    { id: 2, name: "orange" },
    { id: 3, name: "melon" },
    { id: 4, name: "grape" },
    { id: 5, name: "mango" }
  ]);

  const shift0 = () => fruits0.value.push(fruits0.value.shift());
</script>
```

---

| Before | After |
| :-: | :-: |
| ![default before](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOmZ9h%2FbtsepOlMu45%2Fzjkam4pNxXXYpYlvzsBo80%2Fimg.png) | ![default after](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcSM06P%2FbtserDjdmeY%2FSmR8QIFiipZ4lpdbQBviG0%2Fimg.png) |

---

### Array index key

```html
<template>
<div>
  <h1>v-bind:key="Array Index"</h1>
  <table>
    <thead>
      <tr>
        <th>Index</th>
        <th>Name</th>
        <th>Value</th>
        <th>Delete</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(fruit, i) in fruits1" :key="i">
        <td>{{ i }}</td>
        <td>{{ fruit.name }}</td>
        <td>
          <input type="number" />
        </td>
        <td>
          <button @click="deleteFruit2(fruit)">X</button>
        </td>
      </tr>
    </tbody>
  </table>
  <button v-on:click="shift1">shift</button>
</div>
</template>
<script setup>
  import { ref } from "vue";

  const fruits1 = ref([
    { id: 1, name: "apple" },
    { id: 2, name: "orange" },
    { id: 3, name: "melon" },
    { id: 4, name: "grape" },
    { id: 5, name: "mango" }
  ]);

  const shift1 = () => fruits1.value.push(fruits1.value.shift());
  const deleteFruit1 = (fruit) => (fruits1.value = fruits1.value.filter((f) => f.id !== fruit.id))
  const deleteFruit2 = (fruit) => (fruits2.value = fruits2.value.filter((f) => f.id !== fruit.id));
</script>
```

---

#### 수정

| Before | After |
| :-: | :-: |
| ![none-before](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb8M8R3%2FbtseoYhSzyV%2FobEgKvnxh2bNcARSvIIBx0%2Fimg.png) | ![none-after](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcdyNqI%2FbtseqtOXOkk%2FGkbVL9zP2CntYDimv4kfH1%2Fimg.png) |

---

0번 인덱스에서 `fruit` 객체가 `shift`되지만, 다시 0번 인덱스가 바로 채워진다.

또한, 4번 인덱스가 3번으로 당겨지지만, 바로 4번 인덱스에 값이 채워진다.  

따라서 키가 변하지 않으므로 `tr` 객체에는 변화가 없다. 그래서 `input` 태그는 변화가 없고, 내부의 `{{ fruit.name }}`만 변경된다.

---

#### 삭제

| Before | After |
| :-: | :-: |
| ![none-before](https://user-images.githubusercontent.com/38150034/236971918-be198e4e-696a-4060-848a-5cdb7ff0c1c6.png) | ![none-after](https://user-images.githubusercontent.com/38150034/236971943-50f4faaf-05cd-401f-93c2-e6381915d2fa.png) |

* 해당 아이템의 key(인덱스)가 변경되지 않고, 인덱스 순서에 맞게 변경된다.
* 아이템 삭제 시 객체의 key인 인덱스가 함께 변경되므로 index key는 사용하지 않는 것이 좋다.

---

### Object Id key

```html
<template>
<div>
  <h1>v-bind:key="Object Id"</h1>
  <table>
    <thead>
      <tr>
        <th>Object Id</th>
        <th>Name</th>
        <th>Value</th>
        <th>Delete</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="fruit in fruits2" :key="fruit.id">
        <td>{{ fruit.id }}</td>
        <td>{{ fruit.name }}</td>
        <td><input type="number" /></td>
        <td>
          <button @click="deleteFruit2(fruit)">X</button>
        </td>
      </tr>
    </tbody>
  </table>
  <button v-on:click="shift2">shift</button>
</div>
</template>
<script setup>
  import { ref } from "vue";

  const fruits2 = ref([
    { id: 1, name: "apple" },
    { id: 2, name: "orange" },
    { id: 3, name: "melon" },
    { id: 4, name: "grape" },
    { id: 5, name: "mango" }
  ]);

  const shift2 = () => fruits2.value.push(fruits2.value.shift());

  const deleteFruit2 = (fruit) => (fruits2.value = fruits2.value.filter((f) => f.id !== fruit.id))
</script>
```

---

#### 수정

`fruit.id`라는 고유값을 가진 `tr` 태그가 `shift` 후 다른 `fruit.id`를 가진 `tr` 태그로 채워지므로 `tr` 태그 자체가 다른 값으로 바뀌었다.  
따라서 `input`태그 또한 함께 이동하였다.

---

| Before | After |
| :-: | :-: |
| ![object key before](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYJDIt%2FbtserNsI3T3%2FFFYAyeIkrTKKPOZUGIUAfK%2Fimg.png) | ![object key after](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FB9UV8%2Fbtseqt2uO9m%2FxkzplA918X7mSBkBX2t1pK%2Fimg.png) |

---

#### 삭제

| Before | After |
| :-: | :-: |
| ![none-before](https://user-images.githubusercontent.com/38150034/236971967-f0aa6f88-aa54-4873-8c40-1b03d8c6235b.png) | ![none-after](https://user-images.githubusercontent.com/38150034/236971985-07925a83-0379-4250-9235-05d6ca64fc4f.png) |

* 객체의 key가 함께 변경되었다.

---

### 결론

반복되는 DOM 컨텐츠가 단순하거나, 의도적으로 렌더링 동작을 통해 성능 향상을 꾀하는 경우가 아니라면, 가능한 `v-for`는 `key` 속성과 함께 사용하는 것을 [권장한다](https://ko.vuejs.org/style-guide/rules-essential.html#use-keyed-v-for).

`key: number | string | symbol`

---

![w:800](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQRagP%2FbtseqdezplJ%2FL3Ej6yM4SBUTVkDjrTKWXk%2Fimg.png)

---

## 배열 변경 감지

### 수정 메서드

-   Vue는 반응형 배열의 메서드가 호출되는 것을 감지하여, 필요한 업데이트를 발생시킵니다.

`push()` | `pop()` | `shift()` `unshift()` | `splice()` | `sort()` | `reverse()`

<br />

### 배열 교체

-   `filter()` | `concat()` | `slice()`는 원본 배열을 수정하지 않고 새 배열을 반환하므로 기존의 배열(ref(\[\]). value)을 변경해야 한다.

---

## 감사합니다

---
출처

-   [https://ko.vuejs.org/guide/essentials/list.html](https://ko.vuejs.org/guide/essentials/list.html)
-   [https://goodteacher.tistory.com/525](https://goodteacher.tistory.com/525)
