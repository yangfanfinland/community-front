<template>
  <div class="fly-footer">
    <p>
      <a href="https://www.eurostep.com/" target="_blank">ShareAspace community</a>
      &copy; ShareAspace 2020
    </p>
  </div>
</template>

<script>
export default {
  name: 'Footer',
  data () {
    return {
      isShow: false
    }
  },
  methods: {
    show () {
      this.isShow = true
    },
    hide () {
      setTimeout(() => {
        this.isShow = false
      }, 500)
    }
  }
}
</script>

<style lang="scss" scoped>
.qr {
  width: 120px;
  height: 120px;
  position: absolute;
  left: 0;
  top: -130px;
}
</style>
