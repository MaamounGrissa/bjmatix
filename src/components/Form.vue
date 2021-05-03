<template>
  <div class="form-container">
    <form action="mailer.php">
      <button @click.prevent="closeModal()">X</button>
      <div class="images">
          <div class="selected-image">
              <img :src="selectedImage" alt="Main Pic">
          </div>
          <div class="images-list">
              <div v-for="(image, index) in Product.bgSrc" :key="index" 
                   :class="isActive === index ? 'image active' : 'image'">
                  <img :src="getImageUrl(image)" alt="Product Image" @click="selectImage(index)">
              </div>
          </div>
      </div>
      <h1 v-text="Product.name"></h1>
        <div v-if="errors.length">
            <ul><li v-for="(error, index) in errors" :key="index">{{ error }}</li></ul>
        </div>
        <div class="form-input">
            <input id="username" v-model="userName" type="text" name="username" placeholder="Nom et Prénom - الاسم و اللقب" >
        </div>
        <div class="form-input">
            <input id="userphone" v-model="userPhone" type="text" name="userphone" placeholder="Téléphone - الهاتف" >
        </div>
        <div class="auto-pos">
            <div class="form-input">
                <input id="userstate" v-model="userState" type="text" name="userstate" >
            </div>
            <div class="form-input">
                <input id="usercity" v-model="userCity" type="text" name="usercity" >
            </div>
        </div>
        <div class="manual-pos">
            <div class="form-input">

            </div>
            <div class="form-input">

            </div>
        </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Form',
    props: ['myProduct'],
    data() {
        return {
            Product: [],
            selectedImage: String,
            isActive: null,
            command: {},
            errors: [],
            options : {
                enableHighAccuracy: true,
                timeout: 5000,
                maximumAge: 0
            }
        };
    },
    created() {
        this.Product = this.myProduct
        this.selectedImage = require('./../assets/' + this.Product.bgSrc[0]);
        this.isActive = 0;
        navigator.geolocation.getCurrentPosition(
            position => {
            this.getStreetAddressFrom(position.coords.latitude, position.coords.longitude)
            },
            error => {
            console.log(error.message);
            },
            this.options
        );
    },
    methods: {
        closeModal() {
            this.$emit('close')
        },
        getImageUrl(fileName) {
            return require('./../assets/' + fileName);
        },
        selectImage(index) {
            this.selectedImage = require('./../assets/' + this.Product.bgSrc[index]);
            this.isActive = index;
        },
        async getStreetAddressFrom(lat, long) {
            try {
                var { data } = await axios.get(
                "https://maps.googleapis.com/maps/api/geocode/json?latlng=" +
                    lat +
                    "," +
                    long +
                    "&key={AIzaSyCgmA_QwuSkDjuy5DjAXOEpph8Zx0Ocsyo}"
                );
                if(data.error_message) {
                    console.log(data.error_message)
                } else {
                // this.address = data.results[0].formatted_address;
                    console.log(data.results)
                }
            } catch (error) {
                console.log(error.message);
            } 
        }
    }
}
</script>

<style scoped>
.form-container {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.9);
    position: absolute;
    top: 0;
    left: 0;
    z-index: 999;
    transition: all 0.3s ease-in-out;
    padding: 20px;
}

form {
    width: 100%;
    height: 100%;
    position: relative;
    padding-top: 50px;
}

p {
    color: #fff;
}

button {
    border-radius: 50%;
    padding: 5px 8px;
    font-weight: bold;
    color: #333;
    text-align: center;
    border: 1px solid #fff;
    background-color: #fff;
    position: absolute;
    right: 0;
    top: 0;
}

img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.images {
    display: flex;
    justify-content: center;
    align-items: center;
}

.selected-image {
    width: 50%;
    border-radius: 20px;
}

.images-list {
    width: 50%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    padding: 15px;
}

.image {
    width: 45%;
    height: 50%;
    margin-bottom: 10px;
    border: 2px solid transparent;
}

.active {
    border: 2px solid rgb(79, 168, 79)
}

h1 {
    text-align: center;
    color: #fff;
    margin: 15px auto;
}

.form-input {
    margin-bottom: 15px;
}

.form-input input {
    width: 100%;
    padding: 8px 15px;
    border-radius: 10px;
    border: none;
    font-size: 1rem;
}
</style>