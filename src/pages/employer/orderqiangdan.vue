<template>
  <div class="">
    <div class="header">
      <div class="header-left" v-tap="{ methods:goback }" ><img src="../../../static/images/back.png"/></div>
      <span>填写抢单信息</span>
      <div class="header-right" v-tap="{ methods:submit }" >确定</div>
    </div>
    <!--  信息表-->
    <div class="oq-content">
      <div class="oq-list">
        <div class="oq-left"><span>我的报价</span></div>
        <div class="oq-right"><input type="text" placeholder="请输入报价" v-model="subParams.quotation"/></div>
      </div>
      <div class="oq-list">
        <div class="oq-left"><span>任务时长</span></div>
        <div class="oq-right"><input type="text" placeholder="请输入时长" v-model="subParams.taskTime"/></div>
      </div>
      <div class="fangan">
        <div class="tle"><span>方案说明</span></div>
        <textarea class="xt-txt" placeholder="请详细描述一下方案的内容" v-model="subParams.schemeExplains"></textarea>
      </div>
    </div>
    <toast v-model="showMark" :time="1000" type="text" width="5rem">{{showMsg}}</toast>
  </div>
</template>

<script>
  import { Toast } from 'vux'
  import common from '../../../static/common';
  import store from '@/vuex/store'
  export default {
    components: {
      Toast
    },
    store,
    data () {
      return {
        subParams:{
          order_id:null,
          user_id:"",
          quotation:"",
          taskTime:"",
          schemeExplains:"",
        },
        owMark:false,
        showMsg:"",
      }
    },
    created: function () {
      this.userInfo = common.getObjStorage("userInfo");
      this.subParams.order_id = this.$route.query.id;
      this.subParams.user_id = this.userInfo._id;
    },
    methods: {
      goback(){
        this.$router.goBack();
      },
      toUrl: function (pagename) {
        this.$router.push({name: pagename})
      },

      /**************************************/
      showToast(msg){
        this.showMark = true;
        this.showMsg = msg;
      },

      submit(){
        var _self = this;
        if( common.isNull(_self.subParams.user_id) == true ){
          _self.showToast("未成功获取用户信息!");
          return
        }
        if( common.isNull(_self.subParams.quotation) == true ){
          _self.showToast("请输入报价!");
          return
        }
        if( common.isNull(_self.subParams.taskTime) == true ){
          _self.showToast("请输入时长!");
          return
        }
//        if( common.isNull(_self.subParams.schemeExplains) == true ){
//          _self.showToast("未成功获取用户信息!");
//          return
//        }

        var params = {
          //批量添加
          interfaceId:'competiteAnOrder',
          data:_self.subParams
        }

        this.$axios.post('/api/mongoApi',{
          params:params
        }).then((response)=>{
          console.log(response);
          if( response ){
            var data = response.data;
            if( data && data.code == 200 ){
              this.$store.state.indexRefreshMark = 1;
            //   _self.showToast("抢单成功！");
            //   _self.goback();
            _self.toUrl("competiteSuccess")
            }else{
              _self.showToast("抢单失败！");
            }
          }
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import "../../assets/css/employer/order-qiangdan.css";
</style>
