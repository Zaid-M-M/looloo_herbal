<template>
  <div class="product">
    <SfLoader
      class="loading--product-gallery"
      :loading="isFetching"
    >
      <SfGallery
        :images="productGallery"
        :image-width="imageSizes.productGallery.imageWidth"
        :image-height="imageSizes.productGallery.imageHeight"
        :thumb-width="imageSizes.productGallery.thumbWidth"
        :thumb-height="imageSizes.productGallery.thumbHeight"
        :enable-zoom="true"
        image-tag="nuxt-img"
        thumb-image-tag="nuxt-img"
        class="product__gallery zaid_g"
        :nuxt-img-config="{
          fit: 'cover',
        }"
        :thumb-nuxt-img-config="{
          fit: 'cover',
        }"
      />
    </SfLoader>
    <div class="product__info">
      <div class="product__header">
        <SfHeading
          :title="getProductName(product)"
          :level="3"
          class="sf-heading--no-underline sf-heading--left"
        />
        <SvgImage
          icon="drag"
          width="40"
          height="40"
          class="product__drag-icon smartphone-only"
        />
      </div>
      <div class="product__price-and-rating">
        <SfPrice
          :regular="$fc(productPrice)"
          :special="productSpecialPrice && $fc(productSpecialPrice)"
        />

        <div>
          <div class="product__rating">
            <SfRating
              :score="averageRating"
              :max="5"
            />
            <a
              v-if="!!totalReviews"
              href="#"
              class="product__count"
            >
              ({{ totalReviews }})
            </a>
          </div>
          <SfButton
            class="sf-button--text"
            @click="setActiveTab(TabsConfig.reviews.ID)"
          >
            {{ $t('Read all reviews') }}
          </SfButton>
          |
          <SfButton
            class="sf-button--text"
            @click="openNewReviewTab"
          >
            {{ $t('Add a review') }}
          </SfButton>
        </div>
      </div>
      <div v-if="product !== null ">
        <SfAddToCart
          v-model="qty"
          v-e2e="'product_add-to-cart'"
        
          class="product__add-to-cart"
          @click="addItem({ product, quantity: parseInt(qty) })"
        >
          <template #add-to-cart-btn>
            <SfButton
              class="sf-add-to-cart__button"
        
              @click="addItem({ product, quantity: parseInt(qty) })"
            >
              {{ $t('Add to cart') }}
            </SfButton>
          </template>
        </SfAddToCart>

        <HTMLContent
          v-if="productShortDescription"
          :content="productShortDescription"
          tag="p"
          class="product__description desktop-only"
        />
        
        <SfAlert
          :style="{ visibility: !!addToCartError ? 'visible' : 'hidden'}"
          class="product__add-to-cart-error"
          :message="$t(addToCartError)"
          type="danger"
        />
        <div class="product__additional-actions">
          <AddToWishlist
            :is-in-wishlist="isInWishlist"
            :is-show="isAuthenticated"
            @addToWishlist="addItemToWishlist({product})"
          />
        </div>
      </div>
      <LazyHydrate when-idle>
        <ProductTabs
          :product="product"
          :open-tab="activeTab"
          @changeTab="setActiveTab($event)"
        />
      </LazyHydrate>
    </div>
  </div>
</template>
<script lang="ts">
import LazyHydrate from 'vue-lazy-hydration';
import {
  SfAddToCart,
  SfButton,
  SfGallery,
  SfHeading,
  SfLoader,
  SfPrice,
  SfRating,
  SfAlert,
} from '@storefront-ui/vue';
import {
  ref,
  computed,
  defineComponent,
  PropType,
  toRef,
} from '@nuxtjs/composition-api';

import {
  getName as getProductName,
  getPrice as getProductPrice,
} from '~/modules/catalog/product/getters/productGetters';

import {
  getTotalReviews,
  getAverageRating,
} from '~/modules/review/getters/reviewGetters';

import useWishlist from '~/modules/wishlist/composables/useWishlist';
import SvgImage from '~/components/General/SvgImage.vue';
import HTMLContent from '~/components/HTMLContent.vue';
import AddToWishlist from '~/components/AddToWishlist.vue';
import { useUser } from '~/modules/customer/composables/useUser';
import { Product } from '~/modules/catalog/product/types';
import { useCart } from '~/modules/checkout/composables/useCart';
import ProductTabs from '~/modules/catalog/product/components/tabs/ProductTabs.vue';
import { useProductGallery } from '~/modules/catalog/product/composables/useProductGallery';
import { TabsConfig, useProductTabs } from '~/modules/catalog/product/composables/useProductTabs';

export default defineComponent({
  name: 'SimpleProduct',
  components: {
    HTMLContent,
    LazyHydrate,
    SfAddToCart,
    SfButton,
    SfGallery,
    SfHeading,
    SfLoader,
    SfPrice,
    SfRating,
    SfAlert,
    AddToWishlist,
    SvgImage,
    ProductTabs,
  },
  transition: 'fade',
  props: {
    product: {
      type: [Object, null] as PropType<Product>,
      default: null,
    },
    isFetching: {
      type: Boolean,
      default: true,
    },
  },
  setup(props) {
    const qty = ref(1);
    const product = toRef(props, 'product');
    const {
      addItem, error: cartError, loading: isCartLoading, canAddToCart,
    } = useCart();
    const { productGallery, imageSizes } = useProductGallery(product);
    const { isAuthenticated } = useUser();
    const { addOrRemoveItem, isInWishlist } = useWishlist();
    const { activeTab, setActiveTab, openNewReviewTab } = useProductTabs();

    const productShortDescription = computed(
      () => props.product?.short_description?.html || '',
    );

    const productPrice = computed(() => getProductPrice(props.product).regular);
    const productSpecialPrice = computed(() => getProductPrice(props.product).special);
    const totalReviews = computed(() => getTotalReviews(props.product));
    const averageRating = computed(() => getAverageRating(props.product));
    const addToCartError = computed(() => cartError.value?.addItem?.message);

    return {
      addItem,
      addItemToWishlist: addOrRemoveItem,
      averageRating,
      totalReviews,
      canAddToCart,
      isAuthenticated,
      isInWishlist: computed(() => isInWishlist({ product: props.product })),
      isCartLoading,
      productShortDescription,
      productGallery,
      getProductName,
      productPrice,
      productSpecialPrice,
      qty,
      imageSizes,
      setActiveTab,
      openNewReviewTab,
      activeTab,
      TabsConfig,
      addToCartError,
    };
  },
});
</script>
<style>
.product {
  display: block !important;
  width: 100%;
  float: left;
  padding-top: 70px;
  /* background: blue; */
}
.product .loading--product-gallery {
  width: 40%;
  float: left;
  /* background: green; */
  padding-top: 0px !important;
}
.product .product__info {
  max-width: 55% !important;
  float: left;
  padding-left: 55px !important;
  /* background: aqua; */
}
.product .sf-gallery.product__gallery {
    display: block;
}
.product .sf-gallery__thumbs {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    gap:10px;
}
.product .sf-gallery__thumbs button {
    height: 122px !important;
    max-width: 122px !important;
}
.product .sf-gallery__thumbs button img {
  width: 100%;
}
.product .sf-gallery__stage {
    width: 100%;
    max-width: 100%;
    margin-bottom: 5px;
}
.product .sf-gallery__stage .sf-image--wrapper.sf-gallery__big-image {
  background-color: #fff;
}
.product .sf-gallery__stage .sf-image {
  width: 100%;
  object-fit: contain;
}
.product .product__price-and-rating {
    align-items: flex-end !important;
}
.product h3.sf-heading__title.h3 {
    color: #a18e11;
    font-size: 34px;
    line-height: 44px;
    font-weight: 600;
    font-family: 'Montserrat';
}
.product span.sf-price__regular {
    color: #a18e11;
    font-size: 40px;
    line-height: 30px;
    font-weight: 600;
    font-family: 'Montserrat';
}
.product button.sf-button--text.sf-button {
    font-family: 'Montserrat';
    color: #a18e11;
    font-weight: 600;
}
.product .product__description p {
    font-size: 15px;
    line-height: 25px;
    font-weight: 400;
    font-family: 'Montserrat';
    color: #6d6b5d;
}
.product .product__description p span.skutxt {
    color: #a5a5a5;
}
.sf-add-to-cart__select-quantity.sf-quantity-selector {
    border: 2px solid #a18e11;
    border-radius: 5px;
    background: transparent;
}
.sf-add-to-cart__select-quantity button.sf-quantity-selector__button:first-child {
  border-right: 2px solid #a18e11;
  border-left: 0px solid #a18e11;
}
.sf-add-to-cart__select-quantity button.sf-quantity-selector__button {
  border-left: 2px solid #a18e11;
  font-size: 30px;
  color: #a18e11;
  font-weight: 500;
  padding-top: 0;
}
.sf-input__wrapper input {
    color: #727272;
    font-size: 30px;
    font-weight: 300;
    font-family: 'Montserrat';
}
button.sf-add-to-cart__button.sf-button {
    background-color: #9f9f9f !important;
    text-transform: uppercase;
    border-radius: 5px;
    font-size: 20px;
    width: 178px;
    line-height: 20px;
    font-weight: 300;
    font-family: 'Montserrat';
}
.sf-add-to-cart.product__add-to-cart {
    margin-top: 20px !important;
    margin-left: 0 !important;
    margin-bottom: 42px !important;
}
</style>
<style lang="scss" scoped>
@import '../styles.scss';
</style>
