<template>
  <div ref="wrapper">
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  export default {
    props: {
      probetype: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: true
      },
      data: {
        type: Array,
        default: null
      }
    },
    beforeCreate() {
      console.log('scroll-before-creat')
    },
    beforeMount() {
      console.log('sroll-befor-mount')
    },
    mounted() {
      console.log('scroll-mounted', this.$refs)
      setTimeout(() => {
        this._initScroll()
      }, 20)
    },
    methods: {
      _initScroll() {
        console.log(this.$refs.wrapper)
        if (!this.$refs.wrapper) {
          return
        }
        this.scroll = new BScroll(this.$refs.wrapper, {
          probetype: this.probetype,
          click: this.click
        })
      },
      enable() {
        this.scroll && this.scroll.enable()
      },
      disable() {
        this.scroll && this.scroll.disable()
      },
      refresh() {
        console.log('refresh')
        this.scroll && this.scroll.refresh()
      }
    },
    watch: {
      data() {
        console.log('scroll-watch')
        setTimeout(() => {
          this.refresh()
        }, 20)
      }
    }
  }
</script>

<style lang="stylus" scoped>

</style>

