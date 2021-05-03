<template>
  <div class="mycontainer">
    <Header />
    <Store />
  </div>
</template>

<script>
import Header from "./Header.vue"
import Store from "./Store.vue"

export default {
  name: 'MainContainer',
  components: {
    Header,
    Store
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.mycontainer {
  max-width: 767px;
  margin: 0 auto;
  padding-top: 108px;
}
</style>
