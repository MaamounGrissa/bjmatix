<template>
  <div class="app-container">
    <MainContainer msg="Welcome to BJ Matrix"/>
  </div>
</template>

<script>
import MainContainer from './components/MainContainer.vue'

export default {
  name: 'App',
  components: {
    MainContainer
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: #f7f7f7;
}

.app-container {
  max-width: 767px;
  margin: 0 auto;
  overflow: hidden;

}
</style>
