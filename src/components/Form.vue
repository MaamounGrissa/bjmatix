<template>
  <div class="form-container">
    <form action="mailer.php">
      <button @click.prevent="$emit('close')">X</button>
      <div class="images">
          <div class="selected-image">
              <img :src="selectedImage" alt="Main Pic">
          </div>
          <div class="images-list">
              <div v-for="(image, index) in selectedProduct.bgSrc" :key="index" 
                   :class="isActive === index ? 'image active' : 'image'">
                  <img :src="getImageUrl(image)" alt="Product Image" @click.prevent="selectImage(index)">
              </div>
          </div>
      </div>
      <h1 v-text="selectedProduct.name"></h1>
        <div v-if="errors.length">
            <ul><li v-for="(error, index) in errors" :key="index">{{ error }}</li></ul>
        </div>
        <div class="quantity">
            <div class="form-input">
                <input id="quantity" v-model="commande.quantity" type="number" name="quantity" placeholder="Quantité - الكمية" />
                <button id="qtn-plus" @click.prevent="plus">
                <img src="./../assets/plus.png" alt="Plus">
                </button>
                <button id="qtn-moin" @click.prevent="moin">
                    <img src="./../assets/moin.png" alt="Moin">
                </button>
            </div>
            <div class="price">
                {{ commande.price }} DT
            </div>
        </div>
        <div class="form-input">
            <input id="username" v-model="commande.userName" type="text" name="username" placeholder="Nom et Prénom - الاسم و اللقب" >
        </div>
        <div class="form-input">
            <input id="userphone" v-model="commande.userPhone" type="text" name="userphone" placeholder="Téléphone - الهاتف" >
        </div>
        <div class="auto-pos">
            <div class="form-input">
                <input readonly id="userstate" v-model="commande.userState" type="text" name="userstate" placeholder="Ville - المدينة" >
                <img class="loading" src="./../assets/loading-buffering.gif" v-show="isLoading" />
            </div>
            <div class="form-input">
                <input readonly id="usercity" v-model="commande.userCity" type="text" name="usercity" placeholder="Gouvernorat - الولاية" >
                <img class="loading" src="./../assets/loading-buffering.gif" v-show="isLoading" />
            </div>
        </div>
        <button class="submit" type="submit" @click.prevent="sendEmail()">Commander</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import emailjs from 'emailjs-com';

export default {
    name: 'Form',
    props: ['selectedProduct'],
    data() {
        return {
            selectedImage: null,
            isActive: null,
            isLoading: true,
            commande: {},
            errors: [],
            options : {
                enableHighAccuracy: true,
                timeout: 5000,
                maximumAge: 0
            }
        };
    },
    created() {
        this.isLoading = true;
        this.isActive = 0;
        this.commande.quantity = 1;
        navigator.geolocation.getCurrentPosition(
            position => {
                this.GoogleGetStreetAddressFrom(position.coords.latitude, position.coords.longitude)
            },
            error => {
                console.log(error.message);
            },
            this.options
        );
    },
    updated(){
        if (!this.selectedImage && this.selectedProduct.bgSrc) {
            this.selectedImage = require('./../assets/' + this.selectedProduct.bgSrc[0]);
            this.calculateQuantity();
        }
    },
    methods: {
        getImageUrl(fileName) {
            return require('./../assets/' + fileName);
        },
        selectImage(index) {
            this.selectedImage = require('./../assets/' + this.selectedProduct.bgSrc[index]);
            this.isActive = index;
        },
        plus() {
            this.commande.quantity ++;
            this.calculateQuantity()
        },
        moin() {
            if (this.commande.quantity > 1) {
                this.commande.quantity --;
                this.calculateQuantity()
            }
        },
        calculateQuantity() {
            if (this.selectedProduct.price) {
                this.selectedProduct.price.forEach((price, index) => {
                    if (this.commande.quantity >= this.selectedProduct.price.length) {
                        this.commande.price = this.commande.quantity * price;
                    } else if (index == (this.commande.quantity - 1)) {
                        this.commande.price = price;
                    }
                });
            }
        },
        sendEmail() {
            var templateParams = {
                    name: this.commande.userName,
                    product: this.selectedProduct.name,
                    quantity: this.commande.quantity + " ",
                    phone: this.commande.userPhone + " ",
                    state:  this.commande.userState,
                    city: this.commande.userCity,
                    price: this.commande.price + " DT",
            }
            const serviceID = 'service_dyozx05';
            const templateID = 'template_usjk0sm';
            const userID = 'user_aew1XU7c2JP3topXNbU9p';

            try {
                emailjs.send(serviceID, templateID, templateParams, userID);

            } catch(error) {
                console.log({error})
            }

            /* this.commande.quantity = 1;
            this.commande.userName = '';
            this.commande.userPhone = ''; */
        },
        async GoogleGetStreetAddressFrom(lat, long) {
            try {
                var { data } = await axios.get(
                "https://maps.googleapis.com/maps/api/geocode/json?" +
                "&latlng=" + lat + "," + long +
                "&key=AIzaSyCgmA_QwuSkDjuy5DjAXOEpph8Zx0Ocsyo"
                );
                if(data.error_message) {
                    console.log(data.error_message);
                    this.IQLocationGet(lat, long);
                } else {
                    console.log(data)
                    this.commande.userState = 'google test';
                    this.commande.userCity = 'google test';
                    this.isLoading = false;
                }
            } catch (error) {
                console.log(error.message);
                this.IQLocationGet(lat, long);
            } 
        },
        async IQLocationGet(lat, long) {
            try {
                var { data } = await axios.get(
                    "https://us1.locationiq.com/v1/reverse.php?" +
                    "key=pk.3d2cd2ba1f3b3951d60fa60a8c4dacb5&lat=" +
                    lat + "&lon=" + long + "&format=json"
                );
                if(data.error_message) {
                    console.log(data.error_message);
                } else {
                    console.log(data)
                    this.commande.userState = data.address.state;
                    if(data.address.village) {
                        this.commande.userCity = data.address.village + " - " + data.address.postcode;
                    } else {
                        this.commande.userCity = data.address.county + " - " + data.address.postcode;
                    }
                    this.isLoading = false;
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
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.9);
    position: fixed;
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

form:after {
    content: "";
    clear: both;
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
    margin: 20px auto;
}

.form-input {
    margin-bottom: 15px;
    position: relative;
}

.form-input input {
    width: 100%;
    padding: 8px 15px;
    border-radius: 10px;
    border: none;
    font-size: 1rem;
}

.loading {
    width: 25px;
    height: 25px;
    position: absolute;
    right: 6px;
    top: 4px;
}

.quantity {
    position: relative;
    display: flex;
}

.quantity .form-input {
    width: 60%;
}

#qtn-plus, #qtn-moin {
    width: 43px;
    height: 37px;
    font-size: 18px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 0;
    border: none;
    background-color: transparent;
    top: -2px;
}

#qtn-plus {
    right: 0px;
}

#qtn-moin {
    right: 30px;
}

#qtn-plus img, #qtn-moin img {
    width: 100%;
    height: 100%;
}

.price {
    color: #fff;
    padding-left: 20px;
    font-size: 20px;
    font-weight: bold;
    padding-top: 4px;
}

.submit {
    margin-top: 5px;
    padding: 6px 20px;
    border-radius: 10px;
    border: transparent;
    position: static;
    background: rgb(237,237,237);
    background: linear-gradient(0deg, rgba(237,237,237,1) 1%, rgba(127,149,215,1) 15%, rgba(34, 68, 148,1) 46%);
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    color: #fff;
    font-size: 1.1rem;
    font-weight: bold;
    float: right;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>