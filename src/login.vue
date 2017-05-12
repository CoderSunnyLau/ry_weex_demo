<template>
  <div class="wrapper">
    <image class="mainPic" :src="mainSrc"></image>
    <div class="login-wrapper">
      <div><text class="warning-msg">{{warningMsg}}</text></div>
      <div class="login-row"><input class="input" type="text" placeholder="用戶名" v-model="userName"/></div>
      <div class="login-row"><input class="input" type="password" placeholder="密碼" v-model="password"/></div>
    </div>
    <div class="login-btn" v-on:click="login"><text class="login-text">立即登录</text></div>
    <div class="test" v-on:click="test"><text>Test</text></div>
    <div class="test" @click="test3"><text>test3</text></div>
    <div class="test" @click="test8"><text>test8</text></div>
    <div><text>{{postResult}}</text></div>
    <div><text>{{resData}}</text></div>
  </div>
</template>

<style>
  .wrapper{
    flex-direction: column;
    align-items: center;
  }
  .mainPic{
    width: 750px;
    height: 220px;
  }
  .login-wrapper{
    margin-top: 100px;
    flex-direction: column;
    align-items: center;
  }
  .warning-msg{
    font-size: 35px;
    height: 80px;
    margin-bottom: 20px;
    color: red;
    line-height: 80px;
  }
  .login-row{
    width: 750px;
    height: 100px;
    margin-bottom: 10px;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
  .input{
    width: 550px;
    height: 100px;
    border-width: 1px;
    border-color: #CCC;
    border-radius: 10px;
    padding-left: 20px;
    font-size: 35px;
  }
  .login-btn{
    width: 550px;
    height: 100px;
    margin-top: 50px;
    background-color: #F50;
    justify-content: center;
    align-items: center;
    border-radius: 10px;
  }
  .login-text{
    color: #FFF;
    font-size: 40px;
  }
  .test{
    height: 50px;
    width: 100px;
    background-color: #F00;
  }
</style>

<script>
  import md5 from './md5.js'
  import axios from 'axios'
  const stream = weex.requireModule('stream')
  const storage = weex.requireModule('storage')
  const navigator = weex.requireModule('navigator')

  export default {
    data () {
      return {
        mainSrc: 'http://m.ry600.com/snapshot/vms/site/di/di70684438lrfavs/82ojywir5vqogktm/image/login_ad.jpg',
        userName: 'ryliuwf',
        password: 'ryliuwf1',
        warningMsg: '',
        resData: 'resData',
        postResult: 'aaa',
        serviceId:''
      }
    },
    methods: {
      login: function () {
        const vm = this
        if (vm.userName == '') {
          vm.warningMsg = '用戶名不能为空'
          return false
        } else if (vm.password == '') {
          vm.warningMsg = '密码不能为空'
          return false
        } else {
          vm.warningMsg = ''
        }
        let psd = md5(vm.password)
        return stream.fetch({
          method: 'GET',
          type: 'jsonp',
          url: 'http://login.ry600.com/userCenterLogin.jsp?&act=login&captcha=&userName=' + vm.userName + '&password=' + psd + '&_=' + new Date().getTime()
        }, function (res) {
          console.log(res)
          vm.resData = res
          if (res.ok) {
            if (res.data.success) {
              // 登录成功
              vm.warningMsg = '登录成功'
            } else {
              // 登录失败
              vm.warningMsg = res.data.message
            }
          } else {
            vm.warningMsg = '网络错误'
          }
        })
      },
      test8: function () {
        const vm = this
        console.log(weex.config);
        let psd = md5("123456")
        stream.fetch({
          method: 'GET',
          type: 'jsonp',
          url: 'https://www.yr600.com/fish/login?userName=wbyy&password=' + psd
        }, function (res) {
          if(!res.ok){
              vm.postResult = "request failed";
          }else{
              console.log(res)
              vm.resData = res.data;
              if(res.data.success){
                vm.serviceId = res.data._serviceId;
                storage.setItem('serviceId', res.data._serviceId, event => {
                  storage.getItem('serviceId', event => {
                  console.log('get value:', event.data)
                })
              })
            }
          }
        }, function (err) {
          console.log(err)
        })
      },
      test: function () {
        const vm = this

        stream.fetch({
          headers: {'Cookie':'_serviceId=' + vm.serviceId},
          method: 'POST',
          type: 'json',
          url: 'https://www.yr600.com/jsonaction/websiteaction.action',
          body: 'action=VSUser.getBasicInfo'
          // url: 'http://fanxz.cn:3000/sign',
          // body: 'username=zfxmnb&password=123453424&type=login'
        }, function (res) {
          if(!res.ok){
              vm.postResult = "request failed";
            }else{
              console.log(res)
              vm.postResult = res.data.results;
            }
          // vm.resData = res
        }, function (err) {
          console.log(err)
        })
      },
      test3: function() {
        var url = this.$getConfig().bundleUrl;
        url = url.split('/').slice(0,-1).join('/')+'/myAccount.weex.js';
        console.log(url);
        var params = {
          'url': url,
          'animated' : 'true',
        }
        navigator.push(params,function(){});
      },
      test2: function () {
        console.log("test2");
        const me = this;
        stream.fetch({
          method: 'POST',
          url: 'http://httpbin.org/post',
          body:'type=weexType&page=weexPage',
          type:'json'
        }, function(ret) {
          if(!ret.ok){
            me.postResult = "request failed";
          }else{
            console.log('get:'+JSON.stringify(ret));
            me.postResult = JSON.stringify(ret.data);
          }
        },function(response){
          console.log('get in progress:'+response.length);
          me.postResult = "bytes received:"+response.length;
      });
      },
      test4: function () {
        const me = this
        let psd = md5(me.password)
        let url = 'http://login.ry600.com/userCenterLogin.jsp?&act=login&captcha=&userName=' + me.userName + '&password=' + psd + '&_=' + new Date().getTime()
        axios.get(url).then((response) => {
          console.log(response)
        }, (response) => {
          console.log("error")
        })
      }
    }
  }
</script>
