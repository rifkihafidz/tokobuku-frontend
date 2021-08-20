<template>
  <!-- Hero Section Begin -->
  <section class="hero-section">
    <carousel class="hero-items" :items="1" :nav="false">
      <div
        class="single-hero-items set-bg"
        style="background-image: url('/img/heho_2.jpg')"
      >
        <div class="container">
          <div class="row">
            <div class="col-lg-6">
              <h1>Books Supply</h1>
              <p>
                Menyediakan berbagai macam buku dengan harga yang terjangkau.
              </p>
              <a href="#" class="primary-btn">Beli Sekarang</a>
            </div>
          </div>
        </div>
      </div>
      <div
        class="single-hero-items set-bg"
        style="background-image: url('/img/heho_1.jpg')"
      >
        <div class="container">
          <div class="row">
            <div class="col-lg-6">
              <h1>Books Supply</h1>
              <p>
                Menyediakan berbagai macam buku dengan harga yang terjangkau.
              </p>
              <a href="#" class="primary-btn">Beli Sekarang</a>
            </div>
          </div>
        </div>
      </div>
      <div
        class="single-hero-items set-bg"
        style="background-image: url('/img/hero_4.jpg')"
      >
        <div class="container">
          <div class="row">
            <div class="col-lg-6">
              <h1>Books Supply</h1>
              <p>
                Menyediakan berbagai macam buku dengan harga yang terjangkau.
              </p>
              <a href="#" class="primary-btn">Beli Sekarang</a>
            </div>
          </div>
        </div>
      </div>
    </carousel>
  </section>
  <!-- Hero Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";

export default {
  name: "Hero",
  components: {
    carousel,
  },
};
</script>
