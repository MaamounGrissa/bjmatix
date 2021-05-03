<template>
  <div class="store">
      <div v-for="myProduct in productsList" :key="myProduct.id">
           <Product  :myProduct="myProduct"/>
      </div>
  </div>
</template>

<script>
import Product from "./Product.vue";
import * as myData from "../Data.js";

export default {
  name: 'Store',
  components: {
      Product
  },
  data() {
      return {
          productsList: []
      }
  },
  mounted() {
        this.productsList = myData.products
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .product {
        border: 1px solid #CCC;
        background-color: #f7f7f7;
        padding: 20px;
    }
</style>
