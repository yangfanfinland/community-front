<template>
  <div>
    <div class="fly-home fly-panel" style="background-image: url();">
      <img :src="userInfo.pic ? userInfo.pic : '/img/bear-200-200.jpg'" />
      <!-- <i class="iconfont icon-renzheng" title="Imooc社区认证"></i> -->
      <h1>
        {{userInfo.name}}
        <i class="iconfont icon-nan"></i>
        <!-- <i class="iconfont icon-nv"></i>  -->
        <i
          class="layui-badge fly-badge-vip"
        >{{userInfo.isVip === '0' ? '非VIP': 'VIP' + userInfo.isVip}}</i>
        <!--
    <span style="color:#c00;">（管理员）</span>
    <span style="color:#00359B;">（社区之光）</span>
    <span>（该号已被封）</span>
        -->
      </h1>

      <!-- <p style="padding: 10px 0; color: #00359B;">认证信息：layui 作者</p> -->

      <p class="fly-home-info">
        <i class="iconfont icon-kiss" title="飞吻"></i>
        <span style="color: #FF7200;">{{userInfo.favs}} points</span>
        <i class="iconfont icon-shijian"></i>
        <span>{{userInfo.created | moment}} joined</span>
        <i class="iconfont icon-chengshi"></i>
        <span>{{userInfo.location}}</span>
      </p>

      <p class="fly-home-sign">{{userInfo.regmark}}</p>
    </div>

    <div class="layui-container">
      <div class="layui-row layui-col-space15">
        <div class="layui-col-md6 fly-home-jie">
          <div class="fly-panel">
            <h3 class="fly-panel-title">{{userInfo.name}} recent questions</h3>
            <ul class="jie-row">
              <li v-for="(item,index) in postList" :key="'postlist' + index">
                <div
                  class="fly-list-badge"
                  v-show="item.tags.length > 0 && item.tags[0].name !== ''"
                >
                  <span
                    class="layui-badge"
                    v-for="(tag, index) in item.tags"
                    :key="'tag' + index"
                    :class="tag.class"
                  >{{tag.name}}</span>
                </div>
                <router-link
                  class="jie-title link"
                  :to="{name:'detail', params: {tid: item._id}}"
                >{{item.title}}</router-link>
                <i>{{item.created | moment}}</i>
                <em class="layui-hide-xs">{{item.reads}} views / {{item.answer}} answers</em>
              </li>
              <div
                v-show="postList.length === 0"
                class="fly-none"
                style="min-height: 50px; padding:30px 0; height:auto;"
              >
                <i style="font-size:14px;">No posted questions</i>
              </div>
            </ul>
          </div>
        </div>

        <div class="layui-col-md6 fly-home-da">
          <div class="fly-panel">
            <h3 class="fly-panel-title">{{userInfo.name}} recent answers</h3>
            <ul class="home-jieda">
              <li v-for="(item,index) in commentList" :key="'comments' + index">
                <p>
                  <span>{{item.created | moment}}</span>
                  answered in
                  <router-link
                    :to="item.tid? {name: 'detail', params: {tid: item.tid? item.tid._id : ''}} : {name: '404'}"
                  >{{item.tid ? item.tid.title: '(post deleted by author)'}}</router-link>:
                </p>
                <div class="home-dacontent" v-richtext="item.content"></div>
              </li>
              <div
                v-show="commentList.length === 0"
                class="fly-none"
                style="min-height: 50px; padding:30px 0; height:auto;"
              >
                <span>No answered questions</span>
              </div>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getInfo, getCommentList, getPostPublic } from '@/api/user'
import config from '@/config'
const baseUrl = process.env.NODE_ENV === 'development' ? config.baseUrl.dev : config.baseUrl.pro

export default {
  name: 'home',
  props: ['uid'],
  data () {
    return {
      postList: [],
      commentList: [],
      userInfo: {}
    }
  },
  mounted () {
    this.getUserInfo()
    this.getPostList()
    this.getCommentList()
  },
  methods: {
    getUserInfo () {
      // 获取用户的基本信息
      getInfo({ uid: this.uid || 123 }).then((res) => {
        if (res.code === 200) {
          if (res.data && res.data.pic) {
            this.userInfo = { ...res.data, pic: `${baseUrl}${res.data.pic}` }
          } else {
            this.userInfo = res.data
          }
        }
      })
    },
    getPostList () {
      // 获取最近的发贴列表
      getPostPublic({
        uid: this.uid,
        page: 0,
        limit: 20
      }).then((res) => {
        if (res.code === 200) {
          this.postList = res.data
        }
      })
    },
    getCommentList () {
      // 获取最近的评论信息
      getCommentList({
        uid: this.uid,
        page: 0,
        limit: 20
      }).then((res) => {
        if (res.code === 200) {
          this.commentList = res.data
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
