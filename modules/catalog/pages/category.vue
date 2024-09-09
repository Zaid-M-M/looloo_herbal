<template>
  <div id="category">
    <CmsContent
      v-if="isShowCms"
      :content="cmsContent"
    />
    
    <SkeletonLoader
      v-if="!activeCategoryName"
      height="57px"
      width="200px"
      margin="0"
    />
    
    <!-- Display Category Image -->
    <div v-if="activeCategoryImage" class="category-image">
      <img :src="activeCategoryImage" :alt="activeCategoryName" />
    </div>
    <!-- <CategoryBreadcrumbs /> -->
    <!-- <SfHeading
      v-else
      :level="2"
      :title="activeCategoryName"
      class="category-title"
    /> -->

    <div class="category-layout">
      <p class="category_breadcrumb"></p>
      <div class="sidebar column">
        <!-- <CategoryFilters
          v-if="isShowProducts"
          class="mobile-only"
          :is-visible="isFilterSidebarOpen"
          :cat-uid="routeData.uid"
          @close="toggleFilterSidebar"
          @reloadProducts="onReloadProducts"
        /> -->
        <div class="category_sidebar_inner">
          <div class="category_list">
            <h3>Categories</h3>
            <ul>
              <li><a href='#'>Oral Care</a></li>
              <li><a href='#'>Hair Care</a></li>
              <li><a href='#'>Joint Relief</a></li>
              <li><a href='#'>Massage Oil</a></li>
              <li><a href='#'>LooLoo Black Gold Kalonji Oil</a></li>
              <li><a href='#'>LooLoo Oleo Shifa Medicated Oil</a></li>
              <li><a href='#'>Cold & Headache</a></li>
            </ul>
          </div>
          <a href="#" target="_blank" class="catalogue_img">
            <img src='../../../static/category/download_catalogue.png' />
          </a>
        </div>
        
      </div>
      <div
        ref="productContainerElement"
        class="main section column"
      >
        <!-- <CategoryNavbar
          v-if="isShowProducts"
          :sort-by="sortBy"
          :pagination="pagination"
          :is-loading="$fetchState.pending"
          @reloadProducts="onReloadProducts"
        /> -->
        <div class="products">
          <CategoryEmptyResults v-if="products.length === 0 && !$fetchState.pending && isShowProducts" />

          <CategoryProductGrid
            v-if="isCategoryGridView"
            :products="products"
            :prices-loaded="isPriceLoaded"
            :loading="$fetchState.pending"
            @click:wishlist="addItemToWishlist"
            @click:add-to-cart="addItemToCart"
          />

          <CategoryProductList
            v-else
            :products="products"
            :prices-loaded="isPriceLoaded"
            :loading="$fetchState.pending"
            @click:wishlist="addItemToWishlist"
            @click:add-to-cart="addItemToCart"
          />
          <div
            v-if="!$fetchState.pending"
            class="products__display-opt"
          >
            <LazyHydrate when-visible>
              <CategoryPagination
                v-show="pagination.totalPages > 1"
                :current="pagination.currentPage"
                :total="pagination.totalPages"
                :visible="5"
                class="products__pagination"
                @click="goToPage($event)"
              />
            </LazyHydrate>

            <div
              v-show="pagination.totalPages > 1"
              class="products__show-on-page"
            >
              <span class="products__show-on-page__label">{{
                $t('Show')
              }}</span>
              <LazyHydrate when-visible>
                <SfSelect
                  :value="pagination.itemsPerPage.toString()"
                  class="products__items-per-page"
                  @input="doChangeItemsPerPage"
                >
                  <SfSelectOption
                    v-for="option in pagination.pageOptions"
                    :key="option"
                    :value="option"
                    class="products__items-per-page__option"
                  >
                    {{ option }}
                  </SfSelectOption>
                </SfSelect>
              </LazyHydrate>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import LazyHydrate from 'vue-lazy-hydration';
import {
  SfSelect,
  SfHeading,
} from '@storefront-ui/vue';
import {
  computed,
  defineComponent,
  onMounted,
  ref,
  ssrRef,
  useFetch,
} from '@nuxtjs/composition-api';
import { CacheTagPrefix, useCache } from '@vue-storefront/cache';
import { usePageStore } from '~/stores/page';
import SkeletonLoader from '~/components/SkeletonLoader/index.vue';
import CategoryPagination from '~/modules/catalog/category/components/pagination/CategoryPagination.vue';
import {
  useCategory,
  useFacet,
  useUiHelpers,
  useUiState,
} from '~/composables';

import { useAddToCart } from '~/helpers/cart/addToCart';
import { useWishlist } from '~/modules/wishlist/composables/useWishlist';
import { usePrice } from '~/modules/catalog/pricing/usePrice';
import { useCategoryContent } from '~/modules/catalog/category/components/cms/useCategoryContent';
import { useTraverseCategory } from '~/modules/catalog/category/helpers/useTraverseCategory';
import facetGetters from '~/modules/catalog/category/getters/facetGetters';
import { getMetaInfo } from '~/helpers/getMetaInfo';

import CategoryNavbar from '~/modules/catalog/category/components/navbar/CategoryNavbar.vue';
import CategoryBreadcrumbs from '~/modules/catalog/category/components/breadcrumbs/CategoryBreadcrumbs.vue';

import type { ProductInterface } from '~/modules/GraphQL/types';
import type { SortingModel } from '~/modules/catalog/category/composables/useFacet/sortingOptions';
import type { Pagination } from '~/composables/types';
import type { Product } from '~/modules/catalog/product/types';

export default defineComponent({
  name: 'CategoryPage',
  components: {
    CategoryPagination,
    CategoryEmptyResults: () => import('~/modules/catalog/category/components/CategoryEmptyResults.vue'),
    CategoryFilters: () => import('~/modules/catalog/category/components/filters/CategoryFilters.vue'),
    CmsContent: () => import('~/modules/catalog/category/components/cms/CmsContent.vue'),
    CategoryProductGrid: () => import('~/modules/catalog/category/components/views/CategoryProductGrid.vue'),
    CategoryProductList: () => import('~/modules/catalog/category/components/views/CategoryProductList.vue'),
    CategoryNavbar,
    CategoryBreadcrumbs,
    SfSelect,
    LazyHydrate,
    SfHeading,
    SkeletonLoader,
  },
  transition: 'fade',
  setup() {
    const { routeData } = usePageStore();
    const { getContentData } = useCategoryContent();
    const { loadCategoryMeta } = useCategory();
    const { addTags } = useCache();
    const uiHelpers = useUiHelpers();
    const cmsContent = ref('');
    const isShowCms = ref(false);
    const isShowProducts = ref(false);
    const products = ssrRef<ProductInterface[]>([]);
    const sortBy = ref<SortingModel>({ selected: '', options: [] });
    const pagination = ref<Pagination>({});

    const productContainerElement = ref<HTMLElement | null>(null);

    const {
      toggleFilterSidebar,
      changeToCategoryListView,
      changeToCategoryGridView,
      isCategoryGridView,
      isFilterSidebarOpen,
    } = useUiState();
    const {
      load: loadWishlist,
      addItem: addItemToWishlistBase,
      isInWishlist,
      removeItem: removeItemFromWishlist,
    } = useWishlist();
    const { result, search } = useFacet();
    const { addItemToCart } = useAddToCart();

    const categoryMeta = ref(null);
    const categoryImage = ref<string | null>(null); // Declare and initialize categoryImage

    const addItemToWishlist = async (product: Product) => {
      await (isInWishlist({ product })
        ? removeItemFromWishlist({ product })
        : addItemToWishlistBase({ product }));
    };

    const { activeCategory, loadCategoryTree } = useTraverseCategory();

    const activeCategoryName = computed(() => activeCategory.value?.name ?? '');
    const activeCategoryImage = computed(() => activeCategory.value?.image ?? '');

    const categoryUid = routeData.uid;

    const { fetch } = useFetch(async () => {
      if (!activeCategory.value) {
        await loadCategoryTree();
      }
      // console.log('Active Category:', activeCategory.value);

      const [content, categoryMetaData] = await Promise.all([
        getContentData(categoryUid as string),
        loadCategoryMeta({ category_uid: routeData.value?.uid }),
        search({ ...uiHelpers.getFacetsFromURL(), category_uid: categoryUid }),
      ]);

      categoryMeta.value = categoryMetaData;
      cmsContent.value = content?.cmsBlock?.content ?? '';
      isShowCms.value = content.isShowCms;
      isShowProducts.value = content.isShowProducts;

      products.value = facetGetters.getProducts(result.value) ?? [];
      sortBy.value = facetGetters.getSortOptions(result.value);
      pagination.value = facetGetters.getPagination(result.value);

      const tags = [{ prefix: CacheTagPrefix.View, value: routeData.uid }];
      const productTags = products.value.map((product) => ({
        prefix: CacheTagPrefix.Product,
        value: product.uid,
      }));

      addTags([...tags, ...productTags]);
    });

    const isPriceLoaded = ref(false);

    onMounted(async () => {
      loadWishlist();
      const { getPricesBySku } = usePrice();
      if (products.value.length > 0) {
        const skus = products.value.map((item) => item.sku);
        const priceData = await getPricesBySku(skus, pagination.value.itemsPerPage);
        products.value = products.value.map((product) => {
          const priceRange = priceData.items.find((item) => item.sku === product.sku)?.price_range;

          if (priceRange) {
            return {
              ...product,
              price_range: priceRange,
            };
          }

          return { ...product };
        });
      }

      isPriceLoaded.value = true;
    });

    const goToPage = (page: number) => {
      uiHelpers.changePage(page, false);
      fetch();
    };

    const doChangeItemsPerPage = (itemsPerPage: number) => {
      uiHelpers.changeItemsPerPage(itemsPerPage, false);
      goToPage(0);
    };

    const onReloadProducts = () => {
      goToPage(0);
      productContainerElement.value.scrollIntoView();
    };

    return {
      isPriceLoaded,
      ...uiHelpers,
      toggleFilterSidebar,
      isCategoryGridView,
      changeToCategoryListView,
      changeToCategoryGridView,
      isFilterSidebarOpen,
      addItemToCart,
      addItemToWishlist,
      pagination,
      products,
      sortBy,
      isShowCms,
      isShowProducts,
      cmsContent,
      activeCategoryName,
      activeCategoryImage,
      routeData,
      doChangeItemsPerPage,
      productContainerElement,
      categoryMeta,
      categoryImage, // Expose categoryImage
      onReloadProducts,
      goToPage,
    };
  },
  head() {
    return getMetaInfo(this.categoryMeta);
  },
});
</script>

<style lang="scss" scoped>
#category {
  box-sizing: border-box;
  background: #F5F1DD;
  @include for-desktop {
    max-width: 100%;
    margin: 0 auto;
  }
}
.category-image {
    width: 100%;
}
.category-image img {
    width: 100%;
}
.category_list h3 {
    color: #9f9f9f;
    font-size: 20px;
    font-weight: 400;
    line-height: 1.8;
    padding: 10px 25px;
    background-color: #eae9c9;
    font-family: 'Montserrat';
}
.category_sidebar_inner {
    position: sticky;
    top: 0px;
}
.category_list ul {
    background: #ffff;
    margin: 0;
    padding: 1px 25px 25px 25px;
    list-style: none
}
.category_list ul li {
  position: relative;
  border: 1px solid rgba(133, 134, 140, 0.15);
  border-left: 0px;
  border-right: 0;
  border-bottom: 0px;
  margin: 9px 0 0;
  padding-top: 10px;
  padding-left: 23px;
}
.category_list ul li:first-child {
  border-top:none;
}
.category_list ul li a {
    line-height: 25px;
    display: block;
    color:#85868c;
}
.category_list ul li::before {
  content: '';
  position: absolute;
  top: 17px;
  left: 0px;
  background-position: center;
  background-size: auto;
  background-image:url('../../../static/category/category_list_arrow.png');
  width: 10px;
  height: 10px;

}

a.catalogue_img {
  width: 100%;
  display: block;
  margin-top: 10px;
}

a.catalogue_img img{
  width: 100%;
}

.category-layout {
  max-width: 1400px;
  margin: auto;
  box-sizing: border-box;
  padding:70px 50px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;

  @include for-mobile {
    flex-direction: column;
  }

  .column {
    display: flex;
    flex-direction: column;
    flex: 1;

    @include for-mobile {
      flex: auto;
    }

    &.sidebar {
      max-width: 24%;
      padding-left: 0px;
    }
  }
}

.main {
  &.section {
    padding: 0;

    @include for-mobile {
      $padding: var(--spacer-xs);
      padding: $padding;
      width: calc(100% - 2 * $padding);
    }
  }
}

.main {
  display: flex;
}

.category-title  {
  margin-left: var(--spacer-sm);
  text-align: left;
}

.sidebar {
  flex: 0 0 15%;
  padding: 0 0 0 var(--spacer-sm);
  border: 1px solid var(--c-light);
  border-width: 0 1px 0 0;
}

.sidebar-filters {
  --overlay-z-index: 3;
  --sidebar-title-display: none;
  --sidebar-top-padding: 0;

  @include for-desktop {
    --sidebar-content-padding: 0 var(--spacer-xl);
    --sidebar-bottom-padding: 0 var(--spacer-xl);
  }
}

.products {
  box-sizing: border-box;
  flex: 1;
  margin: 0;

  @include for-desktop {
    &__pagination {
      display: flex;
      justify-content: flex-start;
      margin: var(--spacer-xl) 0 0 0;
    }
  }

  @include for-mobile {
    &__display-opt {
      display: flex;
      justify-content: space-around;
      align-items: center;
    }

    &__show-on-page {
      margin-top: 12px;
    }
  }

  &__show-on-page {
    display: flex;
    justify-content: flex-end;
    align-items: baseline;

    &__label {
      font-family: var(--font-family--secondary);
      font-size: var(--font-size--sm);
    }
  }
}

::v-deep .sf-sidebar__aside {
  --sidebar-z-index: 3;
}
</style>
