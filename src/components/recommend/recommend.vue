<template>
  <div class="recommend">
    <div class="slider-wrapper">
      <slider :sliderData="sliderData"  >
        <!-- 之所以用插槽，为了使组件通用！组件内item的每个具体字段，每个接口返回的肯定是不一样的。那我获取数据后改造的复合slider的数据字段需求不就不用插槽了吗，哈哈 -->
        <!-- <div class="slider-item" v-if="sliderList.length" v-for="(item, index) in sliderList" :key="index">
          <a :href="item.linkUrl">
            <img :src="item.picUrl">
          </a>
        </div> -->
      </slider>
    </div>
    <div class="recommend-list">

    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend, getDiscList} from 'api/recommend'
  import {ERR_OK} from 'api/config'
  import Slider from 'base/slider/slider'
  export default {
    data() {
      return {
        sliderList: [],
        diskList: []
      }
    },
    computed: {
      sliderData() {
        console.log(this.sliderList)
        return this.sliderList.map((item) => {
          return {
            ...item,
            url: item.linkUrl,
            image: item.picUrl
          }
        })
      }
    },
    created() {
      this._getRecommend()
      this._getDiscList()
    },
    methods: {
      _getRecommend() {
        getRecommend().then((res) => {
          console.log(res.data.slider)
          if (res.code === ERR_OK) {
            this.sliderList = res.data.slider
          }
        })
      },
      _getDiscList() {
        getDiscList().then((res) => {
          console.log(res.data)
          if (res.code === ERR_OK) {
            this.diskList = res.data.slider
          }
        })
      }
    },
    components: {
      Slider
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"

  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .slider-wrapper
      position: relative
      width: 100%
      // /*overflow: hidden*/
    .recommend-list
      .list-title
        height: 65px
        line-height: 65px
        text-align: center
        font-size: $font-size-medium
        color: $color-theme
      .item
        display: flex
        box-sizing: border-box
        align-items: center
        padding: 0 20px 20px 20px
        .icon
          flex: 0 0 60px
          width: 60px
          padding-right: 20px
        .text
          display: flex
          flex-direction: column
          justify-content: center
          flex: 1
          line-height: 20px
          overflow: hidden
          font-size: $font-size-medium
          .name
            margin-bottom: 10px
            color: $color-text
          .desc
            color: $color-text-d
    .loading-container
      position: absolute
      width: 100%
      top: 50%
      transform: translateY(-50%)
</style>
