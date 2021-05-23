<script>
export default {
  async asyncData ({ $content, params }) {
    // fetch our article here
    const article = await $content('articles', params.slug).fetch()

    return { article }
  },
  methods: {
    formatDate (date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>

<template>
  <article>
    <nav>
      <ul>
        <li v-for="link of article.toc" :key="link.id">
          <NuxtLink :to="`#${link.id}`" :class="{ 'h2': link.depth === 2, 'h3': link.depth === 3 }">
            {{ link.text }}
          </NuxtLink>
        </li>
      </ul>
    </nav>
    <h1>{{ article.title }}</h1>
    <p>{{ article.description }}</p>
    <img :src="require(`~/assets/images/${article.img}`)" :alt="article.alt">
    <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>
    <nuxt-content :document="article" />
    <pre> {{ article }} </pre>
  </article>
</template>

<style>
  .nuxt-content h2 {
    font-weight: bold;
    font-size: 28px;
  }
  .nuxt-content h3 {
    font-weight: bold;
    font-size: 22px;
  }
  .nuxt-content p {
    margin-bottom: 20px;
    color: rebeccapurple;
  }
  .icon.icon-link {
  background-image: url('~assets/images/preview.png');
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
.h2 {
  padding: 2rem 0;
}
.h3 {
  padding-bottom: 2rem;
  margin-left: 2rem;
}
.note {
  background-color: blue;
  color: white;
}
</style>
