<template>
  <div id="mobile">
    <div class="bgm-btn" 
      v-if="h5Config.bgm"
      :class="{rotate: isPlay}" 
      @click="togglePlay">
      <audio ref="music" :src="h5Config.bgm" autoplay preload="auto" loop></audio>
    </div>
    <swiper :options="swiperOption" ref="mySwiper">
      <swiper-slide 
        v-for="(page, idx) in pages"
        :key="page.id">
        <div class="wrap-bg" 
          :style="{
            backgroundColor: page.css.bgc,
            backgroundImage: 'url(' + page.css.bgi + ')'
          }"></div>
        <component-renderer 
          v-for="comp in page.comps"
          :key="comp.id"
          class="comp animated"
          :class="{[animations[comp.anim.type]['class']]: activePage === idx}"
          :comp="comp">
        </component-renderer>
      </swiper-slide>
    </swiper>
  </div>
</template>

<script>
import { swiper, swiperSlide } from 'vue-awesome-swiper'
import ComponentRenderer from '@/containers/component-renderer'
import { ANI_NAME } from '@/const/animation-match.js'

export default {
  name: 'Mobile',
  data () {
    return {
      isPlay: true,
      activePage: 0,
      animations: ANI_NAME,
      swiperOption: {
        direction: 'vertical'
      }
    }
  },
  computed: {
    Swiper () {
      return this.$refs.mySwiper.swiper
    },
    pages () {
      return this.$store.getters.pages
    },
    h5Config () {
      return this.$store.getters.h5Config
    }
  },
  methods: {
    togglePlay () {
      const music = this.$refs.music
      if (this.isPlay) {
        this.isPlay = false
        music.pause()
      } else {
        this.isPlay = true
        music.play()
      }
    }
  },
  mounted () {
    const self = this
    this.Swiper.on('init', function () {
      self.activePage = this.activeIndex
    })
    this.Swiper.on('slideChangeTransitionStart', function () {
      self.activePage = this.activeIndex
    })
    // this.Swiper.on('slideChangeTransitionEnd', function () {
    //   self.activePage = null
    // })
  },
  components: {
    swiper,
    swiperSlide,
    ComponentRenderer
  }
}
</script>
