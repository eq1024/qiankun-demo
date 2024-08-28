<template>
  <div id="subcontainer" class="sub-app-container">
  </div>
</template>

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

<style scoped>
.sub-app-container {
      height: 100%;
      width: 100%;
    }
</style>