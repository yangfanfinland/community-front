<template>
  <div class="modal" v-show="isShow">
    <div class="mask" @click="close()"></div>
    <div class="layui-layer layui-layer-page" :class="{'active': isShow}">
      <div class="layui-layer-title">
        Sign-in instruction
        <i class="layui-icon layui-icon-close pull-right" @click="close()"></i>
      </div>
      <div class="layui-layer-content">
        <div class="layui-text">
          <blockquote class="layui-elem-quote">The "Sign-in" rule as follow:</blockquote>
          <table class="layui-table">
            <thead>
              <tr>
                <th>Continuous</th>
                <th>Score / Day</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>&lt;5</td>
                <td>5</td>
              </tr>
              <tr>
                <td>&ge;5</td>
                <td>10</td>
              </tr>
              <tr>
                <td>&ge;15</td>
                <td>15</td>
              </tr>
              <tr>
                <td>&ge;30</td>
                <td>20</td>
              </tr>
              <tr>
                <td>&ge;100</td>
                <td>30</td>
              </tr>
              <tr>
                <td>&ge;365</td>
                <td>50</td>
              </tr>
            </tbody>
          </table>
          <div>
            <p class="orange">Recalculate if break in middle</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'sign-info',
  props: {
    isShow: {
      default: false,
      type: Boolean
    }
  },
  methods: {
    close () {
      this.$emit('closeModal')
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
