<template>
  <div id="container">
    <vue-loading type="spin" color="#333" :size="{ width: '50px', height: '50px' }"></vue-loading>
  </div>
</template>

<script>
// インポート
import { VueLoading } from 'vue-loading-template'
export default {
  name: 'HelloWorld',
  // componentsに登録
  components: {
    VueLoading
  }
}
</script>