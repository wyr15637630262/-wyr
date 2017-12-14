
<template>
  <div>
    <m-Breadcrumb mTitle1="店铺管理" mTitle2="店铺列表"></m-Breadcrumb>
  	<div class="storeList_content">
      <div class="query_criteria">
        <div class="query_name"><label>姓名</label><input type="text"></div>
        <div class="query_date"><label>时间</label><input type="text"></div>
        <div class="query_ensure"><input type="button" value="查询"></div>
      </div>
      <table class="storeList_table">
        <thead>
        <tr>
          <td style="width: 10%">编号</td>
          <td style="width: 13%">用户姓名</td>
          <td style="width: 18%">联系方式</td>
          <td style="width: 18%">微信账号</td>
          <td style="width: 18%">注册时间</td>
          <td style="width: 13%">订单数量</td>
          <td style="width: 10%">操作</td>
        </tr>
        </thead>
        <tbody v-if="storeList.length!==0">
        <tr v-for="(item,index) in storeList">
          <td>00{{index+1}}</td>
          <td>{{item.memberNickName}}</td>
          <td>{{item.memberTel}}</td>
          <td>xiaoming08383</td>
          <td>{{item.registeredTime|date('YYYY-MM-DD')}}</td>
          <td>30</td>
          <td><a>删除</a><a style="padding: 0 10px">|</a><a @click="$router.push({name:'storeDetail',params:{userId: 123}})">详情</a></td>
        </tr>
        </tbody>
        <tbody v-else-if="storeList.length===0">
        <tr>
          <td colspan="7" >暂无数据...</td>
        </tr>
        </tbody>
      </table>
      <div class="pages" v-if="storeList.length!==0">
        <el-pagination
          :page-size="storeListPageSize"
          layout="prev, pager, next"
          :total="storeListTotal"
          @click="pageChange">
        </el-pagination>
        <div class="pages_skip">
          <a>到第</a><input type="text" class="page_number" v-model="pageNumber">
          <a>页</a><input type="button" value="确定" class="page_ensure" @click="pageSkip()">
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import $ from 'jquery'
  import rq from './../../http/require.js'
  import mBreadcrumb from './../../component/headerTag.vue'
  export default {
    name: 'storeList',
    data () {
      return {
        storeParams: {page: 1, size: 10}, // 会员列表参数
        storeList: [], // 会员列表
        storeListTotal: 10, // 会员总个数
        storeListPageSize: 10, // 每页个数
        pageNumber: ''
      }
    },
    methods: {
      // 改变页码
      pageChange: function () {
        var that = this
        that.storeParams.page = $('.pages .el-pagination .el-pager .active').html()
        rq.allMember(that.storeParams).then(function (data) {
          if (data.result === '成功') {
            console.log(data)
            that.storeList = data.data.data
          } else if (data.result === '失败') {
            that.storeList = []
          }
        })
      },
      // 页码跳转
      pageSkip: function () {
        var that = this
        that.storeParams.page = that.pageNumber
        rq.allMember(that.storeParams).then(function (data) {
          if (data.result === '成功') {
            console.log(data)
            that.storeList = data.data.data
          } else if (data.result === '失败') {
            that.storeList = []
          }
        })
      }
    },
    mounted () {
      var that = this
      //  $('.pages .el-pagination button.btn-prev').html('上一页')
      //  $('.pages .el-pagination button.btn-next').html('下一页')
      rq.allMember(that.storeParams).then(function (data) {
        if (data.result === '成功') {
          console.log(data)
          that.storeList = data.data.data
          that.storeListTotal = data.data.totalRecord
          that.storeListPageSize = data.data.pageSize
        } else if (data.result === '失败') {
          that.storeList = []
        }
      })
    },
    components: {
      mBreadcrumb
    }
  }
</script>
<style>
  .pages .el-pagination .el-pager li.number{
    margin-right: 5px;
    border:1px solid #9c9b9c;
  }
  .pages .el-pagination .el-pager li.number:hover{
    color: #ec6a47;
  }
  .pages .el-pagination button:hover{
    color: #ec6a47;
  }
  .pages .el-pagination .el-pager li.active{
    color: #494d56;
    border:1px solid #494d56;
  }
</style>
<!--引入样式-->
<style lang="scss" scoped>@import "storeList";</style>
