<template>
  <div class="cart">
    <Header />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more text-left">
              <a href="./home.html"><i class="fa fa-home"></i> Home</a>
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th class="p-name text-center">Qty</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="cartUser in cart" :key="cartUser.id">
                        <td class="cart-pic first-row">
                          <img :src="cartUser.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ cartUser.name }}</h5>
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ cartUser.jumlah }}</h5>
                        </td>
                        <td class="p-price first-row">
                          Rp {{ cartUser.price | numFormat }}
                        </td>
                        <td @click="removeItem(cart.index)" class="delete-item">
                          <a href="#">
                            <i class="material-icons">close</i>
                          </a>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8">
                <h4 class="mb-4 text-left">
                  Informasi Pembeli:
                </h4>
                <div class="user-checkout text-left">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        type="text"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                        v-model="customerInfo.name"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input
                        type="email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                        v-model="customerInfo.email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input
                        type="text"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                        v-model="customerInfo.number"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea
                        class="form-control"
                        id="alamatLengkap"
                        rows="3"
                        v-model="customerInfo.address"
                      ></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">
                      Subtotal <span>Rp {{ totalPrice | numFormat }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak 10%<span>Rp {{ totalTax | numFormat }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya <span>Rp {{ grandTotal | numFormat }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer <span>Mandiri</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening <span>2208 1996 1403</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima <span>Kicks Supply</span>
                    </li>
                  </ul>
                  <!-- <router-link to="/success"> -->
                  <a @click="checkout()" href="#" class="proceed-btn"
                    >I ALREADY PAID</a
                  >
                  <!-- </router-link> -->
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->
  </div>
</template>

<script>
import Header from "@/components/Header.vue";
import axios from "axios";

export default {
  name: "ShoppingCart",
  components: {
    Header,
  },
  data() {
    return {
      cart: [],
      customerInfo: {
        name: "",
        email: "",
        number: "",
        address: "",
      },
      uuid: "",
    };
  },
  methods: {
    removeItem(index) {
      this.cart.splice(index, 1);
      const parsed = JSON.stringify(this.cart);
      localStorage.setItem("cart", parsed);
    },
    //function mengirim data ke API
    checkout(uuid) {
      uuid =
        "TRX" +
        Math.floor(Math.random() * (99999 - 10000 + 1) + 10000) +
        Math.floor(Math.random() * (999 - 100 + 1) + 100);
      let productId = this.cart.map(function(product) {
        return product.id;
      });
      let productJumlah = this.cart.map(function(product) {
        return product.jumlah;
      });

      let checkoutData = {
        uuid,
        name: this.customerInfo.name,
        email: this.customerInfo.email,
        number: this.customerInfo.number,
        address: this.customerInfo.address,
        qty: productJumlah,
        transaction_total: this.grandTotal,
        transaction_status: "PENDING",
        transaction_details: productId,
      };
      axios
        .post("http://127.0.0.1:8000/api/checkout", checkoutData)
        .then(
          () =>
            window.open(
              `https://api.whatsapp.com/send?phone=6285921279924&text=Halo%2C+saya+sudah+melakukan+pembayaran+pembelian+buku+dengan+nomor+transaksi+${uuid}+Berikut+saya+lampirkan+foto+bukti+pembayaran`
            ),
          this.$router.push("success")
        )
        //eslint-disable-next-lint no-console
        .catch((err) => console.log(err));

      localStorage.removeItem("cart");
    },
  },
  mounted() {
    if (localStorage.getItem("cart")) {
      try {
        this.cart = JSON.parse(localStorage.getItem("cart"));
      } catch (e) {
        localStorage.removeItem("cart");
      }
    }
  },
  computed: {
    totalPrice() {
      return this.cart.reduce(function(items, data) {
        return items + data.price;
      }, 0);
    },
    totalTax() {
      return (this.totalPrice * 10) / 100;
    },
    grandTotal() {
      return this.totalPrice + this.totalTax;
    },
  },
};
</script>
