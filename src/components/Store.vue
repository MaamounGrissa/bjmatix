<template>
  <div class="store">
      <div v-for="myProduct in productsList" :key="myProduct.id">
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
    .product {
        border: 1px solid #CCC;
        background-color: #f7f7f7;
        padding: 20px;
    }
</style>
