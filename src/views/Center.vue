<template>
  <div class="layui-container fly-marginTop fly-user-main">
    <ul class="layui-nav layui-nav-tree" lay-filter="test">
      <!-- 侧边导航: <ul class="layui-nav layui-nav-tree layui-nav-side"> -->
      <li class="layui-nav-item" v-for="(item,index) in lists" :key="'center' + index">
        <router-link :to="{name: item.link, params: {uid: uid}}" :active-class="item.activeClass">
          <i class="layui-icon" :class="item.icon"></i>
          {{item.name}}
        </router-link>
      </li>
    </ul>
    <router-view></router-view>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'center',
  data () {
    return {
      lists: [
        {
          name: 'Home',
          icon: 'layui-icon-home',
          link: 'home'
        },
        {
          name: 'Member center',
          icon: 'layui-icon-friends',
          link: 'center'
        },
        {
          name: 'Settings',
          icon: 'layui-icon-set',
          link: 'info',
          activeClass: 'layui-this'
        },
        {
          name: 'My posts',
          icon: 'layui-icon-form',
          link: 'mypost',
          activeClass: 'layui-this'
        },
        {
          name: 'My news',
          icon: 'layui-icon-reply-fill',
          link: 'msg'
        },
        {
          name: 'Others',
          icon: 'layui-icon-component',
          link: 'others'
        }
      ]
    }
  },
  computed: {
    ...mapGetters(['uid'])
  }
}
</script>

<style lang="scss" scoped>
.icontoimc {
  margin-right: 10px;
}
.layui-nav {
  &.layui-nav-tree {
    background-color: #f8f8f8;
    > li {
      i {
        color: #666;
      }
    }
    .layui-nav-item {
      a {
        &.layui-this {
          background-color: rgba(0, 0, 0, 0.16);
        }
        &:hover {
          background-color: rgba(0, 0, 0, 0.16);
        }
        color: #666;
      }
    }
  }
}
</style>
