<template>
  <div class="fly-header layui-bg-black">
    <div class="layui-container">
      <a class="fly-logo" href="/">
        <img src="../assets/img/site-logo-dark.svg" width="160" alt="ShareAspace" />
      </a>
      <!-- <ul class="layui-nav fly-nav layui-hide-xs">
        <li class="layui-nav-item layui-this">
          <a href="/">
            <i class="iconfont icon-jiaoliu"></i>Communicate
          </a>
        </li>
      </ul> -->

      <ul class="layui-nav fly-nav-user">
        <!-- 未登入的状态 -->
        <template v-if="!isShow">
          <li class="layui-nav-item">
            <router-link class="iconfont icon-touxiang layui-hide-xs" to="/user123123"></router-link>
          </li>
          <li class="layui-nav-item">
            <router-link :to="{name: 'login'}">Login</router-link>
          </li>
          <li class="layui-nav-item">
            <router-link :to="{name: 'reg'}">Register</router-link>
          </li>
        </template>

        <!-- 登入后的状态 -->
        <template v-else>
          <!-- 调整了Hover的区域 -->
          <li class="layui-nav-item" @mouseover="show()" @mouseleave="hide()">
            <router-link class="fly-nav-avatar" :to="{name: 'center'}">
              <cite class="layui-hide-xs">{{userInfo.name}}</cite>
              <!-- <i class="iconfont icon-renzheng layui-hide-xs" title="认证信息：layui 作者"></i> -->
              <i
                class="layui-badge fly-badge-vip layui-hide-xs"
                v-show="userInfo.isVip !== '0'"
              >VIP{{userInfo.isVip}}</i>
              <img :src="userInfo.pic" />
            </router-link>
            <dl
              class="layui-nav-child layui-anim layui-anim-upbit"
              :class="{'layui-show': isHover}"
            >
              <dd>
                <router-link :to="{name: 'info'}">
                  <i class="layui-icon">&#xe620;</i>Settings
                </router-link>
              </dd>
              <dd>
                <router-link :to="{name: 'msg'}">
                  <i class="iconfont icon-tongzhi" style="top: 4px;"></i>Message
                </router-link>
              </dd>
              <dd>
                <router-link :to="{name: 'home', params: {uid: userInfo._id}}">
                  <i class="layui-icon" style="margin-left: 2px; font-size: 22px;">&#xe68e;</i>Home
                </router-link>
              </dd>
              <hr style="margin: 5px 0;" />
              <dd>
                <a href="javascript: void(0)" style="text-align: center;" @click="logout()">Logout</a>
              </dd>
            </dl>
          </li>
          <div class="fly-nav-msg" v-show="num.message && num.message !== 0">{{num.message}}</div>
          <transition name="fade">
            <div class="layui-layer-tips" v-show="hasMsg">
              <div class="layui-layer-content">
                {{num.message}} unread message
                <i class="layui-layer-TipsG layui-layer-TipsB"></i>
              </div>
            </div>
          </transition>
        </template>
      </ul>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import config from '@/config'
const baseUrl = process.env.NODE_ENV === 'development' ? config.baseUrl.dev : config.baseUrl.pro

export default {
  name: 'Header',
  data () {
    return {
      isHover: false,
      hoverCtrl: {},
      hasMsg: false
    }
  },
  methods: {
    show () {
      // 当用户的鼠标移入头像的时候，去显示操作菜单
      clearTimeout(this.hoverCtrl)
      this.hoverCtrl = setTimeout(() => {
        this.isHover = true
      }, 200)
    },
    hide () {
      // 当用户的鼠标移出头像的时候，去隐藏操作菜单
      clearTimeout(this.hoverCtrl)
      this.hoverCtrl = setTimeout(() => {
        this.isHover = false
      }, 500)
    },
    logout () {
      this.$confirm(
        'Confirm to logout?',
        () => {
          localStorage.clear()
          this.$store.commit('setToken', '')
          this.$store.commit('setUserInfo', {})
          this.$store.commit('setIsLogin', false)
          this.$router.push({ name: 'index' }, () => {})
        },
        () => {}
      )
    }
  },
  watch: {
    num (newval, oldval) {
      if (newval.event && newval !== oldval) {
        // 判断消息数量
        if (newval.message && newval.message > 0) {
          this.hasMsg = true
          setTimeout(() => {
            this.hasMsg = false
          }, 2000)
        }
      }
    }
  },
  computed: {
    // num () {
    //  return this.$store.state.num
    // }
    ...mapState({
      num: (state) => state.num
    }),
    isShow () {
      return this.$store.state.isLogin
    },
    userInfo () {
      if (this.$store.state.userInfo) {
        return { ...this.$store.state.userInfo, pic: `${baseUrl}${this.$store.state.userInfo.pic}` }
      }
      return {
        name: '',
        pic: '',
        isVip: '0'
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.fly-logo {
  left: -15px;
  top: 8px;
  margin-left: 15px;
}
.layui-layer-tips {
  position: absolute;
  white-space: nowrap;
  right: 0;
  top: 60px;
  z-index: 2000;
}
</style>
