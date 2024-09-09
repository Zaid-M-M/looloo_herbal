<template>
  <div>
    <SfFooter
      :column="4"
      multiple
      class="footer"
    >
      <SfFooterColumn>
        <div class="ftr_col_inner">
            <h4>Information</h4>
            <ul>
              <li>
                <a>About us</a>
              </li>
              <li>
                <a>Contact us</a>
              </li>
              <li>
                <a>Blog</a>
              </li>
              <li>
                <a>Returns & refunds policy</a>
              </li>
              <li>
                <a>Terms of service</a>
              </li>
              <li>
                <a>Privacy Statement</a>
              </li>
            </ul>
        </div>
        
      </SfFooterColumn>

      <SfFooterColumn>
        <div class="ftr_col_inner">
            <h4>Categories</h4>
            <ul>
              <li>
                <a>Oral Care</a>
              </li>
              <li>
                <a>Hair Care</a>
              </li>
              <li>
                <a>Joint Relief</a>
              </li>
              <li>
                <a>Massage Oil</a>
              </li>
              <li>
                <a>LooLoo Black Gold Kalonji Oil</a>
              </li>
              <li>
                <a>LooLoo Oleo Shifa Medicated Oil</a>
              </li>
              <li>
                <a>Cold & Headache</a>
              </li>
              <li>
                <a>Value Packs</a>
              </li>
            </ul>
        </div>
      </SfFooterColumn>

      <SfFooterColumn>
        <div class="ftr_col_inner">
          <h4>Customer Care</h4>
          <ul>
            <li>
              <a>Mon-Sat | 10 AM - 7 PM (IST)</a>
            </li>
            <li>
              <span>
                <img src="../static/footer/call.svg"/>
                <a href="tel:089283 66119">089283 66119 |</a>
                <a href="tel:091534 52452"> 091534 52452</a>
              </span>
            </li>
            <li>
              <span>
                <img src="../static/footer/mail.svg"/>
                <a href="mailto:info@loolooherbal.in">info@loolooherbal.in</a>
              </span>
            </li>
            <li>
              <span>
                <img src="../static/footer/location.svg"/>
                <a>KHOJATI HERBAL HEAD OFFICE 33/35 Bapu Khote Street, Jamli Mohallah, Mumbai-400003</a>
              </span>
              
            </li>
          </ul>
        </div>
        <div class="ftr_col_inner">
          <h4>Download our apps</h4>
          <div class="app_img_dv">
            <img src="../static/footer/android_app.png" />
            <img src="../static/footer/ios_app.png" />
          </div>
        </div>
      </SfFooterColumn>

      <SfFooterColumn>
        <div class="ftr_col_inner">
          <h4>Follow us</h4>
          <ul class="social_list">
            <li><img src="../static/footer/facebook.svg"/></li>
            <li><img src="../static/footer/instagram.svg"/></li>
            <li><img src="../static/footer/pinterest.svg"/></li>
            <li><img src="../static/footer/twitter.svg"/></li>
          </ul>
        </div>
        <div class="ftr_col_inner">
          <h4>Payment options</h4>
          <div class="app_img_dv">
            <img src="../static/footer/payment_gateways.gif" />
          </div>
        </div>
      </SfFooterColumn>
      <!-- <SfFooterColumn :title="$t('Information')">
        <SfList>
          <SfListItem
            v-for="item in aboutUs"
            :key="item"
          >
            <SfMenuItem
              :label="$t(item)"
            />
          </SfListItem>
        </SfList>
      </SfFooterColumn>
      <SfFooterColumn :title="$t('Categories')">
        <SfList>
          <SfListItem
            v-for="item in departments"
            :key="item"
          >
            <SfMenuItem
              :label="$t(item)"
            />
          </SfListItem>
        </SfList>
      </SfFooterColumn>
      <SfFooterColumn :title="$t('Customer Care')">
        <SfList>
          <SfListItem
            v-for="item in help"
            :key="item"
          >
            <SfMenuItem
              :label="$t(item)"
            />
          </SfListItem>
        </SfList>
      </SfFooterColumn>
      <SfFooterColumn :title="$t('Follow us')">
        <SfList>
          <SfListItem
            v-for="item in paymentsDelivery"
            :key="item"
          >
            <SfMenuItem
              :label="$t(item)"
            />
          </SfListItem>
        </SfList>
      </SfFooterColumn>
      <SfFooterColumn title="Social">
        <div class="footer__socials">
          <SfImage
            v-for="item in social"
            :key="item"
            class="footer__social-image"
            :src="addBasePath('/icons/'+item+'.svg')"
            :alt="item"
            width="32"
            height="32"
          />
        </div>
      </SfFooterColumn> -->
    </SfFooter>

    <div class="footer_bar">
      <div class="footer_bar_container">
        <p>© Looloo Herbal ALL RIGHTS RESERVED 2024.</p>
        <p>Website developed by <a href="https://www.parashifttech.com" target="_blank">Parashift Technologies</a></p>
      </div> 
    </div>


  </div>
</template>

<script lang="ts">
import {
  SfFooter, SfList, SfImage, SfMenuItem,
} from '@storefront-ui/vue';
import { defineComponent } from '@nuxtjs/composition-api';
import { addBasePath } from '~/helpers/addBasePath';

export default defineComponent({
  components: {
    SfFooter,
    SfList,
    SfImage,
    SfMenuItem,
  },
  setup() {
    return {
      addBasePath,
    };
  },
});
</script>

<style>
.sf-footer {
  background-color:#303030;
  padding: 26px 0 0 0!important;
  margin-top: 0px !important;
}
.sf-footer__container {
    max-width: 1400px !important;
    padding: 0 50px;
}
.sf-footer-column {
  background-color: transparent;
}
.sf-footer-column__content--hidden-on-mobile {
  --footer-column-content-display: unset;
}
.sf-footer-column button , .sf-footer-column__title{
  display: none;
}
.ftr_col_inner h4 {
  color: #f6e6ce;
  font-size: 14px;
  font-weight: 400;
  text-transform: uppercase;
  margin-bottom: 0px;
  margin-top: 20px;
  font-family: "Montserrat", sans-serif;
}
.ftr_col_inner ul {
  padding: 0px;
  list-style: none;
  margin: 0;
}
.ftr_col_inner ul li {
  padding: 3px 0 0;
}
.ftr_col_inner ul li a{
  color: #85868c;
  font-weight: 400;
  font-size: 14px;
  line-height: 25px;
  font-family: "Montserrat", sans-serif;
}
.app_img_dv img {
    margin-top: 3px;
}
.footer_bar {
  background-color:#303030;
}
.footer_bar_container {
  padding: 10px 0 10px 0;
  min-height: 60px;
  border-top: 1px solid #3d3d38;
  max-width: 1400px !important;
  padding: 0 50px;
  box-sizing: border-box;
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.footer_bar_container p {
  color: #ffffff;
  font-weight: 400;
  font-size: 13px;
  line-height: 23px;
  margin: 0;
  font-family: "Montserrat", sans-serif;
}
.footer_bar_container p  a {
  color: #ffffff;
}
ul.social_list {
    display: flex;
    gap: 14px;
}
</style>

<style lang="scss">

.footer {
  margin-bottom: 3.75rem;
  @include for-desktop {
    margin-bottom: 0;
  }
  &__socials {
    display: flex;
    justify-content: space-between;
    margin: 0 auto var(--spacer-lg);
    padding: var(--spacer-base) var(--spacer-xl);
    @include for-desktop {
      justify-content: flex-start;
      padding: var(--spacer-xs) 0;
      margin: 0 auto;
    }
  }
  &__social-image {
    margin: 0 var(--spacer-2xs) 0 0;
    background-color: #fff;
    border-radius: 100%;
  }
}
.sf-footer {
  @include for-desktop {
    padding-bottom: 0;
    margin-top: var(--spacer-2xl);
  }
  &__container {
    margin: var(--spacer-sm);
    @include for-desktop {
      max-width: 69rem;
      margin: 0 auto;
    }
  }
}
</style>
