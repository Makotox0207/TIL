```js
// route.js
export default new Router({
  routes: [
    {
      path: '/', 
      component: EmployeeIndexPage
    },
    {
      path: '/employees/:id', 
      component: EmployeeDetailPage,
	  // propsをtrueに設定すると、route.paramsがコンポーネントのプロパティとして設定される
      props: true
    }
  ],
});
```

```js
// EmployeeDetailPage.vue

<script>
import axios from 'axios';

export default {
  // propsで子コンポーネントに値を渡せる
  props: ['id'],
  data: function () {
    return {
      employee: {}
    }
  },
  mounted () {
    this.mountDetail();
  },
  methods: {
    mountDetail(){
      axios.get(`/api/v1/employees/${this.id}.json`)
      .then(response => (this.employee = response.data))
    }
  }
}
</script>

<style scoped>
</style>
```