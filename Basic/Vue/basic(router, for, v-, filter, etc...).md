## vue
- 경로 
    - @ : 절대경로 
    - ./ :  같은 상대 경로로 가져올 수 도 @ 같은 절대 경로로도 가져 올 수 O
    ```js
    <script>
        import Header from "./components/Header";
        import Menu from "./components/Menu";
        import Content from "@/components/Content";

        export default {
            name: "app",
            components: { // 가져온 component 들을 등록합니다.
            Header,
            Menu,
            Content
            }
        };
    </script> 
    ```
- 생성자
    - new View()

- 데이터 접근 
    - {{ }}
    - v-model : 양방향 접근
- html 
    - v-
    
``` js 
<div id="app">
  <p>{{ message }}</p>
  <p><input v-model="message"></p>
</div>

<script>
var myObject = new Vue({
  el: '#app',
  data: {message: 'Hello Vue!'}
})
</script>
```
- for 문 : v-for
```js
<div id="app">
<ul>
<li v-for="x in todos">
{{ x.text }}
</li>
</ul>
</div>

<script>
myObject = new Vue({
    el: '#app',
    data: {
    todos: [
        { text: 'Learn JavaScript' },
        { text: 'Learn Vue.js' },
        { text: 'Build Something Awesome' }
        ]
    }
})
</script>   
```

- if / show
    - v-if : 조건에 맞지 않으면 렌더링 자체를 하지 X
    - v-show : 조건에 맞지 않아도 렌더링 한 후 display:none 처리

- state / prop : jquery와 달리 직접적으로 document에 접근해 DOM 제어X , DOM을 관리하기 위해 사용
    - state : 나의 data , data()를 이용해 구성
    - prop : 누구로부터(부모 Component) 받는 data
    ```js
    <script>
        export default {
        data() {  // Box 의 state
            return {
            width: 40, // 넓이
            height: 80 // 높이
            };
        }
        };
    </script>
    ```
- router : vue.js에서 페이지 간 이동을 위한 라이브러리
    - \<router-link to = "경로"> : 컴파일시 \<a> 태그로 변환 / to 속성값의 경로로 이동 
        1. v-bind와 함께 사용하면 동적 경로 가능 
        2. to = "test/path" : 현재 url에 path가 붙음 
        3. to = "/test/path" : default에 url이 붙음 (대표적으로 사용)
        4. \<roter-link to ="경로"> == router.push("경로") 
     
    - \<router-view> : 페이지 표시 태그 , url에 따른 컴포넌트가 화면에 그려지는 영역
    - router-link에 to 속성에 path 대신 name 지정 가능 
        ```js 
            <router-link :to="{ name: 'user', params: { username: 'erina' }}">
                    User
            </router-link>
            const routes = [
            {
                path: '/user/:username', // 동적 세그먼트 는 ':'가 앞에 붙음 EX) path='user/:username/post/:post_id' -> this.route.params.username == 'seulebee' this.route.params.post_id == 20
                name: 'user',
                component: User
            }
            ]          
        ``` 
    - redirect 
        ```js
        // '/a'에서 '/b'로 리디렉션
        routes: [{path : '/a', redirect: '/b'}]
        // 이름이 있는 라우터의 경우
        routes: [{path : '/a', redirect: {name: 'babo'}}]
        // 함수를 사용하여 동적 리디렉션 가능 
        routes: [{path : '/a', redirect: to => {return '/with-params:id'}}]
        ```
    - alias : url을 다르게 표시 가능 , ex) 'a'방문을 '/b'로 방문한 것처럼 표시
        ```js
        routes: [
            { path: '/a', component: A, alias: '/b' }]

        alias: {
            '/@': path.resolve(__dirname, './src'), 
            '/@components': path.resolve(__dirname, './src/components'),
            '/@router': path.resolve(__dirname, './src/router'),
            '/@views': path.resolve(__dirname, './src/views'), 
            '/@js': path.resolve(__dirname, './src/js'), 
            '/@css': path.resolve(__dirname, './src/css'), 
            '/@images': path.resolve(__dirname, './src/images'), 
            '/@store': path.resolve(__dirname, './src/store')
        }
        ```
- filter : 특정 데이터를 가공하여 표현할 수 있는 기능, computed와 달리 전역으로 설정 가능 
    - filter() : 배열 내의 모든 요소르르 돌면서 주어진 함수의 조건에 맞는 요소만을 모아 새로운 배열을 리턴
    - https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter 
    ```js
    // Array.prototype.filter()
    let newArray = arr.filter(callback(currentVal[, index, [array]]));

    let fruits = ['kiwi', 'banana','apple', 'peach', 'pineapple', 'strawberry'];
    let result = fruits.filter(item => item.length>6);
    console.log(result);
    // 결과 : ["pineapple", "strawberry"]

    =========================
    let fruits = ['kiwi', 'banana','apple', 'peach', 'pineapple', 'strawberry'];
    let search = (query) => {
        return fruits.filter((item) =>
                item.toLowerCase().includes(query,toLowerCase()));
    }
    console.log(search('ea'));
    // 결과 : ["peach", "pineapple"]

    ```
