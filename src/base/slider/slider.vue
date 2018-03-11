<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="(item, index) of dots" :key="index" :class="{active: currentPageIndex === index}"></span>   
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
      }
    },
    data() {
      return {
        dots: [],
        currentPageIndex: 0
      }
    },
    mounted() {
      this.$nextTick(() => {
        this._setSliderWith()
        this._initDots()
        this._initSlider()

        if (this.autoPlay) {
          this._autoPlay()
        }
      })
      // setTimeout(() => {
      //   this._setSliderWith()
      //   this._initSlider()
      // }, 20)
    },
    methods: {
      _setSliderWith() {
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
        if (this.loop) {
          width += 2 * sliderWidth
        }
        this.$refs.sliderGroup.style.width = `${width}px`
      },
      _initDots() {
        this.dots = this.childrens.length
      },
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
        width: 8px
        height: 8px
        border-radius: 50%
        background: $color-text-l
        &.active
          width: 20px
          border-radius: 5px
          background: $color-text-l1
</style>
