<template lang="pug">
  main(v-if="index > -1" :style="`font-size:${zoom.toFixed(1)}em`")
    section(v-for="slide, i in slides"
            :class="i < index ? 'past' : i === index ? 'present' : 'future'")
      iframe.card(v-if="slide.url" :src="slide.url")

      p(v-else-if="slide.pen"
        data-height="100%"
        data-theme-id="dark"
        :data-pen-title="slide.pen"
        :data-slug-hash="slide.slug"
        data-default-tab="js,result"
        data-user="zerious"
        data-embed-version="2"
        class="codepen")

      div.card(v-else :style="slide.photo ? `background-image:url(${slide.photo})` : ''")
        img.logo(v-if="slide.logo" :src="slide.logo")
        h1(v-if="slide.talk") {{slide.talk}}
        h2(v-if="slide.title") {{slide.title}}
        div(v-if="slide.content") {{slide.content}}
        div(v-if="slide.html" v-html="slide.html")
        ul(v-if="slide.list")
          li(v-for="item in slide.list")
            a(href="item" v-if="/^http/.test(item)") {{item}}
            span(v-else) {{item}}
</template>

<script>
const moves = { 37: -1, 39: 1 }

module.exports = {
  mounted () {
    this.locate()
    document.addEventListener('keyup', event => {
      const move = moves[event.keyCode]
      if (move) {
        const to = this.index + move
        if (to >= 0 && to < this.count) {
          location.hash = `#${to + 1}`
          this.$set(this, 'index', to)
        }
      }
    })
    window.addEventListener('resize', () => this.resize())
  },

  updated () {
    this.resize()
    if (process.browser) {
      const script = document.createElement('script')
      script.src = 'https://production-assets.codepen.io/assets/embed/ei.js'
      document.getElementsByTagName('head')[0].appendChild(script)
    }
  },

  data () {
    const zoom = 2
    const index = -1
    const slides = [{
      logo: '/vue.svg',
      talk: 'Intro to Vue.js',
      html: '<p style="margin:2em 0 0 1.2em"><img src="/sam-grace.jpg" style="width:3.6em;border-radius:50%;margin:0 1em;float:left">Sam Eubank<div style="color:#345;font-size:0.8em">Principal Software Engineer</div><span style="color:#4b8">Globys</span></p>'
    }, {
      title: 'Vue.js',
      content: 'Vue is a progressive framework for building user interfaces. Vue is designed to be incrementally adoptable. The core library is focused on the view layer only.'
    }, {
      url: 'https://trends.google.com/trends/embed/explore/TIMESERIES?req=%7B%22comparisonItem%22%3A%5B%7B%22keyword%22%3A%22%5C%22vue%20js%5C%22%20%2B%20%5C%22vue.js%5C%22%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%222013-01-29%202018-01-29%22%7D%2C%7B%22keyword%22%3A%22%5C%22react%20js%5C%22%20%2B%20%5C%22react.js%5C%22%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%222013-01-29%202018-01-29%22%7D%2C%7B%22keyword%22%3A%22%5C%22angular%20js%5C%22%20%2B%20%5C%22angular.js%5C%22%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%222013-01-29%202018-01-29%22%7D%2C%7B%22keyword%22%3A%22%5C%22backbone%20js%5C%22%20%2B%20%5C%22backbone.js%5C%22%22%2C%22geo%22%3A%22%22%2C%22time%22%3A%222013-01-29%202018-01-29%22%7D%5D%2C%22category%22%3A0%2C%22property%22%3A%22%22%7D&tz=480&eq=date%3D2013-01-29%25202018-01-29%26q%3D%2522vue%2520js%2522%2520%252B%2520%2522vue.js%2522%2C%2522react%2520js%2522%2520%252B%2520%2522react.js%2522%2C%2522angular%2520js%2522%2520%252B%2520%2522angular.js%2522%2C%2522backbone%2520js%2522%2520%252B%2520%2522backbone.js%2522'
    }, {
      title: 'Why so popular?',
      list: [
        'Familiar patterns',
        'Not much boilerplate',
        'If Re* exists, Vue* probably does too',
        'Template language agnostic!'
      ]
    }, {
      pen: '2-way Binding',
      slug: 'xYbwjV'
    }, {
      pen: 'Components',
      slug: 'YePBMX'
    }, {
      pen: 'Lifecycle Hooks',
      slug: 'PQwLPp'
    }, {
      pen: 'Events, Methods & Conditionals',
      slug: 'BYybwJ'
    }, {
      pen: 'Computed Properties',
      slug: 'QQwoZJ'
    }, {
      url: 'https://vuex.vuejs.org/en/intro.html'
    }, {
      url: 'https://nuxtjs.org'
    }, {
      url: 'https://vuetifyjs.com'
    }, {
      url: 'https://weex.apache.org'
    }, {
      title: 'TODO: VENM',
      list: [ 'Vue.js', 'Express', 'Node.js', 'MongoDB' ]
    }, {
      title: 'Thank you!',
      photo: '/sam.jpg'
    }]
    const count = slides.length
    return { slides, index, count, zoom }
  },

  methods: {
    locate () {
      const current = ((process.browser ? location.hash : 0) || '').substr(1) * 1
      const index = current && !isNaN(current) ? current - 1 : 0
      this.$set(this, 'index', index)
    },
    resize () {
      const main = this.$el
      const width = main.offsetWidth
      this.$set(this, 'zoom', width / 400)
    }
  },

  components: {}
}
</script>

<style lang="stylus">
*
  margin 0
  padding 0

html
body
  position relative
  background #345
  color #fff
  width 100%
  height 100%
  perspective 400px
  overflow hidden

main
  padding 4% 5%

section
  position absolute
  display block
  width 90%
  height 90%
  background #4b8
  background -moz-linear-gradient(top left, #123 0%, #000 100%)
  background -webkit-linear-gradient(top left, #123 0%, #000 100%)
  background linear-gradient(to bottom right, #123 0%, #000 100%)
  transform-style preserve-3d
  transition transform 0.8s, opacity 0.8s
  opacity 1

div.card
  padding 3% 5%
  line-height 1.5
  height 100%
  background-size cover
  background-position center center

.card>ul
  margin-left 1em

section>.cp_embed_wrapper
  margin -4px

section
.card
  border-radius 0.5em
  overflow hidden
  z-index 2

.past
  transform translate3d(-90%, 0, 0) rotateY(-120deg) translate3d(-90%, 0, 0)
  z-index 1

.future
  transform translate3d(90%, 0, 0) rotateY(120deg) translate3d(90%, 0, 0)
  z-index 1

.past
.future
  opacity 0

h1
  display block
  font-size 2em
  vertical-align top
  padding 0.2em
  line-height 1.2

.logo
  width 7em
  margin 0 1em 0 0
  float left

h2
  font-size 2em
  color #4b8
  margin-bottom 0.5em

section>iframe
  width 100%
  height 100%
  border 0

.cp_embed_wrapper
  height 100%
</style>
