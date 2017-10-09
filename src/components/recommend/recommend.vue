<template>
  <div class="recommend">
    <div class="slider-wrapper">

    </div>
    <div class="recommend-list">

    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getRecommend} from 'api/recommend'
  import {ERR_OK} from 'api/config'

  export default {
    data() {
      return {
        sliderList: []
      }
    },
    created() {
      this._getRecommend()
    },
    methods: {
      _getRecommend() {
        console.log(1)
        getRecommend().then((res) => {
          console.log(res)
          if (res.status === ERR_OK) {
            this.sliderList = res.data.slider
          }
        })
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
