<template>
  <div class="success">
    <div
      class="d-flex success-checkout align-items-center justify-content-center"
    >
      <div class="col col-lg-4 text-center">
        <img src="img/success-buy.png" alt="" width="294" />
        <h3 class="mt-4">
          Sukses Memesan!
        </h3>
        <p class="mt-2">
          Silakan konfirmasi pembayaran Anda ke admin via whatsapp.
        </p>
        <router-link to="/"
          ><a href="#" class="primary-btn pd-cart mt-3"
            >Kembali ke Home</a
          ></router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Success",
};
</script>
