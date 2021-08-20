<template>
  <!-- Partner Logo Section Begin -->
  <div class="partner-logo">
    <div class="container">
      <carousel
        class="logo-carousel"
        :loop="true"
        :autoplay="true"
        :items="5"
        :dots="false"
        :nav="false"
      >
        <div class="logo-item">
          <div class="tablecell-inner">
            <img src="img/logo-carousel/logo_1.png" alt="" />
          </div>
        </div>
        <div class="logo-item">
          <div class="tablecell-inner">
            <img src="img/logo-carousel/logo_2.png" alt="" />
          </div>
        </div>
        <div class="logo-item">
          <div class="tablecell-inner">
            <img src="img/logo-carousel/logo_3.png" alt="" />
          </div>
        </div>
        <div class="logo-item">
          <div class="tablecell-inner">
            <img src="img/logo-carousel/logo_4.png" alt="" />
          </div>
        </div>
        <div class="logo-item">
          <div class="tablecell-inner">
            <img src="img/logo-carousel/logo_5.png" alt="" />
          </div>
        </div>
      </carousel>
    </div>
  </div>
  <!-- Partner Logo Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";
export default {
  name: "Partner",
  components: {
    carousel,
  },
};
</script>
