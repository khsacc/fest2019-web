<template lang="pug">
  div
    header#header
      h1
        p {{ title }}
        .bg
      .meta
        .txt
          p.author {{ author }}
          time(:datetime="params.date") {{ getDate(params.date) }}
        .bg
    article(v-html="bodyHtml")
</template>

<script>
/* eslint no-console: 0 */
import { sourceFileArray } from '~/assets/articles/summary.json'
export default {
  components: {},
  validate({ params }) {
    return sourceFileArray.includes(
      `./assets/articles/md/${params.date}-${params.slug}.md`
    )
  },
  asyncData({ params }) {
    return Object.assign(
      require(`~/assets/articles/json/${params.date}-${params.slug}.json`),
      { params }
    )
  },
  head() {
    const title = `${this.title} | 第73回灘校文化祭`
    return {
      title: title
    }
  },
  created() {
    if (!this.images) this.images = []
    if (!this.thumbnail) {
      this.images.push('default/thumbnail.jpg')
    }
    this.images.forEach(src => {
      require(`~/assets/img/blog/${src}`)
    })
  },
  mounted() {
    const header = document.getElementById('header')
    const thumbnail = this.thumbnail ? this.thumbnail : 'default/thumbnail.jpg'
    header.style.background = `url(../../../_nuxt/assets/img/blog/${thumbnail})`
    header.style.backgroundSize = 'cover'
    header.style.backgroundPosition = 'center center'
  },
  methods: {
    getDate(sourceDate) {
      return sourceDate.replace(/-/g, ' ')
    }
  }
}
</script>

<style lang="stylus" scoped>
header
  width 100%
  padding 10rem 0 10rem
  text-align center
  display flex
  flex-direction column
  align-items center
  justify-content center
  position relative
  z-index -2
  h1
    display inline-block
    position relative
    padding .4em 1em .1em
    font-size 3rem
    line-height 1.2em
    +tablet()
      font-size 2.5rem
    +sp()
      font-size 2rem
    p
      opacity 0
      animation h1Enter 300ms linear 1300ms 1 normal forwards
      @keyframes h1Enter {
        0% {
          opacity 0
        }
        100% {
          opacity 1
        }
      }
    .bg
      content ''
      position absolute
      width 100%
      height 100%
      top 0
      left 0
      background-color #fff
      z-index -1
      transform-origin left center
      transform scaleX(0)
      animation h1BgEnter 500ms ease-in-out 1000ms 1 normal forwards
      @keyframes h1BgEnter {
        0% {
          transform scaleX(0)
        }
        100% {
          transform scaleX(1)
        }
      }
  .meta
    margin-top 1em
    padding 0 1em
    position relative
    .txt
      opacity 0
      animation h1Enter 300ms linear 1500ms 1 normal forwards
      @keyframes h1Enter {
        0% {
          opacity 0
        }
        100% {
          opacity 1
        }
      }
      .author, time
        display inline-block
        font-size 1.2rem
        bold()
        color $gray-6
      .author::after
        content '/'
        margin 0 .5em
      time
        font-size 1.4rem
    .bg
      content ''
      position absolute
      width 100%
      height 100%
      top 0
      left 0
      background-color #fff
      z-index -1
      transform-origin left center
      transform scaleX(0)
      animation metaEnter 500ms ease-in-out 1200ms 1 normal forwards
      @keyframes metaEnter {
        0% {
          transform scaleX(0)
        }
        100% {
          transform scaleX(1)
        }
      }
article
  margin 5rem auto 10rem
  max-width 48rem
  padding 0 1.5rem
  +tablet()
    padding 0 3rem
  +touch()
    margin 5rem auto 7rem
  >>> h2
    bold()
    font-size 2rem
    margin 3rem 0 1rem
    border-bottom 2px solid $gray-2
  >>> h3
    bold()
    font-size 1.3rem
    margin 1.5rem 0 .5rem
  >>> h4
    bold()
    font-size 1rem
    margin 1rem 0 0
  >>> p
    line-height 2em
  >>> ol
    padding-left 2em
    list-style-type none
    counter-reset item
    li
      &::before
        counter-increment item
        content counter(item)'.'
        margin-right .5em
        bold()
        color $blue-10
  >>> img
    width 100%
    margin 2rem 0
</style>