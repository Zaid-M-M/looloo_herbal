<template>
  <div class='menubar'>
    <SfHeader
      class="sf-header--has-mobile-search"
      :class="{ 'header-on-top': isSearchOpen }"
    >
      <template #logo>
        <HeaderLogo />
      </template>
      <template #navigation>
        <HeaderNavigation :category-tree="categoryTree" />
      </template>
      <template #aside>
        <div class="sf-header__switchers">
          <CurrencySelector
            v-if="hasCurrencyToSelect"
            class="smartphone-only"
          />
          <StoreSwitcher
            v-if="hasStoresToSelect"
            class="smartphone-only"
          />
        </div>
      </template>
      <template #header-icons="{ activeIcon }">
        <div class="sf-header__icons">
          <SfButton
            v-e2e="'app-header-account'"
            class="sf-button--pure sf-header__action"
            data-testid="accountIcon"
            aria-label="Account"
            @click="handleAccountClick"
          >
            <SvgImage
              :icon="accountIcon"
              :label="$t('Account')"
              width="1.25rem"
              height="1.25rem"
              :class="{
                'sf-header__icon is-active': activeIcon === 'account',
              }"
            />
          </SfButton>
          <SfButton
            v-if="isAuthenticated"
            class="sf-button--pure sf-header__action"
            data-testid="wishlistIcon"
            aria-label="Wishlist"
            @click="toggleWishlistSidebar"
          >
            <SvgImage
              :icon="wishlistHasProducts ? 'heart_fill' : 'heart'"
              :label="$t('Wishlist')"
              width="1.25rem"
              height="1.25rem"
              class="sf-header__icon"
              :class="{
                'sf-header__icon is-active': activeIcon === 'wishlist',
              }"
            />
            <SfBadge
              v-if="wishlistHasProducts"
              class="sf-badge--number cart-badge"
            >
              {{ wishlistItemsQty }}
            </SfBadge>
          </SfButton>
          <SfButton
            v-e2e="'app-header-cart'"
            class="sf-button--pure sf-header__action"
            aria-label="Toggle cart sidebar"
            @click="toggleCartSidebar"
          >
            <SvgImage
              icon="empty_cart"
              :label="$t('Cart')"
              width="20"
              height="20"
              class="sf-header__icon"
              :class="{
                'sf-header__icon is-active': activeIcon === 'cart',
              }"
            />
            <SfBadge
              v-if="cartTotalItems"
              class="sf-badge--number cart-badge"
            >
              {{ cartTotalItems }}
            </SfBadge>
          </SfButton>
        </div>
      </template>
      <template #search>
        <SearchBar
          :is-search-open="isSearchOpen"
          @set-is-open="isSearchOpen = $event"
          @set-search-results="productSearchResults = $event"
        />
      </template>
    </SfHeader>
    <SearchResults
      v-if="isSearchOpen"
      :visible="isSearchOpen"
      :search-results="productSearchResults"
      @close="isSearchOpen = false"
    />
    <SfOverlay :visible="isSearchOpen" />
  </div>
</template>

<script lang="ts">
import {
  SfOverlay, SfHeader, SfButton, SfBadge,
} from '@storefront-ui/vue';

import {
  computed,
  ref,
  defineComponent,
  useRouter,
  useContext,
  onMounted,
  useFetch,
} from '@nuxtjs/composition-api';
import HeaderNavigation from '~/components/Header/Navigation/HeaderNavigation.vue';
import { useCategory } from '~/modules/catalog/category/composables/useCategory';
import {
  useUiHelpers,
  useUiState,
} from '~/composables';
import { useCart } from '~/modules/checkout/composables/useCart';
import { useWishlist } from '~/modules/wishlist/composables/useWishlist';
import { useUser } from '~/modules/customer/composables/useUser';
import { useWishlistStore } from '~/modules/wishlist/store/wishlistStore';
import type { CategoryTree, ProductInterface } from '~/modules/GraphQL/types';
import HeaderLogo from '~/components/HeaderLogo.vue';
import SvgImage from '~/components/General/SvgImage.vue';
import { useTopBar } from './TopBar/useTopBar';

export default defineComponent({
  components: {
    HeaderNavigation,
    SfHeader,
    SfOverlay,
    HeaderLogo,
    SvgImage,
    SfButton,
    SfBadge,
    CurrencySelector: () => import('~/components/CurrencySelector.vue'),
    StoreSwitcher: () => import('~/components/StoreSwitcher.vue'),
    SearchBar: () => import('~/components/Header/SearchBar/SearchBar.vue'),
    SearchResults: () => import(
      /* webpackPrefetch: true */ '~/components/Header/SearchBar/SearchResults.vue'
    ),
  },
  setup() {
    const router = useRouter();
    const { app } = useContext();
    const { toggleCartSidebar, toggleWishlistSidebar, toggleLoginModal } = useUiState();
    const { setTermForUrl, getCatLink } = useUiHelpers();
    const { isAuthenticated } = useUser();
    const { loadTotalQty: loadCartTotalQty, cart } = useCart();
    const { loadItemsCount: loadWishlistItemsCount } = useWishlist();
    const { categories: categoryList, load: categoriesListLoad } = useCategory();

    const { hasCurrencyToSelect, hasStoresToSelect } = useTopBar();

    const isSearchOpen = ref(false);
    const productSearchResults = ref<ProductInterface[] | null>(null);

    const wishlistStore = useWishlistStore();
    const wishlistItemsQty = computed(() => wishlistStore.wishlist?.items_count ?? 0);

    const wishlistHasProducts = computed(() => wishlistItemsQty.value > 0);
    const accountIcon = computed(() => (isAuthenticated.value ? 'profile_fill' : 'profile'));
    const categoryTree = ref<CategoryTree[]>([]);

    const handleAccountClick = async () => {
      if (isAuthenticated.value) {
        await router.push(app.localeRoute({ name: 'customer-my-profile' }));
      } else {
        toggleLoginModal();
      }
    };

    useFetch(async () => {
      await categoriesListLoad({ pageSize: 20 });

      categoryTree.value = categoryList.value?.[0]?.children
        .filter((category) => category.include_in_menu);
    });

    onMounted(async () => {
      if (app.$device.isDesktop) {
        await loadCartTotalQty();
        // eslint-disable-next-line promise/catch-or-return
        await loadWishlistItemsCount();
      }
    });

    return {
      accountIcon,
      cartTotalItems: computed(() => cart.value?.total_quantity ?? 0),
      categoryTree,
      getCatLink,
      handleAccountClick,
      isAuthenticated,
      isSearchOpen,
      productSearchResults,
      setTermForUrl,
      toggleCartSidebar,
      toggleWishlistSidebar,
      wishlistHasProducts,
      wishlistItemsQty,
      hasCurrencyToSelect,
      hasStoresToSelect,
    };
  },
});
</script>

<style>
.menubar header.sf-header__header {
    max-width: 1400px;
    width: 100%;
    box-sizing: border-box;
    padding: 0 50px;
    height: 100px;
}
.menubar .header-navigation a {
  padding: 0px;
    font-size: 14px;
    color: #978d4b;
    font-family: "Montserrat", sans-serif;
    font-weight: 600;
    margin: 0 0 0 30px;
    flex: auto;
}
.menubar .sf-header-navigation-item__item a:last-child {
  display: none;
}
.menubar .sf-input.sf-search-bar.sf-header__search {
  display: none;
}
.menubar .sf-header__icons button {
    margin: 0 0 0 20px;
}
.menubar .header-navigation a span {
    margin: 0 0 0 5px;
    --icon-color:#978d4b;
}
</style>

<style lang="scss" scoped>
.sf-header {
  --header-padding: var(--spacer-sm);
  @include for-desktop {
    --header-padding: 0 var(--spacer-sm);
  }

  &__switchers {
    display: flex;
  }
}

.header-on-top {
  z-index: 2;
}

.cart-badge {
  position: absolute;
  bottom: 40%;
  left: 40%;
}
</style>
