## NUXT JS 개념

https://nuxtjs.org/

- Vue.js의 단점을 보완해주는 framework
- 핵심? Server side rendering
  * vue는 SPA(Single Page App) client에 코드가 전송되어 저장되어 rendering된다는 것. <br>
   그래서 router가 생기게 되는 것.
  * first page rendering이 느리게 됨. java script를 다 다운받기 때문
  * SEO가 상대적으로 떨어짐. 검색엔진을 통해 검색을 했을 경우 원하는 데이터가 제대로 전달되지 못할 수도 있음.
    <br> 공유에 적합하지 않은 형태인 것.
- 정적 사이트에 편리
- 코드가 frame으로 정해져있기 때문에 pattern을 알게 되면 생산성이 높아지게 됨.


### 1. 설치하기

> npm init nuxt-app project-name


### 2. create-nuxt-app 알아보기

#### 1) assets

The assets directory contains your uncompiled assets such as Stylus or Sass files, images, or fonts.

style파일을 처리하는 곳

#### 2) components

#### 3) layouts

- 모든 페이지가 똑같은 템플릿이 아님.
- 양식이 다를 경우 다른 레이아웃을 만들어 코드를 쓰게 하는 것.

#### 4) middleware

- 서버가 실행될때 먼저 실행되는 코드들
- 보통은 로그인 
- 로그인이 된 사용자와 로그인이 안된 사용자가 다른 페이지를 보기 때문.

#### 5) pages

- router: router배열 만들어서 path넣고, component 연결하는 작업을 했었음.
- pages는 원하는 path를 넣으면 됨. (ex. profile 디렉토리 만들고 그 안에 index.vue 만들면 profile 자체가 path가 되는 것임.)
- component 안에는 어떤건 page, 어떤건 component 섞여서 들어있었는데, nuxt는 분리가 되어있음.

#### 6) plugin

- 공통적으로 쓰는 모듈이나 함수를 넣어두면 됨.
- vue.js는 어떤 라이브러리든 충돌없이 사용이 가능함.
- but nuxt에서는 오류가 생길 가능성이 많음. server side rendering이다보니 그런 문제가 있는 것.
- mounting되기 전 javascript를 mount되기 전에 써야하는 경우를 넣어두는 것
- module by module이긴 하지만 어떤 모듈을 사용하기 위해서는 꼭 plugin에 넣어야하는 문제가 있기도 함.<br>
 (해당 library의 github의 issue에서 참고)
 
 ```node
 import some from '../'
 
 export default {
 }
 ```
 vue.js에선 위 코드가 바로 먹혔는데, nuxt에서는 이렇게 사용하는 것이 안될 수도(plugin에 넣고만 사용할 수 있는) 있음

#### 7) static

- 정적 페이지
- img, logo 등을 관리 
- http://localhost:3000/image.png 라고 접근이 가능하게 됨

#### 8) store

- 만드는 방법이 간단함. 그냥 store 안에 넣으면 됨.

```node
export const state = () => ({
  counter: 0
})

export const mutations = {
  increment(state) {
    state.counter++
  }
}
```
이렇게하면 store 설정 끝임

```node
$store.state.counter;
```
라고 하면 접근 가능

- namespaced 특성 때문에, 사용자 정보를 관리하고 싶으면 그냥 store 디렉토리 안에 'user.js'만 만들면 끝

user.js
```node
export const state = () => ({
 email: "example@email.com
})

$store.state.user.email;
```

이렇게하면 호출 가능함. 

### 3. nuxt.config.js 살펴보기

- meta tag
- googlefont 등을 가지고 올 수 있는 부분임. 

##### 2) Global CSS
- css 파일을 가져올 수 있는 곳.
- global 적용이 됨

##### 3) plugin

```node
plugins: [
 "~/plugins/util.js"
 ]
 ```

##### 4) components

- 자동 import 기능
- 가지고 오고싶은 vue 파일을 그냥 불러오면 됨.

```node
components: true
```
자동으로 component 등록, but Directory 안에 들어가는 구조는 auto로 되지 않음.

```node
import Logo from "@components/logo"
```
이런식으로 하지 않아도 됨.

##### 5) buildModules

##### 6) modules


### 4. Nuxt 구조 알아보기 






