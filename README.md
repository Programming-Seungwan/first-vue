# vueJS 핵심

- 크게 option API와 composition API로 나뉜다.
- option API에서는 리액트에서의 상태와 같은 data, 관련 상태를 바꾸거나 비즈니스 로직을 전개하는 methods, 그리고 각 컴포넌트의 라이프 싸이클 훅들이 존재한다.
- compositon API는 `<script setup>` 컴포넌트를 이용하여 구현한다. script 태그 내에서 기존 Javascript의 `import` 구문을 쓸 수 있다. 태그에 setup 설정을 하지 않을 것이라면 export default 구문 내에서 `setup()` 함수 내에 로직을 적어주는 방식도 있다.
- option API에서는 프론트엔드 상태를 만들기 위해 data() 함수를 사용하고 이에 접근하기 위해서는 this.{상태명}의 문법을 이용한다. 하지만 composition API에서는 `ref()` 함수를 통해 만들어주고 그냥 바로 변수 명으로 접근하면 되는 것이다.

## vueJS의 철학

- 기본적으로 템플릿 엔진의 느낌을 띄며, js, css, html 코드가 colocate 된 느낌이 크다. 이를 SFC(Single File Component)라고 한다.
- 변수를 렌더링하는 것은 `{{}}` 문법을 이용한다.
- 기존에 vueJS에서 prop에 타입을 정해줄 수 있었다. 각 속성을 적어줄 때 type 필드를 통해서 정해줄 수 있었지만, 이는 결국 런타임에서의 오류를 발생시키므로 타입스크립트를 사용하는 이유인 컴파일 타임의 오류를 낼 수가 없다. 하지만 composition API 시대로 넘어오면서 타입스크립트와의 통합이 더 잘 된다.
- 기존의 react에서는 `BrowserRouter` 객체를 전체 컴포넌트에 감싸주고, Routes 태그로 감싼 뒤, Route 컴포넌트로 각각 맞게 설정해주면 되었다. 하지만 vueJS에서는 각각의 라우팅을 기술한 router 객체를 만들어주고 이를 루트 경로의 main.js에서 `app.use(router)` 문법을 통해 더 간단하게 처리할 수 있다.

### Attribute binding

기존의 react에서 html 요소의 src, class, id, disabled와 같은 **속성**에 접근하기 위해서는 {} 문법을 사용했다.

```js
<div className={classNameVariable}>hi</div>
```

<br />

하지만 vueJS는 바인딩이라는 문법을 통해 이를 구현한다.

```js

// Vue.js
<template>
  <div :class="className">
    <img :src="imageUrl" :alt="imageAlt">
    <button :disabled="isDisabled">클릭</button>
  </div>
</template>
```

위의 코드에서 세미 콜론 앞에 `v-bind`를 붙여도 되고, 그냥 생략해도 된다. 요지는 html 요소의 속성에 접근하는 방법이라는 것이다. <br />
해당 속성으로 문자열을 넣어주면, 문자열은 option API의 data()에서 정의해준 상태 변수가 되는데, 물론 `"'someString'"` 처럼 리터럴 문자열을 넣어줄 수도 있다.

### Event Listener

기존의 react에서는 onClick 등의 컴포넌트 속성을 통해 핸들러 함수를 달아주었다. <br /> 하지만 vue는 `v-on`이라는 바인딩 문법을 사용하며 `@` 단축어로 끝낼 수도 있다.

```js
<button @click="increment">{{ count }}</button>
```

### Form vinding

form의 input 등의 요소에는 양방향 바인딩이라는 개념이 필요하다. 기본적으로 리액트에서는 상태를 선언하고, 이를 바꾸는 핸들러 함수를 onChange 속성에 달아주는 방식이었다.<br/>
vueJS에서는 `v-bind:value="데이터명", @input="핸들러함수명"`의 문법을 사용하지만, 그냥 syntatic sugar로 `v-model="변수명"`으로 처리가 가능하다.

### Conditional Rendering

react에서는 {} 내부에서 조건을 분기쳐서 보여주는 방식을 택했다. 하지만 vueJS에서는 `v-if, v-else, v-else-if, v-show` 문법으로 이를 구현한다. 마지막 것은 문자열 스트링에 boolean 타입의 변수가 오면 된다.

### List Rendering

기존의 react 문법에서는 배열의 `map()` 메서드를 이용하여 중복 컴포넌트를 리스트 렌더링했다. 하지만 vueJS에서는 v-for 문법을 이용하면 된다.

```js
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
```

### Computed property

vueJS는 내부적으로 ref()로 만들어진 상태에서 파생되어 계산되는 또 다른 데이터를 최적화하여 적용한다.

### Life cycle and template ref

vueJS 역시 리액트와 마찬가지로 DOM 조작을 사용자가 일일히 할 필요 없이 라이브러리 단에서 해주는 편리한 기술이다. 하지만 개발자는 직접 DOM 객체에 접근하고 싶은 순간이 있다.<br /> 이를 위해 필요한 기술이 바로 `template ref`와 `life cycle` 함수이다.

```js
const pElementRef = ref(null)
<p ref="pElementRef">hello</p>
```

위의 코드처럼 `ref()` 함수를 이용하여 상태를 만들고, 이를 컴포넌트의 ref 속성으로 부여하면 된다. 하지만 template ref는 컴포넌트가 마운트 된 이후에 접근할 수 있다. 왜냐? DOM 에 생성되지도 않은 대상을 그 이전에는 접근할 수 없기 때문이다. 따라서 vueJS에서는 컴포넌트 `life cycle` 함수를 잘 활용하는 것이 중요하다.

```js
<script setup>
import { ref, onMounted } from 'vue'

const pElementRef = ref(null)

onMounted(()=> {
  pElementRef.value.textContent = "seungwan" // textContent 속성에 접근해야 한다.
})
</script>

<template>
  <p ref="pElementRef">Hello</p>
</template>
```

## Composition API

기존의 option API는 `data() {} 부분의 리턴문`을 통해 데이터를 정의하고, methods 속성의 핸들러 함수에서 접근할 때 this 키워드를 이용하면 되었다. 하지만 composition API는 `ref()` 함수를 이용하여 상태를 관리한다. 또한 값에 접근할 때에는 **변수명.value**로 접근해야 한다.

## 본 문서는 tutorial을 공부하고 느낀 점을 모은 것. 실제 프로젝트는 코드 단에 따로 있음.
