<template>
  <section class="pics">
    <div class="py-8 md:py-16 text-center">
      <h1 class="text-lg md:text-xl lg:text-4xl xl:text-4xl">Плани та карти</h1>
    </div>

    <div class="md:-mx-4 pb-20">
      <img alt="map" class="rounded shadow-xl" src="/images/uploads/full-pic2.jpg"/> <img alt="map" class="rounded shadow-xl" src="/images/uploads/full-pic1.jpg"/>
      <h3 class="text-center subtitle">Детальний план: <a class="download-link -pdf" href="/images/uploads/fullplan.pdf">Скачати PDF</a></h3><br/> <img alt="map" class="rounded shadow-xl" src="/images/uploads/план_зданий_и_сооружений.jpg"/>
      <h3 class="text-center subtitle">Карта 2gis:</h3>
      <a href="https://go.2gis.com/wh8w3" target="_blank"> <img alt="map" class="rounded shadow-xl" src="/images/uploads/2gis.jpg"/> </a>
      <h3 class="text-center subtitle">Карта OpenStreetMap:</h3>
      <iframe width="100%" height="650" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" src="https://www.openstreetmap.org/export/embed.html?bbox=36.298613548278816%2C49.95895104856803%2C36.32693767547608%2C49.970876658194165&amp;layer=mapnik" style="border: 1px solid black"></iframe><br/><small><a href="https://www.openstreetmap.org/#map=16/49.9649/36.3128">Посмотреть более
      крупную карту</a></small>
    </div>
  </section>
</template>

<script lang="ts">
import {Component, Vue} from 'nuxt-property-decorator';
import {MetaInfo} from 'vue-meta';

@Component({
  head(): MetaInfo {
    return {
      title: 'Blog',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Blog index',
        },
      ],
    };
  },
})
export default class BlogIndex extends Vue {
  currentPage!: number;

  totalPages!: number;

  posts: Post[] = [];

}
</script>

<style lang="scss">
.pics {
  img {
    margin: 0 auto 2em;
  }
}
</style>
