<template>
  <section class="container">
    <div>
      <h1 class="title">
        <small>Lory Bineau</small><br>Senior frontend developer
      </h1>
      <h2 class="subtitle">
        HTML5, CSS3 (SASS), Javascript (ES6/ES7), VueJS, ...
      </h2>
      <div class="links">
        <a href="https://codepen.io/lbineau/" target="_blank" class="button button--grey-light">Codepen</a>
        <a href="https://www.linkedin.com/in/lbineau/" target="_blank" class="button button--blue">Linkedin</a>
        <a href="https://github.com/lbineau/portfolio-v2" target="_blank" class="button button--grey">GitHub</a>
        <a href="/media/lory-bineau_CV_2018.pdf" target="_blank" class="button button--black">C.V.(pdf)</a>
      </div>
      <PortfolioGrid v-if="portfolio" :items="portfolio" />
      <CodepenGrid />
    </div>
  </section>
</template>

<script>
import PortfolioGrid from '~/components/portfolio-grid.vue'
import CodepenGrid from '~/components/codepen-grid.vue'

export default {
  components: {
    PortfolioGrid,
    CodepenGrid
  },
  async setup () {
    const { data } = await useFetch('/api/portfolio')
    return {
      portfolio: data
    }
  }
}
</script>

<style lang="scss">
</style>
