# router-viewを固定させる

```html
<template>
  <div>
    <!-- nameをつけるとrouter.jsでコンポーネントを指定できる -->
    <router-view name="header"></router-view>
    <router-view></router-view>
  </div>
</template>

<script>
</script>
```

```js
import Home from "./views/Home.vue";
import Users from "./views/Users.vue";
import HeaderHome from "./views/HeaderHome.vue";
import HeaderUsers from "./views/HeaderUsers.vue";

export default new Router({
  routes: [
    { path: "/", 
      components: {
        // nameがないroute-viewはdefault
        default: Home,
        // App.vueで設定したnameにコンポーネントが適応される
        header: HeaderHome
      }, 
    }, 
    { path: "/users/:id", 
      components: {
        default: Users,
        header: HeaderUsers
      },
      // 名前付きviewごとにpropsを指定しないとidを受け取れない
      props: {
        default: true,
        header: false
      },
    }]
});

```


