<template>
  <transition-group
    appear
    class="grid-layout"
    name="slide"
    tag="div"
  >
    <template v-if="loading">
      <div
        v-for="n in 3*2"
        :key="n"
        class="sf-product-card card"
        data-testid="skeleton"
      >
        <SkeletonLoader :height="`${imageSize.height}px`" />
        <SkeletonLoader />
        <SkeletonLoader />
      </div>
    </template>
    <template v-else>
      <SfProductCard
        v-for="product in productsWithCommonProductCardProps"
        :key="product.uid"
        v-bind="product.commonProps"
        class="card prd_box"
        data-testid="product-card"
        :image-height="imageSize.height"
        :image-width="imageSize.width"
        :show-add-to-cart-button="true"
        @click:wishlist="$emit('click:wishlist', product)"
        @click:add-to-cart="$emit('click:add-to-cart', { product, quantity: 1 })"
      >
        <template #price>
          <CategoryProductPrice :product="product" />
        </template>
      </SfProductCard>
    </template>
  </transition-group>
</template>

<script lang="ts">
import { defineComponent, PropType, toRefs } from '@nuxtjs/composition-api';
import { SfProductCard } from '@storefront-ui/vue';
import { useImage } from '~/composables';
import type { Product } from '~/modules/catalog/product/types';

import SkeletonLoader from '~/components/SkeletonLoader/index.vue';
import CategoryProductPrice from '~/modules/catalog/category/components/views/CategoryProductPrice.vue';
import { useProductsWithCommonProductCardProps } from './useProductsWithCommonCardProps';

export default defineComponent({
  components: {
    CategoryProductPrice,
    SfProductCard,
    SkeletonLoader,
  },
  props: {
    products: {
      type: Array as PropType<Product[]>,
      required: true,
    },
    pricesLoaded: Boolean,
    loading: Boolean,
  },
  emits: ['click:wishlist', 'click:add-to-cart'],
  setup(props) {
    const { imageSizes: { productCard: imageSize } } = useImage();
    const { products } = toRefs(props);
    const { productsWithCommonProductCardProps } = useProductsWithCommonProductCardProps(products);

    return {
      imageSize,
      productsWithCommonProductCardProps,
    };
  },
});
</script>

<style>
  .prd_box {
    box-shadow: 0px 0px 4px 0px #3a2c0c3b;
    border-radius: 5px;
    padding: 0;
    width: 100% !important;
    max-width: 31%;
    background-color: #eae9c9;
  }
  .prd_box .sf-product-card__image-wrapper {
      width: 100%;
      height: auto !important;
  }
  .prd_box .sf-product-card__image img {
      width: 100% !important;
      height: auto !important;
      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
  }
  .prd_box a.sf-product-card__link.sf-button:first-child {
    padding: 0px;
  }
  .prd_box a.sf-product-card__link.sf-button , .prd_box .product-price , .prd_box .sf-product-card__reviews {
    padding: 0 12px 12px 12px;
    display: block;
  }
  .prd_box a span.sf-product-card__title {
    font-size: 16px;
    line-height: 26px;
    color: #a17b20;
    font-family: 'Montserrat';
    text-overflow: unset !important;
    overflow: auto;
    white-space: normal;
    text-align: center;
}
.prd_box .sf-product-card__price.sf-price , .prd_box .sf-product-card__rating.sf-rating {
    justify-content: center;
}
.prd_box span.sf-price__regular {
  font-size: 24px;
  line-height: 26px;
  color:#a18e11;
  font-family: 'Montserrat';
  font-weight: 400;
}
.prd_box .sf-rating__icon--negative {
    --icon-color: rgba(161, 142, 17, 0.2);
}
</style>
<style lang="scss" scoped>
.products .grid-layout {
    gap: 32px;
    margin-top: 0;
    margin-left: 20px;
}
@import "./transition.scss";
.grid-layout {
  display: flex;
  flex-wrap: wrap;
  align-content: flex-start;
  justify-content: center;
  background: #F5F1DD;
  @include for-desktop {
    justify-content: flex-start;
    margin: var(--spacer-sm) 0 0 var(--spacer-sm);
  }
}


.card {
  --product-card-title-font-weight: var(--font-weight--medium);
  --product-card-title-margin: var(--spacer-xs) 0 0 0;
  flex: 1 1 100%;

  @include for-desktop {
    --product-card-max-width: 25%;
    --product-card-title-font-weight: var(--font-weight--normal);
    --product-card-title-margin: var(--spacer-sm) 0 0 0;
    --product-card-add-button-bottom: var(--spacer-base);
  }
}

::v-deep .sf-product-card {
  .card {
    will-change: transform, opacity;
  }

  &__image-wrapper {
    height: 257px;
  }

  &__add-button {
    @include for-mobile {
      opacity: 1;
      display: flex;
      bottom: 1rem;
      right: 0;
    }
  }
}

::v-deep .sf-product-card::after {
  content: none;
}

</style>
