## vuex 개념 정리

- React에서는 redux, mobex와 같은 개념
> https://vuex.vuejs.org/kr/

### 1. 설치하기 

##### 1) npm 설치하기

> npm install vuex --save

##### 2) vue의 main.js에 삽입

```node
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

### 2. 시작하기

##### 1) 가장 단순한 저장소

- javascript 안에서 ES2015(ES6)가 이전 문법에 큰 차이가 나타남.

- main.js에 담기
```node
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

- 이렇게 하면 상태 저장이 끝나게 됨.
- commit: mutiaions 안에 있는 값을 '호출'한다.

### 3. 활용하기

* Tip] .name(class이름) 쓰고 [tab]누르면 자동으로 <div class="name"></div> 만들어짐
 
##### 1) 숫자 증가 버튼 만들기

``` vue
this.$store.commit("increment")
this.$store.dispatch("")
```
- mutaion은 commit으로 호출
- action은 dispatch로 호출

- computed: 숫자, 문자열의 변화를 구현할 때 사용. 

```node
couputed:{
  count(){
    return this.$store.state.count
  }
}
```

- main.js

```node
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

.
.
.
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```
- 사용하려는 vue

```vue
<template>
  <div id="app">
    <div class="count">{{$store.state.count}}</div>
    <button @click="increase">증가</button>
  </div>
</template>

<script>
export default {
  methods:{
    increase(){
      this.$store.commit("increment")
    }
  }
}
</script>

```

## Vuex의 상태 관리를 컴포넌트에서 사용하기

### 1. Getters

- 언제 활용? 완료한 갯수를 한 파일 안에서만 쓰지 않는다면? <br>
  앱 전체에서 사용한다면 getters 안에 넣어두면 생산성이 올라갈 것


- mapGetters: mapState와 같은 기능을 가지고 있음

```node
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // getter를 객체 전개 연산자(Object Spread Operator)로 계산하여 추가합니다.
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

### 2. 변이

```node
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```

```node
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // ES2015에서 계산 된 프로퍼티 이름 기능을 사용하여
    // 상수를 함수 이름으로 사용할 수 있습니다
    [SOME_MUTATION] (state) {
      // 변이 상태
    }
  }
})
```

- mutations 안에 들어가는 함수의 이름을 상수로 바꾸어 사용할 경우에, <br>
  실수로 이름을 바꿔버려서 하드코딩 되어있는 모든 참조문들을 바꿀 위험이 줄어들게 됨

### 3. 액션

- mutation 안에 있는 여러개의 commit을 한 번에 불러 올 수 있을 것
- getters 안에 있는 것도 접근 가능
- 곧 state 안의 모든 요소들에 접근이 가능하고, 활용 가능


[ store ]
```node
actions: {
  test({commit}){
    commit(a);
    commit(b);
    commit(c);
    commit(d);
  }
}
```

[활용하려는 vue]
``` node
methods:{
  increase(){
    this.$store.dispatch("test")
  }
}
```

### 4. 모듈


### Tip

- 왜 안돼지? 
  * Advanced 안에서 애플리케이션 구조
  * vue Life cycle을 배워야 실질적 문제해결이 가능 (vue에서는 중요함.)
  * data 변화를 vue가 watching 하고있는지 확인해야 함.

![image](https://user-images.githubusercontent.com/69338643/131459724-88ce75a4-c641-4c23-91e5-d1b27937b6fc.png)

출처: https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram
