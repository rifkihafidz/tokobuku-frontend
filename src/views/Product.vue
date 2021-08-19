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
                                    <h3 class="stoks">{{ stok }}</h3>
                                    <h3>{{ productDetails.name}}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>Rp {{ productDetails.price | numFormat }}</h4>
                                    <div class="row">
                                            <button class="minus" @click="minus()">-</button>
                                            <h3 class="qty">{{ jumlah }}</h3>
                                            <button class="plus" @click="plus()">+</button>
                                    </div>
                                </div>
                                <div class="quantity">
                                <router-link to="/cart">
                                    <a @click="saveCart(productDetails.id, productDetails.name, productDetails.price, productDetails.photo, jumlah)" href="#" class="primary-btn pd-cart">Add To Cart</a>
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
            cart: [],
            jumlah:1,
            harga:0,
            stok:"",
        }
    },
    methods: {
        changeImage(urlImage) {
            this.gambar_default = urlImage
        },
        setDataPicture(data) {
            //replace object productDetails dengan data dari API
            this.productDetails = data;
            this.harga = data.price;
            //replace value gambar_default dengan data dari API (galleries)
            this.gambar_default = 'http://127.0.0.1:8000/storage/'+data.photo
        },
        saveCart(idProduct, nameProduct, priceProduct, photoProduct, jumlah) {
            let data = this.cart;
            let index;
            if(data != 0){
                for (var key in  data) {
                    if(idProduct === data[key].id){
                        index = key;
                    }
                }
                    if(index != null){
                        this.cart[index].jumlah += jumlah;
                        this.cart[index].price += priceProduct;
                    }else{
                        var productStoreds = {
                            "id" : idProduct,
                            "name" : nameProduct,
                            "price" : priceProduct,
                            "photo" : 'http://127.0.0.1:8000/storage/'+photoProduct,
                            "jumlah" : jumlah,
                        }
                        this.cart.push(productStoreds);
                    }
            }else if(data == 0){
                var productStored = {
                    "id" : idProduct,
                    "name" : nameProduct,
                    "price" : priceProduct,
                    "photo" : 'http://127.0.0.1:8000/storage/'+photoProduct,
                    "jumlah" : jumlah,
                }
                this.cart.push(productStored);
            }
            const parsed = JSON.stringify(this.cart);
            localStorage.setItem('cart', parsed);
            // localStorage.setItem('stok', 0);
            // console.log(localStorage.getItem('stok'))
        },
        plus(){
            if(this.productDetails.quantity > this.jumlah){
                this.jumlah++
                this.productDetails.price = this.harga * this.jumlah
            }else{
                this.productDetails.quantity = 0
            }
        },
        minus(){
            if(this.jumlah === 1){
                this.jumlah = 1
            }else if(this.jumlah === 0){
                this.jumlah = 0
                this.productDetails.price = this.harga * this.jumlah
            }else{
                this.jumlah--
                this.productDetails.price = this.harga * this.jumlah
            }
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
            .then(res => {
                this.setDataPicture(res.data.data)
                console.log(this.cart)
                // if(this.productDetails.id == )
                let data = this.cart;
                let jumlah;
                if(data != 0){
                    for (var key in  data) {
                        if(this.productDetails.id === data[key].id){
                            jumlah = data[key].jumlah;
                        }
                    }
                    if(this.productDetails.quantity-jumlah === 0){
                        this.jumlah = 0;
                        this.productDetails.quantity = 0;
                        this.productDetails.price = 0;
                        this.stok = "[Stok Tidak Cukup]";
                    }
                }
            })
            //eslint-disable-next-Line no-console
            .catch(err => console.log(err))

            // 
        
    }
  
}
</script>

<style scoped>
    .product-thumbs .pt {
        margin-left: 7px;
        margin-right: 7px;
    }
    .qty {
        color:#333333;
        font-weight:bold;
        margin:10px;
    }
    .pd-title .stoks {
        color:RED;
        font-weight:bold;
    }
    .minus{
        background:#AAAAAA;
        width:30px;
        height:30px;
        text-align:center;
        margin:15px;
    }
    .plus{
        background:#AAAAAA;
        width:30px;
        height:30px;
        text-align:center;
        margin:15px;
    }
</style>