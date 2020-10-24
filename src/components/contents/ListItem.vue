<template>
  <div>
    <ul class="fly-list">
      <li v-for="(item,index) in items" :key="'listitem' + index">
        <router-link class="fly-avatar" :to="{name: 'home', params: {uid: item.uid._id}}" link>
          <img :src="item.uid.pic ? item.uid.pic : '/img/header.jpg'" alt="avatar" />
        </router-link>
        <h2>
          <a class="layui-badge">{{item.catalog}}</a>
          <router-link :to="{name: 'detail', params: {tid: item._id}}">{{item.title}}</router-link>
        </h2>
        <div class="fly-list-info">
          <router-link :to="{name: 'home', params: {uid: item.uid._id}}" link>
            <cite>{{item.uid.name}}</cite>
            <!--<i class="iconfont icon-renzheng" title="认证信息：XXX"></i>-->
            <i
              class="layui-badge fly-badge-vip"
              v-if="item.uid.isVip !== '0'"
            >{{'VIP' + item.uid.isVip}}</i>
          </router-link>
          <span>{{item.created | moment}}</span>

          <span class="fly-list-kiss layui-hide-xs" title="悬赏飞吻">
            <i class="iconfont icon-kiss"></i>
            {{item.fav}}
          </span>
          <span class="layui-badge fly-badge-accept layui-hide-xs" v-show="item.status !== '0'">已结</span>
          <span class="fly-list-nums">
            <i class="iconfont icon-pinglun1" title="回答"></i>
            {{item.answer}}
          </span>
        </div>
        <div class="fly-list-badge" v-show="item.tags.length > 0 && item.tags[0].name !== ''">
          <span
            class="layui-badge"
            v-for="(tag, index) in item.tags"
            :key="'tag' + index"
            :class="tag.class"
          >{{tag.name}}</span>
        </div>
      </li>
    </ul>
    <div style="text-align: center" v-show="isShow">
      <div class="laypage-main" v-if="!isEnd">
        <a @click.prevent="more()" class="laypage-next">More questions</a>
      </div>
      <div class="nomore gray" v-else>No more questions</div>
    </div>
  </div>
</template>

<script>
import config from '@/config'
const baseUrl = process.env.NODE_ENV === 'development' ? config.baseUrl.dev : config.baseUrl.pro

export default {
  name: 'listitem',
  props: {
    lists: {
      default: () => [],
      type: Array
    },
    isShow: {
      default: true,
      type: Boolean
    },
    isEnd: {
      default: false,
      type: Boolean
    }
  },
  computed: {
    items () {
      this.lists.map((item) => {
        if (item.uid && item.uid.pic) {
          item.uid.pic = `${baseUrl}${item.uid.pic}`
        }

        switch (item.catalog) {
          case 'ask':
            item.catalog = 'Question'
            break
          case 'share':
            item.catalog = 'Share'
            break
          case 'logs':
            item.catalog = 'News'
            break
          case 'notice':
            item.catalog = 'Announcement'
            break
          case 'advise':
            item.catalog = 'Advise'
            break
          case 'discuss':
            item.catalog = 'Discuss'
            break
        }
      })
      return this.lists
    }
  },
  methods: {
    more () {
      this.$emit('nextpage')
    }
  }
  // filters: {
  //   moment (date) {
  //     // 超过7天，显示日期
  //     if (moment(date).isBefore(moment().subtract(7, 'days'))) {
  //       return moment(date).format('YYYY-MM-DD')
  //     } else {
  //       // 1小前，xx小时前，X天前
  //       return moment(date).locale('zh-cn').from(moment())
  //     }
  //   }
  // }
}
</script>

<style lang="scss" scoped>
.nomore {
  font-size: 16px;
  padding: 30px 0;
}
</style>
