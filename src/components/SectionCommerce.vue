<template>
    <div class="container">
        <div v-if="product" v-bind:class="isFemale ? 'female-bg': 'male-bg'">
            <div class="card">
                <img class="product-img" v-bind:src="product.image"/>
                <h3 v-bind:class="isFemale ? 'product-title-female': 'product-title-male'">
                    {{product.title}}
                </h3>
                <p class="product-type">{{product.category}}</p>
                <p class="product-rating">{{product.rating.rate}}/5</p>
                <RatingCommerce :rating="roundedRating" :gender="productGender" />
                <div class="divider-top"></div>
                <p class="product-description">
                    {{truncatedDescription}}
                </p>
                <p v-bind:class="isFemale ? 'product-price-female': 'product-price-male'">${{product.price}}</p>
                <div class="button-layout">
                    <button v-bind:class="isFemale ? 'button-buy-female': 'button-buy-male'">Buy Now</button>
                    <button @click="nextProduct" v-bind:class="isFemale ? 'button-next-female': 'button-next-male'">Next Product</button>
                </div>
                <div class="divider-bottom"></div>
            </div>
        </div>
        <div v-else class="unavail-bg">
            <div class="card" style="text-align: center;">
                <img class="unavail-img" src="https://res.cloudinary.com/diyb3la39/image/upload/v1679384619/Random/sad-face_chjrhs.png"/>
                <div class="unavail-container">
                    <p>This Product is Unable to shown</p>
                    <button @click="nextProduct" class="unavail-button">Next Product</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import RatingCommerce from './RatingCommerce.vue'
    import axios from 'axios';

    export default {
        components: {
            RatingCommerce
        },
        data() {
            return {
                isFemale:false,
                product: null,
                productId: 1,
                maxProductId: 20,
            }
        },
        mounted() {
            this.fetchProduct();
        },
        watch: {
            product: {
                handler(newProduct) {
                    this.isFemale = newProduct.category.includes("wo");
                },
                immediate: true,
            },
        },
        methods: {
            fetchProduct() {
            axios.get(`https://fakestoreapi.com/products/${this.productId}`)
                .then(response => {
                    if (response.data.category === "men's clothing" || response.data.category === "women's clothing") {
                        this.product = response.data;
                    }else {
                        this.product = null;
                    }
                })
                .catch(error => {
                    console.log(error);
                });
            },

            nextProduct() {
                if (this.productId < this.maxProductId) {
                    this.productId++;
                } else {
                    this.productId = 1;
                }
                this.fetchProduct();
            },
        },
        computed: {
            productGender() {
                if (this.product.category.includes("wo")) {
                    return 'female';
                } else {
                    return 'male';
                }
            },
            truncatedDescription() {
                const words = this.product.description.split(' ');
                if (words.length > 50) {
                    return words.slice(0, 50).join(' ') + '...';
                } else {
                    return this.product.description;
                }
            },
            roundedRating() {
                return Math.floor(this.product.rating.rate);
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Inter');
    .container {
        height: 100vh;
    }
    .male-bg {
        background-color: #d6e6ff;
        background-image: url('https://res.cloudinary.com/diyb3la39/image/upload/v1679376259/Random/bg-pattern_hebhru.png');
        height: 70%;
    }
    .female-bg {
        background-color: #fde2ff;
        background-image: url('https://res.cloudinary.com/diyb3la39/image/upload/v1679376259/Random/bg-pattern_hebhru.png');
        height: 70%;
    }
    .unavail-bg {
        background-color: #dcdcdc;
        height: 70%;
    }
    .card {
        background-color: white;
        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
        width: 80%;
        height: 80%;
        position: absolute; 
        top: 50%; 
        left: 50%; 
        transform: translate(-50%,-50%);
        overflow: hidden;
        border-radius: 10px;
    }
    .product-title-male{
        position: absolute;
        left: 50%;
        right: 30px;
        top: 5%;
        color: #002772;
        font-family: 'Inter';
        font-weight: bold;
    }
    .product-title-female{
        position: absolute;
        left: 50%;
        right: 30px;
        top: 5%;
        color: #720060;
        font-family: 'Inter';
        font-weight: bold;
    }
    .product-img{
        width: 300px;
        height: 300px;
        position: absolute;
        left: 10%;
        top: 20%;
    }
    .divider-top{
        position: absolute;
        top: 38%;
        left: 50%;
        background-color: #dcdcdc;
        width: 45%;
        height: 2px;
    }
    .divider-bottom{
        position: absolute;
        top: 70%;
        left: 50%;
        background-color: #dcdcdc;
        width: 45%;
        height: 2px;
    }
    .grid-container{
        display: grid;
        column-gap: 20px;
    }
    .product-type{
        position: absolute;
        top: 30%;
        left: 50%;
        font-size: 14px;
        color: #3f3f3f;
        font-family: "Inter";
    }
    .product-rating{
        position: absolute;
        top: 30%;
        left: 81%;
        font-size: 14px;
        color: #3f3f3f;
        font-family: "Inter";
    }
    .product-description{
        position: absolute;
        left: 50%;
        right: 50px;
        top: 40%;
        color: #1e1e1e;
        font-family: 'Inter';
        word-wrap: break-word;
    }

    .product-price-male{
        position: absolute;
        left: 50%;
        right: 50px;
        top: 70%;
        color: #002772;
        font-family: 'Inter';
        font-size: larger;
        font-weight: bold;
    }
    .product-price-female{
        position: absolute;
        left: 50%;
        top: 70%;
        color: #720060;
        font-family: 'Inter';
        font-size: larger;
        font-weight: bold;
    }
    .button-layout{
        position: absolute;
        left: 50%;
        right: 5px;
        top: 84%;
        display: flex;
        flex-direction: row;
        align-items: center;
    }
    .button-buy-male{
        padding: 7px 70px 7px 70px;
        background-color: #002772;
        border: 2px solid #002772;
        border-radius: 3px;
        color: white;
        font-family: 'Inter';
        margin-right: 10px;
        cursor: pointer;
    }
    .button-buy-female{
        padding: 7px 70px 7px 70px;
        background-color: #720060;
        border: 2px solid #720060;
        border-radius: 3px;
        color: white;
        font-family: 'Inter';
        margin-right: 10px;
        cursor: pointer;
    }
    .button-next-male{
        padding: 7px 60px 7px 60px;
        color: #002772;
        background-color: white;
        border: 2px solid #002772;
        border-radius: 3px;
        font-family: 'Inter';
        cursor: pointer;
    }
    .button-next-female{
        padding: 7px 60px 7px 60px;
        color: #720060;
        background-color: white;
        border: 2px solid #720060;
        border-radius: 3px;
        font-family: 'Inter';
        cursor: pointer;
    }
    .unavail-img{
        position: relative;
        left: 6.5%;
        top: 15%
    }
    .unavail-container{
        position: absolute;
        font-family: 'Inter';
        top: 40%;
        left: 33%;
    }
    .unavail-button{
        padding: 7px 150px 7px 150px;
        color: #3f3f3f;
        background-color: white;
        border: 2px solid #000;
        border-radius: 3px;
        font-family: 'Inter';
        cursor: pointer;
    }
</style>