<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <div class="slider-item" v-if="sliderList.length" v-for="(item, index) in sliderList" :key="index">
        <a :href="item.linkUrl">
          <img :src="item.picUrl">
        </a>
      </div>
    </div>
    <div class="dots">
      <span class="dot" v-if="sliderList.length" v-for="(item, index) in sliderList" :key="index" :class="{active: currentPageIndex === index}"></span>   
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import { addClass } from 'common/js/dom'
  export default {
    name: 'slider',
    props: {
      loop: {
        type: Boolean,
        default: true
      },
      autoPlay: {
        type: Boolean,
        default: true
      },
      interval: {
        type: Number,
        default: 4000
      },
      sliderData: {
        type: Array
      }
    },
    data() {
      return {
        // sliderList: this.sliderData,
        dots: [],
        currentPageIndex: 0
      }
    },
    computed: {
      sliderList() {
        return this.sliderData
      }
    },
    created() {
      // console.log(this.sliderList)
    },
    mounted() {
      // this.$nextTick(() => {
      //   this._setSliderWith()
      //   this._initDots()
      //   this._initSlider()

      //   if (this.autoPlay) {
      //     this._autoPlay()
      //   }
      // })
      setTimeout(() => {
        this._setSliderWith()
        // this._initDots()
        this._initSlider()

        if (this.autoPlay) {
          this._autoPlay()
        }
      }, 20)

      window.addEventListener('resize', () => {
        if (!this.slider) {
          return
        }
        this._setSliderWith(true)
        this.slider.refresh()
      }, false)
    },
    destroyed() {
      clearTimeout(this.timer)
    },
    methods: {
      _setSliderWith(isResize) {
        this.childrens = this.$refs.sliderGroup.children
        let width = 0
        let sliderWidth = this.$refs.slider.clientWidth
        const childrensLen = this.childrens.length
        console.log(sliderWidth, childrensLen)
        for (let i = 0; i < childrensLen; i++) {
          let child = this.childrens[i]
          addClass(child, 'slider-item')
          child.style.width = `${sliderWidth}px`
          width += sliderWidth
        }console.log(width)
        if (this.loop && !isResize) {
          width += 2 * sliderWidth
        }
        this.$refs.sliderGroup.style.width = `${width}px`
      },
      // _initDots() {
      //   this.dots = this.childrens.length
      // },
      _autoPlay() {
        let pageIndex = this.currentPageIndex + 1
        if (this.loop) {
          pageIndex += 1
        }
        this.timer = setTimeout(() => {
          this.slider.goToPage(pageIndex, 0, 400)
        }, this.interval)
      },
      _initSlider() {
        this.slider = new BScroll(this.$refs.slider, {
          scrollX: true,
          scrollY: false,
          momentum: false,
          snap: true,
          snapLoop: this.loop,
          snapThreshold: 0.3,
          snapSpeed: 400
        })

        this.slider.on('scrollEnd', () => {
          let pageIndex = this.slider.getCurrentPage().pageX
          if (this.loop) {
            pageIndex -= 1
          }
          this.currentPageIndex = pageIndex

          clearTimeout(this.timer)
          this._autoPlay()
        })
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .slider
    min-height: 1px
    .slider-group
      position: relative
      overflow: hidden
      white-space: nowrap
      .slider-item
        float: left
        box-sizing: border-box
        overflow: hidden
        text-align: center
        a
          display: block
          width: 100%
          overflow: hidden
          text-decoration: none
        img
          display: block
          width: 100%
    .dots
      position: absolute
      left: 0
      right: 0
      bottom: 12px
      text-align: center
      font-size: 0
      .dot
        display: inline-block
        margin: 0 4px
        width: 20px
        height: 20px
        border-radius: 10px
        background: $color-text-l
        &.active
          width: 40px
          border-radius: 5px
          background: $color-text-l1
</style>
