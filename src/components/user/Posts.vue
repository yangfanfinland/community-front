<template>
  <div class="fly-panel fly-panel-user" pad20>
    <div class="layui-tab layui-tab-brief">
      <ul class="layui-tab-title">
        <li>
          <router-link :to="{name: 'mypost'}">My posts</router-link>
        </li>
        <li>
          <router-link :to="{name: 'mycollection'}">My favorite posts</router-link>
        </li>
      </ul>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: 'user-post'
}
</script>

<style lang="scss" scoped>
</style>
