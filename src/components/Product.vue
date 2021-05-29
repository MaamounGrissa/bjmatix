<template>
    <splide :slides="slides">
        <splide-slide v-for="slide in slides" :key="slide.src" class="slide">
            <img v-if="slide.bgSrc" :src="slide.bgSrc">
            <div class="overlay"></div>
            <h3 v-if="slide.title" v-text="slide.title" />
            <h4 v-if="slide.price" v-text="slide.price" />
            <p v-if="slide.description" v-text="slide.description" />
            <button v-if="slide.btnTarget === 'form'" @click="$emit('open' ,$event, myProduct)">Achetez</button>
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
    background-color: rgba(0, 0, 0, 0.4);
}

h3, h4 , p {
    position: absolute;
    left: 60px;
    color: #fff;
    z-index: 10;
    padding-right: 30px;
}

h3 {
    bottom: 130px;
    font-size: 2.5rem;
}

h4 {
    bottom: 70px;
    font-size: 2.5rem;
}

p {
    bottom: 100px;
    font-size: 20px;
    line-height: 1.5;
    padding-right: 30px;
}

button {
    padding: 8px 20px;
    text-align: center;
    position: absolute;
    bottom: 25px;
    right: 25px;
    border: 1px solid #fff;
    color: #fff;
    font-weight: 600;
    font-size: 18px;
    z-index: 100;
    background: transparent;
    transition: all 0.3s ease-in-out;
}

button:hover, button:active {
    background: #fff;
    color: #000;
}
</style>