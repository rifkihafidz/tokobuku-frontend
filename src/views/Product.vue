<template>
  <div class="product">
    <Header/>  
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="gambar_default" alt="" style="height:580px"/>
                            </div>
                            <!-- <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :dots="false" :nav="false">
                                    <div
                                        v-for="thumb in productDetails.galleries"
                                        :key="thumb.id"
                                        class="pt"
                                        @click="changeImage(thumb.photo)"
                                        :class="thumb.photo == gambar_default ? 'active' : '' ">
                                            <img :src="thumb.photo" alt />
                                    </div>
                                </carousel>
                            </div>-->
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name}}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>Rp {{ productDetails.price | numFormat }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart">
                                        <a @click="saveCart(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct/>

    <Footer/>
  </div>
</template>

<script>
// @ is an alias to /src

 import Header from '@/components/Header.vue'
 import Footer from '@/components/Footer.vue'
 import RelatedProduct from '@/components/RelatedProduct.vue'
 
//  import carousel from "vue-owl-carousel"
 import axios from "axios"

export default {
  name: 'Product',
  components: {
    Header,
    RelatedProduct,
    Footer,
    // carousel
  },
    data() {
        return {
            gambar_default:"",
            productDetails: [],
            cart: []
        }
    },
    methods: {
        changeImage(urlImage) {
            this.gambar_default = urlImage
        },
        setDataPicture(data) {
            //replace object productDetails dengan data dari API
            this.productDetails = data;
            //replace value gambar_default dengan data dari API (galleries)
            this.gambar_default = data.galleries[0].photo
        },
        saveCart(idProduct, nameProduct, priceProduct, photoProduct) {
            var productStored = {
                "id" : idProduct,
                "name" : nameProduct,
                "price" : priceProduct,
                "photo" : photoProduct
            }
            this.cart.push(productStored);
            const parsed = JSON.stringify(this.cart);
            localStorage.setItem('cart', parsed);
        }
    },
    mounted() {
        if (localStorage.getItem('cart')) {
            try {
                this.cart = JSON.parse(localStorage.getItem('cart'));
            } catch(e) {
                localStorage.removeItem('cart');
            }
        }

        axios  
            .get("http://127.0.0.1:8000/api/products", {
                params: {
                    slug: this.$route.params.slug
                }
            })
            .then(res => (this.setDataPicture(res.data.data)))
            //eslint-disable-next-Line no-console
            .catch(err => console.log(err))

        
    }
  
}
</script>

<style scoped>
    .product-thumbs .pt {
        margin-left: 7px;
        margin-right: 7px;
    }
</style>