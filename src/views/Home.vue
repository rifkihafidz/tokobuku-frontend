<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Header/>  
    <Hero/>
    <Woman/>
    <Insta/>
    <Partner/>
    <Footer/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
 import Header from '@/components/Header.vue'
 import Hero from '@/components/Hero.vue'
 import Woman from '@/components/Product.vue'
 import Insta from '@/components/Insta.vue'
 import Partner from '@/components/Partner.vue'
 import Footer from '@/components/Footer.vue'
 

export default {
  name: 'Home',
  components: {
    Header,
    Hero,
    Woman,
    Insta,
    Partner,
    Footer
  }
}
</script>
