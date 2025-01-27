# vueJS 핵심
- 크게 option API와 composition API로 나뉜다.
- option API에서는 리액트에서의 상태와 같은 data, 관련 상태를 바꾸거나 비즈니스 로직을 전개하는 methods, 그리고 각 컴포넌트의 라이프 싸이클 훅들이 존재한다.
- compositon API는 `<script setup>` 컴포넌트를 이용하여 구현한다. script 태그 내에서 기존 Javascript의 `import` 구문을 쓸 수 있다. 태그에 setup 설정을 하지 않을 것이라면 export default 구문 내에서 `setup()` 함수 내에 로직을 적어주는 방식도 있다.
- option API에서는 프론트엔드 상태를 만들기 위해 data() 함수를 사용하고 이에 접근하기 위해서는 this.{상태명}의 문법을 이용한다. 하지만 composition API에서는 `ref()` 함수를 통해 만들어주고 그냥 바로 변수 명으로 접근하면 되는 것이다.
