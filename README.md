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

위의 코드처럼 `ref()` 함수를 이용하여 상태를 만들고, 이를 컴포넌트의 ref 속성으로 부여하면 된다. 하지만 template ref는 컴포넌트가 마운트 된 이후에 접근할 수 있다. 왜냐? DOM 에 생성되지도 않은 대상을 그 이전에는 접근할 수 없기 때문이다. 따라서 vueJS에서는 컴포넌트 `life cycle` 함수를 잘 활용하는 것이 중요하다. 기존의 option API에서는 이를 메서드 형태로 기술하고 하고 싶은 작업들을 적어주면 된다.

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

### watch

react에서 side Effect , 즉 컴포넌트의 렌더링을 제외한 작업인 Fetch Data와 같은 일들을 처리할 때에는 `useEffect()` 같은 훅을 이용하곤 했다. vueJS에서는 이를 **watch** 함수를 통해 구현한다. 단, 하나의 상태를 의존성으로 가질 때에는 첫 인자로 ref 상태를 넣어주면 되지만 여러 개일 경우에는 이를 배열의 원소들로 넣어주면 된다. 기존의 option API에서는 `watch :` 다음에 원하는 상태를 함수 형태로 적고 기술해주면 된다.

```js
watch: {
    todoId() {
      this.fetchData()
    }
  }


```

> 다음의 코드는 composition API에서의 watch 구현 방법이다.

```js
<script setup>
import { ref, watch } from 'vue'

const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}

fetchData() // 첫 마운트 시에 실행되는 함수

watch(todoId,() => {
  fetchData();
} ) // todoId가 일종의 의존성이고, 이것이 바뀔 때마다 fetchData() 함수가 실행됨

</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
```

### Components & Props

기존의 reactJS에서는 다른 파일에서 작성한 컴포넌트를 import하여 jsx 형태로 적어주면 되었다. <br />
먼저 option API에서는 다음과 같다

1. 자식 컴포넌트의 `export default` 구문에서 props 필드로 원하는 필드를 받는다.
2. 부모 컴포넌트의 `export default` 구문의 components 필드에 import 해온 자식 컴포넌트를 등록해준다.
   <br/>

반면, composition API에서는 다음과 같다.

1. 자식 컴포넌트의 script setup 구문에서 `defineProps({message: string})` 문법처럼 받을 prop을 정의한다.
2. 부모 컴포넌트에서는 그냥 script 태그 내에서 import하고 바로 사용하면 된다.

> 다만, 부모 컴포넌트에서 자식 컴포넌트에게 prop을 전달해줄 때에는 v-bind 문법처럼 쓴다.

```js
<ChildComp :msg="greeting" />
```

### Emit

기존의 reactJS 프레임워크에서는 자식 컴포넌트가 부모 컴포넌트에게 바로 이벤트를 트리거해서 데이터를 전송해주는 방식이 있지는 않다. 그래서 react 진영에서는 부모 컴포넌트의 상태를 바꾸는 핸들러 함수를 자식 컴포넌트에 전달해주거나, contextAPI를 이용하는 방식이 권장되었다.<br />
하지만 vueJS에서의 `emit`은 이를 구현하는 기능인데 특징은 다음과 같다.

**option API**

1. 자식 컴포넌트에서 emit 관련 함수를 작성. 당연히 여러 데이터를 보내고 싶다면 객체로 묶어서 보내면 됨

```js
<template>
  <div>
    <h2>자식 컴포넌트</h2>
    <button @click="sendData">데이터 보내기</button>
  </div>
</template>

<script>
export default {
  methods: {
    sendData() {
      this.$emit('custom-event', '안녕하세요, 부모님!');
    },
  },
};
</script>

```

2. 부모 컴포넌트에서 `v-on` 문법이나 `@` 문법으로 이를 잡고 받은 데이터를 핸들러 함수로 처리

```js
<template>
  <div>
    <h1>부모 컴포넌트</h1>
    <ChildComponent @custom-event="handleEvent" />
    <p>받은 메시지: {{ message }}</p>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent },
  data() {
    return {
      message: '',
    };
  },
  methods: {
    handleEvent(data) {
      this.message = data;
    },
  },
};
</script>

```

**composition API**

1. 자식 컴포넌트에서 emit을 선언하고 데이터를 전송.

```js
<template>
  <div>
    <h2>자식 컴포넌트</h2>
    <button @click="sendData">데이터 보내기</button>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['custom-event']); // 이벤트 정의

const sendData = () => {
  emit('custom-event', '안녕하세요, 부모님!'); // 이벤트 발생
};
</script>

```

2. 부모 컴포넌트에서 이를 받아서 핸들러 함수로 처리

```js
<template>
  <div>
    <h1>부모 컴포넌트</h1>
    <ChildComponent @custom-event="handleEvent" />
    <p>받은 메시지: {{ message }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

const message = ref('');

const handleEvent = (data) => {
  message.value = data;
};
</script>
```

### slot

사실 이 기능은 별것 없다. 부모 컴포넌트가 자식 컴포넌트의 내부에 넣은 컴포넌트 코드를 slot이라는 jsx 문법 내에 렌더링하는 기능이다. 물론 부모 컴포넌트가 하나의 fragment만을 전달하는 것은 아니고, `template`으로 감싸고 이름을 전달하면 여러 개도 넣어줄 수 있다.

**자식 컴포넌트**

```js
<template>
  <div class="box">
    <header>
      <slot name="header"></slot> <!-- "header" 슬롯 -->
    </header>
    <main>
      <slot></slot> <!-- 기본 슬롯 -->
    </main>
    <footer>
      <slot name="footer"></slot> <!-- "footer" 슬롯 -->
    </footer>
  </div>
</template>
```

**부모 컴포넌트**

```js
<template>
  <ChildComponent>
    <template #header>
      <h1>헤더 영역</h1>
    </template>

    <p>이 부분은 기본 슬롯에 들어갑니다.</p>

    <template #footer>
      <p>푸터 영역</p>
    </template>
  </ChildComponent>
</template>

<script setup>
import ChildComponent from './ChildComponent.vue';
</script>

```

## Composition API

기존의 option API는 `data() {} 부분의 리턴문`을 통해 데이터를 정의하고, methods 속성의 핸들러 함수에서 접근할 때 this 키워드를 이용하면 되었다. 하지만 composition API는 `ref()` 함수를 이용하여 상태를 관리한다. 또한 값에 접근할 때에는 **변수명.value**로 접근해야 한다.

## 본 문서는 tutorial을 공부하고 느낀 점을 모은 것. 실제 프로젝트는 코드 단에 따로 있음.
