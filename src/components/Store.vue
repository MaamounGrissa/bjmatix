<template>
  <div class="store">
      <div class="product" v-for="myProduct in productsList" :key="myProduct.id">
           <Product :myProduct="myProduct" @open="openModal" />
      </div>
      <Form :selectedProduct="theProduct" :isOpen="isOpen" ref="myForm" v-show="isOpen" @close="closeModal()"/>
  </div>
</template>

<script>
import Product from "./Product.vue";
import Form from './Form.vue';
import * as myData from "../Data.js";

export default {
  name: 'Store',
  components: {
      Product,
      Form
  },
  data() {
      return {
          productsList: [],
          isOpen: false,
          theProduct: {},
          theSelectedImage: null
      }
  },
  mounted() {
        this.productsList = myData.products;
  },
  methods: {
    openModal(evt, selectedProd) {
        this.theProduct = selectedProd;
        // this.$refs.myForm.selectImage(0);
        this.isOpen = true;
    },
    closeModal() {
        this.isOpen = false;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .store {
    padding: 20px;
  }

  .product {
    margin-bottom: 20px;
    box-shadow: 0 20px 15px -10px rgb(0 0 0 / 40%);
    border-radius: 20px;
    overflow: hidden;
  }

</style>
