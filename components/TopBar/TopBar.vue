<template>
  <div class="topbar">

      <div class="topbar_row">

        <div class="topbar_col1">
          <div class="tb_col1_inner">
            <img src="../../static/header/Khojati_herbal_tag.png"/>
            <p>Blessed to Cure Since Generations Naturally...</p>
          </div>
        </div>

        <div class="topbar_col2">
          <div class="tb_col2_inner">
            <ul>
              <li>
                <a href="#">Product Catalogue</a>
              </li>
              <li>
                <a href="#"><img src="../../static/header/blog_icon.png" /> Blog</a>
              </li>
              <li class="offer_li">
                <a href="/offer-zone.html"><img src="../../static/header/offer_icon.png" /> Offer Zone</a>
              </li>
              <li>
                <SfButton
                  v-e2e="'app-header-account'"
                  class="sf-button--pure sf-header__action"
                  data-testid="accountIcon"
                  aria-label="Account"
                  @click="handleAccountClick"
                >
                  Sign In
                </SfButton>
              </li>
            </ul>
          </div>
        </div>

      </div>
  
    <!-- <template #center>
      <p>{{ $t('Download') }}</p>
      <SfButton class="topbar__button sf-button--text">
        {{ $t('Find out more') }}
      </SfButton>
    </template> -->
    <!-- <template #right> -->
      <!-- <CurrencySelector v-if="hasCurrencyToSelect" />
      <StoreSwitcher v-if="hasStoresToSelect" /> -->
    <!-- </template> -->
  </div>
</template>

<script lang="ts">

import { defineComponent, useRouter , useContext } from '@nuxtjs/composition-api';
import {
  useUiState,
} from '~/composables';
import { SfButton, SfTopBar } from '@storefront-ui/vue';
import { useTopBar } from './useTopBar';
import { useUser } from '~/modules/customer/composables/useUser';

export default defineComponent({
  components: {
    CurrencySelector: () => import('../CurrencySelector.vue'),
    StoreSwitcher: () => import('../StoreSwitcher.vue'),
    SfTopBar,
    SfButton,
  },
  setup() {
    const router = useRouter();
    const { app } = useContext();
    const { hasCurrencyToSelect, hasStoresToSelect } = useTopBar();
    const { isAuthenticated } = useUser();
    const { toggleLoginModal } = useUiState();
    const handleAccountClick = async () => {
      if (isAuthenticated.value) {
        await router.push(app.localeRoute({ name: 'customer-my-profile' }));
      } else {
        toggleLoginModal();
      }
    };

    return {
      hasCurrencyToSelect,
      hasStoresToSelect,
      handleAccountClick,
      isAuthenticated,
    };
  },
});

</script>

<style>
.topbar {
  background-color: #05923f;
}
.topbar_row {
    max-width: calc(1400px);
    width: 100%;
    margin: 0 auto;
    padding: 0 50px;
    position: relative;
    box-sizing: border-box;
    display: flex;
    height: 36px;
    align-items: center;
    justify-content: space-between;
}
.topbar_col1 {
    display: block;
}
.tb_col1_inner {
  display: flex;
  align-items: center;
}
.tb_col1_inner img{
  margin-right: 15px;
}
.tb_col1_inner p {
    font-family: 'Oxygen';
    text-transform: uppercase;
    font-size: 14px;
    color: #fff;
    font-weight: normal;
}
.topbar_col2 {
  display: block;
} 
.tb_col2_inner {
  display: flex;
}
.tb_col2_inner ul {
    display: flex;
    list-style: none;
    margin-top: 0;
    margin-bottom: 0;
    height: 36px;
}
.tb_col2_inner ul li:last-child {
  margin-right: 0;
}
.tb_col2_inner ul li {
  margin-right:20px;
  display: flex;
  align-items: center;
}
.offer_li {
  background-color: #096e32;
  padding: 0 10px;
}
.tb_col2_inner ul li a{
  font-family: 'Oxygen';
    text-transform: uppercase;
    font-size: 13px;
    color: #fff;
    font-weight: normal;
    display: flex;
    align-items: center;
    gap: 5px;
}
.tb_col2_inner ul li button {
    margin: 0;
    font-family: 'Oxygen';
    text-transform: uppercase;
    font-size: 13px;
    color: #fff;
    font-weight: normal;
}
</style>
<style lang="scss" scoped>


.topbar {
  position: relative;
  z-index: 2;
  &__button {
    margin: 0 0 0 var(--spacer-xs);
  }
}

::v-deep {
  .sf-top-bar__container {
    justify-content: space-between;
    & > * {
      width: calc(100% / 3);
      justify-content: center;
    }
    & > :first-child {
      justify-content: flex-start;
    }
    & > :last-child {
      justify-content: flex-end;
    }
  }
}
</style>
