<template>
  <section class="home">
    <div class="pt-6 pb-6 md:py-36 mx-auto flex flex-wrap flex-col md:flex-row items-center">
      <div class="flex flex-col w-full xl:w-2/5 justify-center lg:items-start overflow-y-hidden">
        <div v-html="$md.render(welcomeText)" class="home__welcome markdown or-" />

<!--        <div class="mb-12 xl:mb-0">
          <h4 v-if="isSignedUp">Thank you - we'll be in touch shortly.</h4>

          <form
            v-else
            @submit.prevent="handleSubmit"
            name="signups"
            netlify
            class="flex items-center border-b border-b-2 border-blue-400 py-2"
          >
            <input
              ref="emailInput"
              v-model="form.email"
              class="appearance-none mb-36 bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
              type="text"
              name="email"
              placeholder="your@email.com"
              aria-label="Email address"
            />

            <button
              class="flex-shrink-0 bg-blue-500 hover:bg-blue-700 border-blue-500 hover:border-blue-700 text-sm border-4 text-white py-1 px-2 rounded"
              type="submit"
            >
              Sign Up
            </button>
          </form>
        </div>-->
      </div>
      <div class="flex flex-col w-full xl:w-3/5 xl:pl-6">
        <img
          alt="Hero"
          class="rounded shadow-xl"
          src="/images/uploads/prolisok-landing.jpg"
        />
      </div>
    </div>

    <h2 class="text-base md:text-lg lg:text-xl xl:text-4xl latest-news__title">
      <span>Останні новини</span>

      <nuxt-link to="/blog" class="btn-primary">
        Більше новин
      </nuxt-link>
    </h2>

    <div class="flex flex-wrap md:-mx-4 pb-20 blog latest-news__grid">
      <div v-for="(post, index) in posts" :key="index" class="w-full md:w-1/4 my-4 md:px-4">
        <div class="post">
          <nuxt-link :to="`/blog/${post.slug}`">
            <img
              :alt="post.title"
              class="w-full"
              :src="post.featuredImage || 'https://source.unsplash.com/random/640x340'"
            />
            <p class="text-base post-date">
              {{ post.publishedAt }}
            </p>
            <div class="p-4 bg-white">
              <h2 class="text-1xl mb-2 post-title">{{ post.title }}</h2>
<!--

              <p class="text-base font-light">
                {{ post.excerpt }}
              </p>
-->

              <h6 class="text-blue-600 mt-4 font-medium">Детальніше</h6>
            </div>
          </nuxt-link>
        </div>
      </div>

    </div>

  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import settings from '@/content/settings/general.json';
const Pagination = () => import('@/components/commons/pagination.vue');

@Component({
  // Called to know which transition to apply
  transition() {
    return 'slide-left';
  },
  components: {
    Pagination,
  },

})
export default class Home extends Vue {
  welcomeText = settings.welcomeText;

/*  get posts(): Post[] {
    return this.$store.state.posts;
  }*/
  currentPage!: number;
  totalPages!: number;
  posts: Post[] = [];

  async asyncData({ params, store }) {
    const page: number = params.page ? parseInt(params.page, 10) : 1;
    const { perPageHome }: { perPageHome: number } = store.state;
    const range = page * perPageHome;

    const posts = store.state.posts.filter((post, index) => {
      const indexPage = index + 1;
      return range - perPageHome < indexPage && indexPage <= range;
    });

    return {
      currentPage: page,
      totalPages: Math.ceil(store.state.posts.length / perPageHome),
      posts: posts || [],
    };
  }

  isSignedUp = false;

  form = {
    email: '',
  };

  encode(data): string {
    return Object.keys(data)
      .map((key) => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
      .join('&');
  }

  validEmail(email): boolean {
    // eslint-disable-next-line
    const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
  }

  async handleSubmit(): Promise<void> {
    if (!this.validEmail(this.form.email)) {
      this.$refs.emailInput.focus();
      return;
    }

    try {
      await fetch('/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: this.encode({ 'form-name': 'signups', ...this.form }),
      });

      this.isSignedUp = true;
    } catch (error) {
      console.error(error);
    }
  }
}
</script>


<style lang="scss">
.blog {
  .post {
    @apply shadow-md;
    transition: all 0.2s cubic-bezier(0.64, 0, 0.35, 1);
    overflow: hidden;
    &-date {
      padding: 10px 1rem 0;
    }
    &-title {
      font-weight: 600;
    }
    img {
      transition: all .6s;
    }
    &:hover {
      @apply shadow-xl;
      img {
        transform: scale(1.05);
      }
    }
  }
}
.home__welcome {
  strong {
    margin-top: 10px;
    padding: 0 10px;
    display: inline-block;
    background-color: var(--brand-dark);
    color: #fff;
  }
}
.latest-news__title {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-top: 5vmin;
  margin-bottom: .5em;
/*  > span {
    padding: 0 10px;
    display: inline-block;
    background-color: var(--brand-dark);
    color: #fff;
  }*/
}
</style>
