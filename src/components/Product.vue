<template>
    <splide :slides="slides">
        <splide-slide v-for="slide in slides" :key="slide.src" class="slide">
            <img v-if="slide.bgSrc" :src="slide.bgSrc">
            <div class="overlay"></div>
            <h3 v-if="slide.title" v-text="slide.title" />
            <p v-if="slide.description" v-text="slide.description" />
            <button v-if="slide.btnTarget === 'form'" @click="$emit('open' ,$event, myProduct)">Achetez à {{ slide.price }}</button>
            <button v-else-if="slide.btnTarget === 'plus'">Voir Plus</button>
        </splide-slide>
    </splide>
</template>

<script>
import { Splide, SplideSlide } from '@splidejs/vue-splide';
import '@splidejs/splide/dist/css/themes/splide-default.min.css';

export default {
    components: {
        Splide,
        SplideSlide
    },
    props: ['myProduct'],
    data() {
        return {
            slides: [],
        };
    },
    created() {
        this.theProduct = this.myProduct;
        this.slides = [
            { 
                bgSrc: require('./../assets/' + this.myProduct.bgSrc[0]),
                title: this.myProduct.name,
                price: this.myProduct.price[0] + ' DT',
                btnTarget: this.myProduct.btnTarget[0]
            },
            { 
                bgSrc: require('./../assets/' + this.myProduct.bgSrc[1]),
                description: this.myProduct.desc,
                price: this.myProduct.price[0] + ' DT',
                btnTarget: this.myProduct.btnTarget[1]
            }
        ];
    }
}
</script>

<style scoped>

.slide {
    position: relative;
}

img {
    width: 100%;
    height: 300px;
    object-fit: cover;
}

.overlay {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.3);
}

h3 , p {
    position: absolute;
    left: 50px;
    color: #fff;
    z-index: 10;
    padding-right: 35px;
}

h3 {
    bottom: 40px;
    font-size: 2.5rem;
}

p {
    bottom: 110px;
    font-size: 20px;
    line-height: 1.7;
    padding-right: 40px;
    padding-left: 10px;
}

button {
    padding: 10px 20px;
    text-align: center;
    position: absolute;
    bottom: 40px;
    right: 50px;
    border: 1px solid #fff;
    color: #fff;
    font-weight: 600;
    font-size: 20px;
    z-index: 100;
    background: transparent;
    transition: all 0.3s ease-in-out;
    box-shadow: 0 20px 15px -10px rgb(0 0 0 / 40%);
    cursor: pointer;
    border-radius: 10px;
}

button:hover, button:active {
    background: #fff;
    color: #000;
}

@media(max-width: 500px) {

    h3 {
        font-size: 1.6rem;
        bottom: 100px;
        width: 100%;
        text-align: center;
        left: 0;
        padding: 0;
    }

    p {
        font-size: 1rem;
    }

    button {
        width: 200px;
        background: white;
        color: black;
        bottom: 35px;
        right: 50%;
        transform: translate(50%, 0);
    }
}
</style>