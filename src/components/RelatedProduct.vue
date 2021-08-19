<template>
    <!-- Related Products Section End -->
    <div class="related-products spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="section-title">
                        <h2>Related Products</h2>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-lg-12" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :nav="false" :autoplay="true"  :dots="false">
                        <div class="product-item mr-4" v-for="itemProduct in products" v-bind:key="itemProduct.slug">
                            <div class="pi-pic">
                                <img v-bind:src="`http://127.0.0.1:8000/storage/`+itemProduct.photo" alt="" style="height:424px">
                                <ul>
                                    <li @click="saveCart(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.photo)" class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view">
                                    <!-- 

                                        <a v-bind:href="'/product/'+itemProduct.slug"> Quick View</a>
                                        -->
                                        <router-link v-bind:to="'/product/'+itemProduct.slug" @click.native="link()"> Quick View</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <router-link v-bind:to="'/product/' + itemProduct.slug">
                                    <h5>{{ itemProduct.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    Rp {{ itemProduct.price | numFormat }}
                                    <!-- <span>$35.00</span> -->
                                </div>
                            </div>
                        </div>
                    </carousel>
                    
                </div>
            </div>
        </div>
    </div>
    <!-- Related Products Section End -->
</template>

<script>
import axios from 'axios'
import carousel from 'vue-owl-carousel'

export default {
    name: 'RelatedProduct',
    components: {
        carousel
    },

    data() {
        return {
            products: [],
            cart:[],
        };
    },

    mounted() {
        // localStorage.removeItem("search");
        axios  
            .get("http://127.0.0.1:8000/api/products")
            .then(res => (this.products = res.data.data.data))
            // eslint-disable-next-Line no-console
            .catch(err => console.log(err))

        if (localStorage.getItem("cart")) {
            try {
                this.cart = JSON.parse(localStorage.getItem("cart"));
            } catch (e) {
                localStorage.removeItem("cart");
            }
        }
    },
    methods: {
        saveCart(idProduct, nameProduct, priceProduct, photoProduct) {
            var productStored = {
                "id" : idProduct,
                "name" : nameProduct,
                "price" : priceProduct,
                "photo" : `http://127.0.0.1:8000/storage/`+photoProduct
            }

            this.cart.push(productStored);
            const parsed = JSON.stringify(this.cart);
            localStorage.setItem("cart", parsed);

            window.location.reload();
        },
        link(){
            window.location.reload();
        }
    }
}

</script>