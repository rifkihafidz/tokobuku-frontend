<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <div class="section-title mt-3">
            <h2>Daftar Produk</h2>
          </div>
        </div>
      </div>

      <div class="row" v-if="products.length > 0">
        <div
          class="product-item col-lg-3"
          v-for="itemProduct in products"
          v-bind:key="itemProduct.slug"
        >
          <div class="pi-pic">
            <img
              v-bind:src="`http://127.0.0.1:8000/storage/` + itemProduct.photo"
              alt=""
              style="height:424px"
            />
            <ul>
              <li class="quick-view">
                <router-link v-bind:to="'/product/' + itemProduct.slug">
                  Detail Buku</router-link
                >
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
      </div>
      <div class="col-lg-12" v-else>
        <p>Produk belum tersedia untuk saat ini.</p>
      </div>
    </div>
    <!--<div class="container-fluid">
            <div class="row">
                <div class="col-lg-12">
                    <div class="section-title">
                        <h2>Best Products</h2>
                    </div>
                </div>
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :nav="false" :autoplay="true"  :dots="true">
                        <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.slug">
                            <div class="pi-pic">
                                <img  v-bind:src="itemProduct.galleries[0].photo" alt="" style="height:424px"/>
                                <ul>
                                    <li @click="saveCart(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo)" class="w-icon active">
                                        <a href="#"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/product/'+itemProduct.slug"> Quick View</router-link>
                                    </li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ itemProduct.type }}</div>
                                <router-link v-bind:to="'/product/'+itemProduct.slug">
                                    <h5>{{ itemProduct.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    Rp {{ itemProduct.price | numFormat }}
                                </div>
                            </div>
                        </div>
                        
                    </carousel>
                </div>

                <div class="col-lg-12" v-else>
                    <p>Produk terbaru belum tersedia untuk saat ini.</p>
                </div>
            </div>
        </div>-->
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
// import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "Product",
  components: {
    // carousel
  },
  data() {
    return {
      products: [],
      cart: [],
    };
  },
  mounted() {
    localStorage.setItem("searchbar", true);
    if (localStorage.getItem("search")) {
      const data = { value: localStorage.getItem("search") };
      axios
        .post("http://127.0.0.1:8000/api/products", data)
        .then((res) => {
          this.products = res.data.data;
          // localStorage.setItem("stok", res.data.data.quantity);
        })
        // eslint-disable-next-Line no-console
        .catch((err) => console.log(err));
      localStorage.removeItem("search");
    } else {
      axios
        .get("http://127.0.0.1:8000/api/products")
        .then((res) => {
          this.products = res.data.data.data;
          // localStorage.setItem("stok", res.data.data.quantity);
        })
        // eslint-disable-next-Line no-console
        .catch((err) => console.log(err));
    }

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
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: `http://127.0.0.1:8000/storage/` + photoProduct,
      };

      this.cart.push(productStored);
      const parsed = JSON.stringify(this.cart);
      localStorage.setItem("cart", parsed);

      window.location.reload();
    },
    onClick() {
      localStorage.removeItem("search");
    },
  },
};
</script>

<style scoped>
.product-item {
}
</style>
