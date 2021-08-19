<template>
    <!-- Header Section Begin -->
    <header class="header-section">
        <div class="header-top">
            <div class="container">
                <div class="ht-left">
                    <div class="mail-service">
                        <i class=" fa fa-envelope"></i> hello.kickssupply@gmail.com
                    </div>
                    <div class="phone-service">
                        <i class=" fa fa-phone"></i> +62 813 1957 3518
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="inner-header">
                <div class="row">
                    <div class="col-lg-2 col-md-2">
                        <div class="logo">
                            <router-link to="/">
                                <img src="img/logo_ks.png" alt="" />
                            </router-link>
                        </div>
                    </div>
                    <div class="col-lg-4 col-md-4 pl-5">
                        <form class="mt-2" action="" method="GET">
                            <div class="input-group">
                                <input class="form-control bg-light border-0 small" type="text" name="search" 
                                placeholder="Search Product Here..." aria-label="Search" aria-describedby="basic-addon">
                                <div class="input-group-append">
                                    <button class="btn btn-warning text-white" type="submit">
                                        <i class="icon_search_alt"></i>
                                    </button>
                                </div>
                            </div>
                            
                        </form>
                        
                    </div>
                    <!-- <div class="col-lg-4 col-md-7"></div> -->
                    <div class="col-lg-6 text-right col-md-3">
                        <ul class="nav-right">
                            <li class="cart-icon">
                                Keranjang Belanja &nbsp;
                                <a href="#">
                                    <i class="icon_bag_alt"></i>
                                    <span>{{ cart.length }}</span>
                                </a>
                                <div class="cart-hover">
                                    <div class="select-items">
                                        <table>
                                            <tbody v-if="cart.length > 0">
                                                <tr v-for="cartUser in cart" :key="cartUser.id">
                                                    <td class="si-pic">
                                                        <img class="photo-item" :src="cartUser.photo" alt=""/>
                                                    </td>
                                                    <td class="si-text">
                                                        <div class="product-selected">
                                                            <p>Rp {{ cartUser.price | numFormat }} x 1</p>
                                                            <h6>{{ cartUser.name }}</h6>
                                                        </div>
                                                    </td>
                                                    <td @click="removeItem(cartUser.id)" class="si-close">
                                                        <i class="ti-close"></i>
                                                    </td>
                                                </tr>
                                            </tbody>
                                            <tbody v-else> 
                                                <tr>
                                                    <td class="empty-cart">Keranjang anda masih kosong</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <div class="select-total">
                                        <span>total:</span>
                                        <h5>Rp {{ totalPrice | numFormat }}</h5>
                                    </div>
                                    <div class="select-button">
                                        <a href="#" class="primary-btn checkout-btn"><router-link to="/cart" style="color: #FFF">CHECK OUT</router-link></a>
                                        <!-- <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>-->
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <!-- Header End -->
</template>

<script>
export default {
    name: 'Header',
    data() {
        return {
            cart: []
        }
    },
    methods: {
        removeItem(idx) {
            // this.cart.splice(index, 1);
            // const parsed = JSON.stringify(this.cart);
            // localStorage.setItem('cart', parsed);

            //mencari id item yang ingin dihapus
            let cartStorage = JSON.parse(localStorage.getItem("cart"));
            let cartItemStorage = cartStorage.map(cartItemStorage => cartItemStorage.id);
            
            //mencocokkan id item yang ingin dihapus dengan id yang ada di storage
            let index = cartItemStorage.findIndex(id => id == idx);
            this.cart.splice(index, 1);

            const parsed = JSON.stringify(this.cart);
            localStorage.setItem("cart", parsed);
            window.location.reload();
            //eslint-disable-next-line no-console
            console.log(index);
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
    },
    computed: {
        totalPrice() {
            return this.cart.reduce(function(items, data) {
                return items + data.price;
            }, 0)
        }
    }
}
</script>

<style scoped>
    .photo-item {
        width: 70px;
    }
    .empty-cart {
        font-size: 16px;
        font-family: "Muli", sans-serif;
        color: #636363;
        font-weight: 400;
    }
</style>