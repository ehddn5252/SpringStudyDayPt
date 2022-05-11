# Vue router



### vue-router

- Vue 에서 라우팅 기능을 구현할 수 있도록 지원하는 공식 라이브러리입니다.

라우터란?

- 네트워크에서 데이터를 송수신 하는 중계 장치이다. 
- Vue 에서도 말 그대로 데이터를 보내고 받는 역할을 한다.



#### VueRouter 객체 속성(자료형 변수명 형태) 

- String modeL 기본 값은 Hash 모드 (history 모드를 사용하면 브라우저 히스토리 스택에 기록된다.)
  - 히스토리모드: (localhost:5500/board)
  - 해시모드(localhost:5500/0510_배포/router/router01/#/board)
- String redirect: 리다이렉팅 ( 주로 메인파이지 등에 사용한다.)
- array routes: 페이지 라우팅 정보
  - String path: 페이지 경로(url)
  - Object component: 해당 url 페이지에 사용 할 Component
  - Array children: 중첩 라우팅을 위한 배열
  - name: 라우트의 이름으로 호출할 때 사용된다.

예시

```javascript

Vue.use(VueRouter);

export default new VueRouter({
  mode: "history",
  routes: [
    {
      path: "/a",
      name: "main",
      component: MainContent,
      alias: '/b',
    }]
});
```

- 여기서 **Vue.use()** 코드는 **플러그인을 설치**해서 사용하는 **코드**인데, 플러그인을 한번 설치하고 나면 뷰 인스턴스의 내부에 플러그인에 정의한 기능이 추가된다. 그러고 나면 컴포넌트 내부에서 this로 해당 기능을 편리하게 접근할 수 있다.
- export default 는 import 할 수 있게 export 하여 다른 곳에서 사용하겠다는 뜻으로 생각하면 된다.
  - export name을 사용할 수도 있는데 차이는 같은 기능으로 name은 이름이 있고 default는 이름이 없고의 차이다.
- 위의 경우로 보면 alias 는 사용자는 b를 방문했을 때 url은 b를 유지하지만 a를 방문한 것처럼 매칭된다. 



#### 이름을 가지는 라우트 (name 속성 사용)

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```



이름을 가진 라우트에 링크하려면, 객체를 `router-link`, 컴포넌트의 `to` prop로 전달할 수 있습니다.

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

이것은 `router.push()`와 프로그램적으로 사용되는 것과 정확히 같은 객체입니다.

```js
router.push({ name: 'user', params: { userId: 123 }})
```

두 경우 모두 라우터는 `/user/123` 경로로 이동합니다.



#### \<router-view> 태그 (랜더링)

- **router-view 태그**는 **페이지 표시 태그, 변경된 URL에 따라 해당 컴포넌트를 뿌려주는 영역**이다.
- 즉 라우팅 된 화면이 출력되는 곳입니다.

- 적용된 router-view 태그는 라우팅 정보가 바뀌면 해당하는 뷰를 그려 다른 화면으로 전환하게 됩니다.

```html
<template>
	<div id="app">
        <router-view></router-view>
    </div>
</template>
```



### \<router-link>

- a태그(앵커 태그)의 역할을 한다.

- 차이점은 a 태그를 클릭하면 화면을 갱신하는데, 라우터 링크 태그는 갱신 없이 화면을 이동할 수 있다. 

```html

<router-link to="/"></router-link>
```



#### router-link 태그의 to 사용

- 그냥 **to** 를 사용하면 **문자열**로 읽고 **:to** 를 사용하면 **자바스크립트 표현식**으로 읽는 다는 것을 참고하자. 

in ArticleView.js

```html
<router-link :to="'./modify?articleno=' + article.articleno" class="btn">수정</router-link>
<router-link :to="'./delete?articleno=' + article.articleno" class="btn">삭제</router-link>
```

위에서는 전달할 파라미터가 있어서 bind인 :to 를 사용했다. 문자열은 쌍따옴표로 문자열 표시를 해주고 거기에 변수를 더하는 형식으로 사용



in MainContent.js

```d
<li><router-link to="/register">글 등록</router-link></li>
<li><router-link to="/list">글 목록</router-link></li>
```

전달할 파라미터가 없어서 그냥 to를 사용했다.



vue-router 동작 순서



#### 동적 라우팅

url-path를 동적으로 요청받아 데이터를 넘길 수 있는 라우팅 방식이다. path 끝에 :id를 붙이면 된다. (파라미터로 전달) 반대로 해당 파라미터 정볼르 가져올 때는 $route 객체를 참조하면 된다. post/라는 url에 파라미터로 post id를 넘겨야한다고 생각해보자.

```javascript
import AskItem from '../views/PostItem.vue';
const router = new VueRouter({
	routes: [{
		path : '/post/:id'
        component : PostItem
	}]
});
```

이제 사용자가 /post/123이라는 url을 입력한다면, PostItem 컴포넌트에서는 '123'이라는 ID를 가진 내용을 API를 통해 요청받아야 할 것이다. this.$route.params.id를 통해 가져올 수 있다.

```javascript
created(){
	getPost('requestPost',this.$route.params.id);
}
```







참고 자료:

- vue-router https://jinyisland.kr/post/vue-router/

- named router https://v3.router.vuejs.org/kr/guide/essentials/named-routes.html

- export default https://lily-im.tistory.com/21