<template>
  <div class="wrapper account">
    <div class="title"><text>我的賬戶</text></div>
    <div class="info">
      <div class="avatar">
        <image></image>
        <text></text>
      </div>
      <div class="detail">
        <div><text>姓名：</text></div>
        <div><text>昵称：</text></div>
        <div><text>机构：</text></div>
      </div>
    </div>
    <div class="menu">
      <div><text>我的订单</text></div>
      <div><text>我的退换货</text></div>
      <div><text>订购申请</text></div>
      <div><text>我的账簿</text></div>
      <div><text>收货地址</text></div>
      <div><text>我的评价</text></div>
      <div><text>我的留言</text></div>
      <div><text>我的收藏</text></div>
    </div>
  </div>
</template>

<style>
  .title{
    width: 750px;
    height: 50px;
    background-color: #F50;
    color:#FFF;
    text-align: center;
  }
</style>

<script>
  import axios from 'axios'
  const stream = weex.requireModule('stream')
  const storage = weex.requireModule('storage')

  export default {
    data () {
      return {
      }
    },
    methods: {
    }
  }
</script>
