<template>
    <!-- Hero Section Begin -->
    <section class="hero-section">
        <carousel class="hero-items" :items="1" :nav="false">

            <div class="single-hero-items set-bg" style="background-image: url('/img/hero_4.jpg')">
                <div class="container">
                    <div class="row">
                        <div class="col-lg-5">
                            <span>Sport, Men</span>
                            <h1>Black friday</h1>
                            <p>
                                Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore
                            </p>
                            <a href="#" class="primary-btn">Shop Now</a>
                        </div>
                    </div>
                </div>
            </div>
            <!--<div class="single-hero-items set-bg" style="background-image: url('/img/hero_2.jpg')">
                <div class="container">
                    <div class="row">
                        <div class="col-lg-5">
                            <span>Fashion, kids</span>
                            <h1>Sale Everyday</h1>
                            <p>
                                Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore
                            </p>
                            <a href="#" class="primary-btn">Shop Now</a>
                        </div>
                    </div>
                </div>
            </div>
            <div class="single-hero-items set-bg" style="background-image: url('/img/hero_3.jpg')">
                <div class="container">
                    <div class="row">
                        <div class="col-lg-5">
                            <span>Running, Women</span>
                            <h1>Black friday</h1>
                            <p>
                                Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore
                            </p>
                            <a href="#" class="primary-btn">Shop Now</a>
                        </div>
                    </div>
                </div>
            </div>-->
            
        </carousel>
    </section>
    <!-- Hero Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";

export default {
    name: 'Hero',
    components: {
        carousel
    }
}
</script>