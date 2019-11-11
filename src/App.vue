<template>
  <div id="app" class="app">
    <Header></Header>
    <div class="main">
      <router-view name="slidebar"></router-view>
      <router-view name="main"></router-view>    
    </div>
    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import PostList from './components/PostList.vue'
export default {
  name: 'App',
  components:{
    Header,
    PostList
  }
}
</script>

<style scoped>
.app{
  background: #e1e1e1;
}
.main{
  width: 80%;
  margin: 0 auto;
}
</style>