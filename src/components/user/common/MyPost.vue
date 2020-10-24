<template>
  <div>
    <div class="overflow">
      <table class="layui-table" border="0">
        <thead>
          <tr class="layui-table-header">
            <th>
              <div class="layui-table-cell pl0">
                <span>Title</span>
              </div>
            </th>
            <th>
              <div class="layui-table-cell">
                <span>State</span>
              </div>
            </th>
            <th>
              <div class="layui-table-cell">
                <span>End</span>
              </div>
            </th>
            <th>
              <div class="layui-table-cell">
                <span>Post time</span>
              </div>
            </th>
            <th>
              <div class="layui-table-cell">
                <span>Data</span>
              </div>
            </th>
            <th class="min-cell">
              <div class="layui-table-cell">
                <span>Operate</span>
              </div>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr class="text-center" v-for="(item,index) in list" :key="'mypost' + index">
            <td class="text-left">
              <router-link
                class="link"
                :to="{name: 'detail', params: {tid: item._id}}"
              >{{item.title}}</router-link>
            </td>
            <td>{{item.status === '0' ? 'Open': 'Closed'}}</td>
            <td :class="{'success': item.isEnd !=='0'}">{{item.isEnd === '0'? 'No': 'Yes'}}</td>
            <td>{{item.created | moment}}</td>
            <td>{{item.reads}} views / {{item.answer}} answers</td>
            <td>
              <div
                class="layui-btn layui-btn-xs"
                :class="{'layui-btn-disabled': item.isEnd === '1'}"
                @click="editPost(item)"
              >Edit</div>
              <div class="layui-btn layui-btn-xs layui-btn-danger" @click="deletePost(item)">Delete</div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <imooc-page
      v-show="total > 1"
      :total="total"
      :current="current"
      :align="'left'"
      :hasTotal="true"
      :hasSelect="true"
      @changeCurrent="handleChange"
    ></imooc-page>
  </div>
</template>

<script>
import { getPostListByUid, deletePostByUid } from '@/api/user'
import Pagination from '@/components/modules/pagination/Index'
export default {
  name: 'my-post',
  components: {
    'imooc-page': Pagination
  },
  data () {
    return {
      list: [],
      total: 0,
      current: 0,
      page: 0,
      limit: 10
    }
  },
  mounted () {
    this.getPostList()
  },
  methods: {
    getPostList () {
      getPostListByUid({
        page: this.current,
        limit: this.limit
      }).then((res) => {
        if (res.code === 200) {
          this.list = res.data
          this.total = res.total
        }
      })
    },
    deletePost (item) {
      this.$confirm('Confirm delete?', () => {
        if (item.isEnd !== '0') {
          this.$pop('shake', 'Can not delete end post!')
          return
        }
        deletePostByUid({
          tid: item._id
        }).then((res) => {
          if (res.code === 200) {
            this.$pop('', 'Delete success')
            this.list.splice(this.list.indexOf(item), 1)
          } else {
            this.$pop('shake', res.msg)
          }
        })
      }, () => {

      })
    },
    handleChange (val) {
      this.current = val
      this.getPostList()
    },
    editPost (item) {
      if (item.isEnd === '1') {
        this.$pop('shake', 'Can not edit end post!')
      } else {
        this.$router.push({
          name: 'edit',
          params: { tid: item._id, page: item }
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.overflow {
  overflow-y: auto;
}
.min-cell {
  min-width: 80px;
}
.layui-table-header {
  th {
    text-align: center;
    &:first-child {
      text-align: left;
    }
  }
}
</style>
