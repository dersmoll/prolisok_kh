<template>
  <header class="flex items-center py-4 md:py-4">
    <div class="header__logo">
      <nuxt-link to="/">
        <img :src="logo" alt="Logo" />
      </nuxt-link>
    </div>

    <nav class="nav ml-auto">
      <ul class="flex flex-row items-center sm:mt-4 sm:pt-4 md:mt-0 md:pt-0">
        <li>
          <nuxt-link to="/blog" class="block font-bold px-4 py-1 md:p-2 lg:px-4">
            Новини
          </nuxt-link>
        </li>
        <li>
          <nuxt-link to="/pictures" class="block font-bold px-4 py-1 md:p-2 lg:px-4">
            Карти на плани
          </nuxt-link>
        </li>

        <li
          v-for="(page, index) in pages"
          :key="index"
        >
          <nuxt-link :to="`/${page.slug}`" class="block font-bold px-4 py-1 md:p-2 lg:px-4">{{ page.title }}</nuxt-link>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import settings from '@/content/settings/general.json';

@Component
export default class Header extends Vue {
  get pages(): Page[] {
    return this.$store.state.pages;
  }

  logo = settings.logo;
}
</script>

<style lang="scss">
.nav {
  background: var(--brand-dark);
  color: #fff;
  li {
    margin-right: 1px;
    > a {
      transition: all .3s;

      &.nuxt-link-active {
        background-color: lighten($brand-dark, 15%);
      }
      &:hover {
        background-color: lighten($brand-dark, 15%);
      }
    }
  }
}
.header__logo {
  img {
    max-width: 180px;
  }
}
@media (max-width: 600px) {
  .site-header {
    flex-direction: column;
    text-align: center;
    font-size: 14px;
    .nav {
      margin-top: 10px;
      width: 100%;
      ul {
        justify-content: space-between;
      }
    }
    .block {
      padding-left: 5px;
      padding-right: 5px;
    }
  }
}
</style>
