main应用关键代码
```js
<script setup lang="ts">
import { start } from 'qiankun'
import {onMounted}from 'vue'
import { registerMicroApps } from 'qiankun'
  onMounted(()=>{

  registerMicroApps([
    {
      name: 'son', // 子应用名称，跟package.json一致
      entry: '//localhost:7001', // 子应用入口，本地环境下指定端口
      container: '#subcontainer', // 挂载子应用的dom
      activeRule: '/app/son', // 路由匹配规则
    }
  ], {
    beforeLoad: [
      app => {
        console.log('before load', app)
      }
    ],
    beforeMount: [
      app => {
        console.log('before mount', app)
      }
    ],
    afterUnmount: [
      app => {
        console.log('before unmount', app)
      }
    ]
  })
  start()
}) 
</script>
```

```js
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('../views/AboutView.vue')
    },
    {
      path: '/app/son/:pathMatch(.*)*',
      name: 'son',
      component: () => import('../components/SubContainer.vue')
    }
  ]
```
子应用关键代码
```js
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import {
  renderWithQiankun,
  qiankunWindow
} from 'vite-plugin-qiankun/dist/helper'

let app
if (!qiankunWindow.__POWERED_BY_QIANKUN__) {
  createApp(App).use(router).mount('#app');
} else {
  renderWithQiankun({
    mount(props) {
      console.log('--mount',props);

      app = createApp(App);
      app
        .use(router)
        .mount(
          (props.container
            ? props.container.querySelector('#app')
            : document.getElementById('app'))
        );
    },
    bootstrap() {
      console.log('--bootstrap');
    },
    update() {
      console.log('--update');
    },
    unmount() {
      console.log('--unmount');
      app.unmount();
    }
  });
}
```

