<template>
  <div class="fly-panel fly-column">
    <div class="layui-container">
      <ul class="layui-clear">
        <router-link tag="li" to="/" class="layui-hide-xs">
          <a href="/">Main</a>
        </router-link>
        <router-link v-for="(item,index) in lists" :key="'panel'+ index" tag="li" :to="item.path">
          <a href="jie/index.html">
            {{item.name}}
            <span class="layui-badge-dot" v-if="item.isNew"></span>
          </a>
        </router-link>
        <li class="layui-hide-xs layui-hide-sm layui-show-md-inline-block">
          <span class="fly-mid"></span>
        </li>

        <!-- 用户登入后显示 -->
        <template v-if="isLogin">
          <li class="layui-hide-xs layui-hide-sm layui-show-md-inline-block">
            <router-link :to="{name: 'mypost'}">My posts</router-link>
          </li>
          <li class="layui-hide-xs layui-hide-sm layui-show-md-inline-block">
            <router-link :to="{name: 'mycollection'}">My favorite posts</router-link>
          </li>
        </template>
      </ul>

      <div class="fly-column-right layui-hide-xs">
        <span class="fly-search">
          <i class="layui-icon"></i>
        </span>
        <router-link :to="{'name': 'add'}" class="layui-btn">New post</router-link>
      </div>
      <div
        class="layui-hide-sm layui-show-xs-block"
        style="margin-top: -10px; padding-bottom: 10px; text-align: center;"
      >
        <router-link :to="{'name': 'add'}" class="layui-btn">New post</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'panel',
  data () {
    return {
      lists: [
        {
          name: 'Question',
          path: '/index/ask',
          isNew: false
        },
        {
          name: 'Share',
          path: '/index/share',
          isNew: true
        },
        {
          name: 'Discuss',
          path: '/index/discuss',
          isNew: false
        },
        {
          name: 'Advise',
          path: '/index/advise',
          isNew: false
        },
        {
          name: 'Announcement',
          path: '/index/notice',
          isNew: false
        },
        {
          name: 'News',
          path: '/index/logs',
          isNew: false
        }
      ],
      isLogin: this.$store.state.isLogin
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
