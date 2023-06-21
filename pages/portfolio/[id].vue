<template>
  <section class="container">
    <div class="portfolio-detail">
      <nuxt-link to="/" class="button button--back button--grey">
        &larr;&nbsp;Go back
      </nuxt-link>
      <h1 class="title" v-text="item.name" />
      <h2 class="subtitle">
        <span v-html="item.description" /><br>
      </h2>
      <ul v-if="item.tools" class="technos">
        <li v-for="(tool, idx) in item.tools" :key="idx" class="tag" v-text="tool" />
      </ul>
      <p v-if="item.link" class="cta">
        <a class="button button--blue" :href="item.link" target="_blank">Discover {{ item.name }}</a>
      </p>
      <div class="portfolio-images">
        <template v-if="item.videos">
          <div
            v-for="(video, idx) in item.videos"
            :key="`video-${idx}`"
            v-intersection-observer="[(entries) => entries.forEach(toggleInViewportCSSClass)]"
            class="portfolio-video"
          >
            <iframe
              :src="video.src"
              width="640"
              height="360"
              frameborder="0"
              webkitallowfullscreen
              mozallowfullscreen
              allowfullscreen
            />
          </div>
        </template>
        <img
          v-for="(image, idx) in item.portfolioImages"
          :key="`image-${idx}`"
          v-intersection-observer="[(entries) => entries.forEach(toggleInViewportCSSClass)]"
          :src="image"
          :alt="`Screenshot of '${item.name}' project`"
        >
      </div>
    </div>
  </section>
</template>

<script>
import { vIntersectionObserver } from '@vueuse/components'

export default {
  directives: {
    IntersectionObserver: vIntersectionObserver
  },
  validate ({ params }) {
    return params.id
  },
  async setup () {
    const route = useRoute()
    const { data } = await useFetch(`/api/portfolio/${route.params.id}`)
    return {
      item: data
    }
  }
}
</script>

<style lang="scss">
@use 'sass:math' as math;

.portfolio-detail {
  .button--back {
    font-size: 0.7em;
    @media (min-width: 32em) {
      float: left;
      clear: right;
      margin: 1rem 2rem;
    }
  }
  .title {
    clear: left;
  }
  .subtitle {
  }

  .technos {
    .tag {
      background: #eee;
      border-radius: 3px 0 0 3px;
      color: #555;
      display: inline-block;
      height: 26px;
      line-height: 26px;
      padding: 0 20px 0 23px;
      position: relative;
      margin: 0 10px 10px 0;
      text-decoration: none;
      transition: color 0.2s;
    }

    .tag::before {
      background: #fff;
      border-radius: 10px;
      box-shadow: inset 0 1px rgba(0, 0, 0, 0.25);
      content: '';
      height: 6px;
      left: 10px;
      position: absolute;
      width: 6px;
      top: 10px;
    }

    .tag::after {
      background: #fff;
      border-bottom: 13px solid transparent;
      border-left: 10px solid #eee;
      border-top: 13px solid transparent;
      content: '';
      position: absolute;
      right: 0;
      top: 0;
    }
  }
  .portfolio-video {
    position: relative;
    width: 100%;
    height: 0;
    padding-bottom: math.percentage(math.div(1, math.div(16, 9)));
    > iframe {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 100%;
      margin: 0 auto;
      box-shadow: 0px 0px 100px -5px rgba(100, 100, 100, 0.3);
    }
  }
  .portfolio-images {
    margin: 1em auto;
    @media (min-width: 32em) {
      margin: 3em auto;
      padding: 0 2em;
    }
    * + * {
      margin-top: 2em;
      @media (min-width: 32em) {
        margin-top: 5em;
      }
    }
    iframe,
    img {
      display: block;
      max-width: 100%;
      margin-left: auto;
      margin-right: auto;
      box-shadow: 0px 0px 100px -5px rgba(100, 100, 100, 0.3);
    }
    > * {
      // only for javascript version
      #__nuxt:not([data-server-rendered="true"]) & {
        backface-visibility: hidden;
      transform: translateZ(0);
        animation-fill-mode: backwards;
        animation: scroll-appear 1.2s cubic-bezier(.165,.84,.44,1);
        animation-play-state: paused;
        &.in-viewport {
          animation-play-state: running;
        }
      }
    }
  }
}
</style>
