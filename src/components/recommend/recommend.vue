<template>
  <div class="recommend">
    <!-- scroll 组件一定要定好高度（窗口大小），不然子内容没法滚动 -->
    <scroll class="recommend-content" :data="discList" ref="scroll">
      <div class="slider-wrapper">
        <slider :sliderData="sliderData" @emitImgLoad="scrollRefresh">
          <!-- 之所以用插槽，为了使组件通用！组件内item的每个具体字段，每个接口返回的肯定是不一样的。那我获取数据后改造的复合slider的数据字段需求不就不用插槽了吗，哈哈 -->
        </slider>
      </div>
      <div class="recommend-list">
        <h1 class="list-title">热门歌曲推荐</h1>
        <ul class="recommend-list-wrapper">
          <li v-if="discList" v-for="(item, index) in discList" class="list-item" :key="'discList'+index">
            <!-- src必须要绑定才能认为是表达式，不然就是字符串 -->
            <img width="60" height="60" class="item-avatar" v-lazy="item.imgurl" alt="">
            <div class="item-desc">
              <h2 class="name">{{item.creator.name}}</h2>
              <span class="desc">{{item.dissname}}</span>
            </div>
          </li>
        </ul>
      </div>
    </scroll>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend, getDiscList} from 'api/recommend'
  import {ERR_OK} from 'api/config'
  import Slider from 'base/slider/slider'
  import Scroll from 'base/scroll/scroll'
  export default {
    data() {
      console.log('rec-data-observer-1')
      return {
        sliderList: [],
        discList: []
      }
    },
    computed: {
      sliderData() {
        console.log('rec-data-computed-observer-2')
        return this.sliderList.map((item) => {
          return {
            ...item,
            url: item.linkUrl,
            image: item.picUrl
          }
        })
      }
    },
    beforeCreate () {
      console.log('rec-before')
    },
    created() {
      console.log('rec-creat')
      setTimeout(this._getRecommend, 2000)
      // this._getRecommend()
      this._getDiscList()
    },
    beforeMount() {
      console.log('rec-befor-mount')
    },
    mounted() {
      console.log('rec-mounted')
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
          console.log(res)
          if (res.code === ERR_OK) {
            this.discList = res.data.list
            console.log(this.discList)
          }
        })
      },
      scrollRefresh() {
        if (this.isEmitedImgLoaded) return
        this.$refs.scroll.refresh()
        this.isEmitedImgLoaded = true
      }
    },
    components: {
      Slider,
      Scroll
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
    .recommend-content
      height 100%
      overflow hidden
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
      .list-item
        display: flex
        box-sizing: border-box
        align-items: center
        padding: 0 20px 20px
        .item-avatar
          flex: 0 0 60px
          width: 60px
          padding-right: 20px
        .item-desc
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
