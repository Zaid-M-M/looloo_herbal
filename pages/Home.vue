<template>
  <div id="home" class="">
    <!-- Slider -->
    <div class="slideshow-container">
      <div
        v-for="(slide, index) in slides"
        :key="index"
        class="mySlides fade"
        v-show="index === slideIndex"
      >
        <img :src="slide.image" style="width:100%" class="slideImg">
      </div>
      <a class="prev" @click="plusSlides(-1)">❮</a>
      <a class="next" @click="plusSlides(1)">❯</a>
    </div>
    
    <!-- Home Feature -->
    <div class="homeFeaturedSec ">
        <h1 class="homeFeatureHeading">Featured Product Range</h1>


        <div class="featureProCon">
            <div class="featureProduct ">
                <div class="proDetail">
                    <div class="left ">
                        <img src="https://loolooherbal.in/wp-content/uploads/2019/05/LooLoo_herbal_Dental_care_ribbon.png" alt="">
                        <div class="xyz">
                            <h2>
                                Herbal Dental Gels & Powders
                            </h2>
                            <p>Looloo offer a range which is 100% vegetarian. The tooth products do not contain components, such as calcium, derived from animals.  
                            </p>
                            <button>Shop Now</button>
                        </div>
                    </div>
                    <div class="right">
                        <img src="https://loolooherbal.in/wp-content/uploads/2021/10/Looloo_herbal_Featured_Product_Range_oral_care_.png" alt="" >
                    </div>
                </div>
            </div>
            <div class="featureProduct ">
                <div class="proDetail">
                    <div class="left ">
                        <img src="https://loolooherbal.in/wp-content/uploads/2019/06/LooLoo_herbal_oleo_rehuma_ribbon.png" alt="">
                        <div class=" xyz">
                            <h2>
                              Oleo Rheuma
                            </h2>
                            <p>Looloo Oleo Rheuma, a herbal liniment, not only takes over the pain, but also heals the inflamed joints and muscles. 
                            </p>
                            <button>Shop Now</button>
                        </div>
                    </div>
                    <div class="right">
                        <img src="https://loolooherbal.in/wp-content/uploads/2021/10/Looloo_herbal_Featured_Product_Range_Oleo_rehuma.png" alt="" >
                    </div>
                </div>
            </div>
            <div class="featureProduct ">
                <div class="proDetail">
                    <div class="left ">
                        <img src="https://loolooherbal.in/wp-content/uploads/2019/06/LooLoo_herbal_oleo_rehuma_ribbon.png" alt="">
                        <div class=" xyz">
                            <h2>
                              revitalise your hair
                            </h2>
                            <p>Mumtaz Amla Hair Oil: Contains the natural goodness of amla (L. Phyllanthus emblica) Jasmine and Lemon. 
                            </p>
                            <button>Shop Now</button>
                        </div>
                    </div>
                    <div class="right">
                        <img src="https://loolooherbal.in/wp-content/uploads/2021/10/Looloo_herbal_Featured_Product_Range_revitalise_your_hair_2.png" alt="" >
                    </div>
                </div>
            </div>
            <div class="featureProduct ">
                <div class="proDetail">
                    <div class="left ">
                        <img src="https://loolooherbal.in/wp-content/uploads/2019/06/LooLoo_herbal_oleo_rehuma_ribbon.png" alt="">
                        <div class=" xyz">
                            <h2>
                              Oleo Cold Shield
                            </h2>
                            <p>Looloo Oleo Cold Shield contains the valuable extracts of Kalonji and other essential oils to give instant relief from cold and headache.  
                            </p>
                            <button>Shop Now</button>
                        </div>
                    </div>
                    <div class="right">
                        <img src="https://loolooherbal.in/wp-content/uploads/2021/10/looloohearble_home_cold_shield.png" alt="" >
                    </div>
                </div>
            </div>
          
        </div>

    </div>


    <!-- Free Shipping Img -->
    <div class="freeShip">
      <img src="https://loolooherbal.in/wp-content/uploads/2024/03/Rs-700-Offer-Big-Banner.jpg" alt="">
    </div>



    <!-- LooLoo Herbal Commitment -->
     <div class="commitmentCon">
      <div class="container">

        <h1>LooLoo Herbal Commitment</h1>
        <p>LooLoo Herbal is a progressive, research based health and wellness organisation.
         <br>We develop innovative and natural products that are imbibed with the essence of Unani medicine.</p>
          <div class="">
            <img src="https://loolooherbal.in/wp-content/uploads/2021/04/LooLoo_herble_badges_hp.png" alt="">  
          </div>

      </div>
     </div>



    <!-- Our Blogs -->
     <div class="blogsMainSec ">
      <h1>Our Blogs</h1>
      <div class="container">
        <div class="blogsCon">
          <div class="singleBlog">

            <div class="blogImgCon">
              <img src="https://loolooherbal.in/blog/wp-content/uploads/2021/09/LooLoo_herble_Is_Working_from_Home_Cover_image.jpg" alt="">
            </div>
            <div class="blogContent">
              <h2 class="blogHeading">Is Working from Home Damaging Your Body?</h2>
              <p class="blogPara">This pandemic has been a true test of patience. The possibility of resurrection of the normal, simple, pleasant way of life seems to be far-fetched with the increasing chaos and uncertainty.</p>
              <p><a href="">Read More</a></p>

            </div>
          </div>
          <div class="singleBlog">

            <div class="blogImgCon">
              <img src="https://loolooherbal.in/blog/wp-content/uploads/2021/03/LooLoo_herble_ZULF_main_image_blog.jpg" alt="">
            </div>
            <div class="blogContent">
              <h2 class="blogHeading">Why It’s Important to Stay Cool in Summers</h2>
              <p class="blogPara">This summer, dodge the intolerant heat with LooLoo Roshan-e-Zulf. Its ultra-cool minty freshness is a quick getaway from unbearable heat to a refreshing burst of ice.</p>
              <p><a href="">Read More</a></p>

            </div>
          </div>
          <div class="singleBlog">

            <div class="blogImgCon">
              <img src="https://loolooherbal.in/blog/wp-content/uploads/2021/02/LooLoo_herble_cold_shield_header_image.jpg" alt="">
            </div>
            <div class="blogContent">
              <h2 class="blogHeading">Treating Common Cold in Winters</h2>
              <p  class="blogPara" >Breathe easy with a blast of freshness with LooLoo Oleo Cold Shield Roll-On Oil – Unique amalgamation of essential oils that grants Instant Relief from congestion and headache.</p>
              <p><a href="">Read More</a></p>

            </div>
          </div>
        </div>

      </div>
     </div>

  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  onMounted,
  onBeforeUnmount
} from '@nuxtjs/composition-api';

export default defineComponent({
  name: 'HomePage',
  data() {
    return {
      slideIndex: 0 as number,
      slides: [
        { image: "/homeSlider/Dhamaka-Offer-Banner.jpg", caption: "Caption Text" },
        { image: "/homeSlider/02-Oleo-Rheuma-Gel.jpg", caption: "Caption Two" },
        { image: "/homeSlider/03-Cold-Shield.jpg", caption: "Caption Three" },
        { image: "/homeSlider/04-Oleo-Rheuma-Oil.jpg", caption: "Caption Four" },
        { image: "/homeSlider/05-Aqua-Mint.jpg", caption: "Caption Five" },
        { image: "/homeSlider/06-Charcoal.jpg", caption: "Caption Six" },
        { image: "/homeSlider/07-Dental-GEL.jpg", caption: "Caption Seven" },
        { image: "/homeSlider/08-Shifa.jpg", caption: "Caption Eight" },
        { image: "/homeSlider/09-Amla.jpg", caption: "Caption Nine" },
      ] as Array<{ image: string, caption: string }>,

      autoSlideInterval: null as ReturnType<typeof setInterval> | null,
    };
  },
  methods: {
    plusSlides(n: number) {
      this.slideIndex = (this.slideIndex + n + this.slides.length) % this.slides.length;
    },
    startAutoSlide() {
      this.autoSlideInterval = setInterval(() => {
        this.plusSlides(1);
      }, 4000);
    },
    stopAutoSlide() {
      if (this.autoSlideInterval) {
        clearInterval(this.autoSlideInterval);
      }
    }
  },
  mounted() {
    this.startAutoSlide();
  },
  beforeDestroy() {
    this.stopAutoSlide();
  },
});
</script>

<style lang="scss" scoped>
// --------------- Style start ---------------
.mySlides {
  /* display: none; */
  height: 500px;
}
.mySlides img {
  vertical-align: middle;
  height: 100%;
  object-fit: cover;
}
/* Slideshow container */
.slideshow-container {
  max-width: 100%;
  position: relative;
  margin: auto;
}
/* Next & previous buttons */
.prev,
.next {
  cursor: pointer;
  position: absolute;
  top: 50%;
  width: auto;
  padding: 16px;
  margin-top: -22px;
  color: white;
  font-weight: bold;
  font-size: 18px;
  transition: 0.6s ease;
  border-radius: 0 3px 3px 0;
  user-select: none;
}
/* Position the "next button" to the right */
.next {
  right: 0;
  border-radius: 3px 0 0 3px;
}
/* Fading animation */
.fade {
  animation-name: fade;
  animation-duration: 1.5s;
}
@keyframes fade {
  from {
    opacity: 0.4;
  }
  to {
    opacity: 1;
  }
}
/* On smaller screens, decrease text size */
@media only screen and (max-width: 300px) {
  .prev,
  .next,
  .text {
    font-size: 11px;
  }
}
.silder {

  img{
    width: 100%;
    height: auto;
  }
}

.container {
  max-width: 1400px;
  padding: 0 44px;
  width: 100%;
  margin: 0 auto;
  box-sizing: border-box;
}

h1 {
    text-align: center;
    font-family: Montserrat, Helvetica, Arial, Verdana, sans-serif;
    color: rgb(161, 142, 17);
    font-weight: 600;
    font-size: 30px;
    line-height: 35px;
    margin: 33px 0;
  }

.homeFeaturedSec {
  max-width: 1400px;
  padding: 0 44px;
  margin: 0 auto;
  box-sizing: border-box;

  h1{
    margin: 0;
    padding: 33px 0;
  }

 
}



.proDetail {
  height: 330px;
  background-color: #fff;
  box-shadow: 0px 0px 18px -8px rgba(0,0,0,0.41);
}

.featureProCon {
  .featureProduct {
    height: 400px;
  }
}

.proDetail .left h2{
  font-family: 'Oswald', sans-serif;
    line-height: 38px;
    font-weight: 400;
    text-transform: uppercase;
}


.xyz {
  padding-left: 20px;
  padding-top: 20px;
  padding-bottom: 30px;

  button{
    border-width: 0px;
    color: #ffffff;
    background: #05923f !important;
    padding: 9px 15px 9px 15px;
    font-size: 18px;
    line-height: 20px;
    border-radius: 4px;
  }
}

.b{
  border: 1px solid red;
}


.freeShip img{
  height: auto;
  width: 100%;
}


.commitmentCon{
  background-image: url('https://loolooherbal.in/wp-content/uploads/2019/05/LooLoo_herbal_benefis_bg.jpg');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: 50% 100%;
    background-color: rgba(0, 0, 0, 0);
    background-attachment: scroll;
    height: 358px;
  
}

.commitmentCon .container{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100%;


    p{
      text-align: center;
      margin-bottom: 30px;
    }


    h1{
      margin: 0 0 0 20px;
    }
    
}


// blog

.blogsMainSec{
  background-color: #ffffff;

  h1{
    margin: 0;
    padding: 44px 0;
  }




  .blogsCon{
    display: flex;
    justify-content: space-between;



    .singleBlog{
      width: 380px;
      


      p{
        font-size: 16px;
        font-weight: 400;
        line-height: 26px;
        color: #6d6b5d ;
      }

      .blogImgCon {
        height: auto;

        img{
          width: 100%;
          height: auto;
        }
      }


      .blogContent{
        background-color: #fff;
        display: flex;
        flex-direction: column;
       


        p{
          margin: 0;
        }

        .blogPara{
          margin: 10px 0;
        }
     



        a{
          font-weight: 600;
          color: #a18e11;
          border-bottom: 1px solid ;
        }
      }

      

      .blogHeading{
        font-size: 20px;
        font-weight: 600;
        line-height: 30px;
        color: rgb(161, 142, 17);
        font-family: "Montserrat", sans-serif;
      }
    }
  }
}




@media only screen and (min-device-width: 768px) {
  .featureProCon {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;

    .featureProduct {
      width: 48%;
      height: 365px;
      margin-bottom: 35px;
      position: relative;
    }
  }

  .proDetail {
    display: flex;
    justify-content: space-between;
    
    

    .left {
      width: 50%;
      padding-top: 20px;

    }

    .right {
      width: 50%;
      display: flex;
      align-items: end;
      justify-content: end;


      img {
        position: absolute;
        top: auto;
        bottom: 0;
        right: 15px;
      }
    }
  }
}



// --------------- Style End -----------------




// Media Query 





@media only screen and (min-device-width: 320px) and (max-device-width: 767px) {
  .homeFeaturedSec{
    padding: 0;
  }

  .featureProduct{
    margin: 20px 0px;
    border: 1px solid red;
    height: auto;
  }



  .proDetail{
    height: auto;
    padding: 20px 0px;
  }

  .right{
    height: 275px;
    display: flex;
    justify-content: center;
    align-items: center
  }


  .xyz{
    padding-bottom: 0px;
  }


  .featureProCon .featureProduct{
    height: auto;
  }

 
}

.blogsMainSec  {
  padding-bottom: 50px;
}

@media only screen and (min-device-width: 768px) and (max-device-width: 991px) {
  // Styles for Tablet (768px devices)
}


@media only screen and (min-device-width: 992px) and (max-device-width: 1199px) {
  // Styles for Laptop (1024px devices)
}


@media only screen and (min-device-width: 1200px) and (max-device-width: 1280px) {
  // Styles for devices between 1200px and 1280px
}

@media only screen and (min-device-width: 1281px) and (max-device-width: 1369px) {
  // Styles for devices between 1281px and 1369px
}

@media only screen and (min-device-width: 1370px) and (max-device-width: 1440px) {
  // Styles for devices between 1370px and 1440px
}

@media only screen and (min-device-width: 1441px) and (max-device-width: 1536px) {
  // Styles for devices between 1441px and 1536px
}

@media only screen and (min-device-width: 1537px) and (max-device-width: 1600px) {
  
}




.article-meta h4 a {
  color: #111111;
  font-weight: 600;
  font-size: 20px;
}

.article-meta {
  margin-top: 10px;
}

.article-item__meta-item:not(:last-child)::after {
  display: inline-block;
  content: '';
  width: 5px;
  height: 5px;
  margin: -1px 10px 0 10px;
  border-radius: 100%;
  background: rgba(0, 0, 0, 0.4);
  vertical-align: middle;
}

#home {
  box-sizing: border-box;
  padding: 0 var(--spacer-sm);
  @include for-desktop {
    max-width: 100%;
    padding: 0;
    margin: 0 auto;
  }
}

.hero-section {
  margin: var(--spacer-xl) auto var(--spacer-lg);

  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }

  @include for-desktop {
    margin: var(--spacer-xl) auto var(--spacer-2xl);
  }
}

.banner-grid {
  --banner-container-width: 50%;
  margin: var(--spacer-xl) 0;

  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }

  @include for-desktop {
    margin: var(--spacer-2xl) 0;
    ::v-deep .sf-link {
      --button-width: auto;
    }
  }
}

.banner {
  &__tshirt {
    background-position: left;
  }

  &-central {
    @include for-desktop {
      --banner-container-flex: 0 0 70%;
    }
  }
}

.similar-products {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: var(--spacer-2xs);
  --heading-padding: 0;
  border-bottom: 1px var(--c-light) solid;
  @include for-desktop {
    border-bottom: 0;
    justify-content: center;
    padding-bottom: 0;
  }
}

.call-to-action {
  background-position: right;
  margin: var(--spacer-xs) 0;
  @include for-desktop {
    margin: var(--spacer-xl) 0 var(--spacer-2xl) 0;
  }
}

.products {
  margin-top: var(--spacer-base);
}

.carousel {
  margin: 0 calc(-1 * var(--spacer-sm)) 0 0;
  @include for-desktop {
    margin: 0;
  }

  &__item {
    margin: 1.375rem 0 2.5rem 0;
    @include for-desktop {
      margin: var(--spacer-xl) 0 var(--spacer-xl) 0;
    }

    &__product {
      --product-card-add-button-transform: translate3d(0, 30%, 0);
    }
  }
}
</style>
