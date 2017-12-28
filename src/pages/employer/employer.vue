<template>
  <div class="employer" style="background: #f2f2f2">
    <!--头部导航-->
    <div class="header">
      <span>雇主</span>
      <div class="header-right "@click="toUrl('message')">
        <img src="../../../static/images/employer/ling.png" />
        <div class="hongdian"></div>
      </div>
    </div>
    <!--状态模块-->
    <div class="gu-mokuai">
      <div class="dingdan" @click="toUrl('fabudingdan',true)">
        发 布 项 目
      </div>
      <div class="shouchang" @click="toUrl('myorderchuli',true)">
        我 的 订 单
      </div>
    </div>
    <!--智能排序-->
    <div class="id-znpx">
      <div class="xian" @click="znbx()">
        <p :class="znpxMark==true ? 'up' : ''">智能排序</p>
      </div>
      <div class="area" v-if="znpxMark">
        <ul>
          <li @click="sort(0)">智能排序</li>
          <li @click="sort(1)">人气最高</li>
          <li @click="sort(0)">最新发布</li>
        </ul>
      </div>
    </div>
    <div class="content">
      <!-- 上拉加载 -->
      <scroller lock-x height="" @on-scroll-bottom="onScrollBottom" ref="scrollerBottom" :scroll-bottom-offst="100">
        <div>
          <div class="gz-list" v-for="order in orderList" @click="toUrl('emporder')">
            <div class="gz-top">
              <div class="gz-touxiang">
                <img :src="order.user.img" />
              </div>
              <div class="gz-nicheng">{{order.user.user_name}}</div>
              <div class="gz-jiage"><span>￥</span><span>{{order.project_budget}}</span></div>
            </div>
            <div class="gz-timeleixin">
              <div class="gz-time"><span><img src="../../../static/images/index/time.png"/></span><span>{{order.project_deadLine}}过期</span></div>
              <div class="gz-leixin"><span>{{getNameById(order.project_type)}}</span></div>
            </div>
            <div class="gz-content">
              <div class="tupian">
                <div class="tu" v-for="img in order.imgs">
                  <img :src="img" />
                </div>
              </div>
              <div class="wenzhi">{{order.project_describe}}</div>
            </div>
            <div class="gz-bottom">
              <div class="gb-left">
                <div class="gb-tu" v-for="bidder in order.bidders">
                  <img :src="bidder.img" />
                </div>
                <div class="gb-wz"><span>{{order.bidders.length}}</span>人抢单</div>
              </div>
              <div class="gb-right">
                <div class="gb-ljqd"@click.stop="toUrl('orderqiangdan')">立即抢单</div>
              </div>
            </div>
          </div>
          <load-more :show-loading="showLoading" :tip="loadtext" background-color="#fbf9fe" style="margin-top: 30px"></load-more>
        </div>
      </scroller>
    </div>
  </div>
</template>

<script>
  import {LoadMore, Scroller,} from 'vux'
  import common from '../../../static/common';
  export default {
    components: {
      Scroller,
      LoadMore,
    },
    data () {
      return {
        orderList : [],
        znpxMark:false,

        pageNo: 0,
        pageSize: 10,
        onFetching:true,
        showLoading:false,
        loadtext:"上拉加载",
        loadmore:"上拉加载",
        loadrefresh: '正在加载...',
        loadnomore: '没有更多数据了',
      }
    },
    created:function () {
       this.loadMore();
    },
    mounted: function () {
//      this.$nextTick(() => {
//          this.$refs.scrollerBottom.reset({top: 0})
//      })
    },
    methods: {
      cgLink: function (param) {
        this.$router.push({name: param.pagename})
      },
      toUrl: function (pagename,flag) {
        var user = common.getObjStorage("userInfo");
        console.log(user,user._id);
        console.log("flag:"+flag);
        console.log(common.isNull(user._id));

        if( common.isNull(user._id)){
          this.$router.push({name:'login'})
        }else{
          this.$router.push({name: pagename})
        }
        console.log(common.isNull(user._id) || flag);
      },
      getNameById(id){
        return common.getNameByTypeId(id);
      },
      //智能排序    
      znbx(){
        var _self = this;
        _self.znpxMark = _self.znpxMark == true ? false : true;
      },
      //排序
      sort(flag){
        var _self = this;
        _self.znpxMark = false;
      },
      //下拉加载下拉加载
      onScrollBottom () {
        console.log("onScrollBottom:")
        var _self = this;
        if (_self.onFetching) {
          // do nothing
        } else {
          _self.onFetching = true
          setTimeout(() => {
              _self.loadMore()
          }, 100)
        }
      },

      loadMore () {
        var _self = this;
        var params = {
          interfaceId:'getOrderList',
          pageNo: _self.pageNo,
          pageSize: _self.pageSize,
        }

        _self.loadtext = _self.loadrefresh;
        _self.showLoading = true;
        _self.$axios.post('/api/mongoApi',{
          params:params
        }).then((response)=>{
          if( response.data ){
            var data = response.data.data;
            if( data ){
              // console.log("loadMore:"+JSON.stringify(data));
              //订单
              var orderUsers = data.orderUsers || [];
              var orderBidders = data.orderBidders || [];
              var bidders = data.bidders || [];
              var orderList = data.orderList || [];
              orderList.forEach(function (item,index) {
                //雇主
                orderUsers.forEach(function (u,j) {
                  if( item.user_id == u._id ){
                    item.user = u;
                  }else{
                    item.user = {};
                  }
                })
                //参与人
                item.bidders = [];
                orderBidders.forEach(function (b,j) {
                  if( item._id == b.order_id ){
                    bidders.forEach(function (u,j) {
                      if( b.user_id == u._id ){
                        b.user_name = u.user_name;
                        b.img = u.img;
                      }
                    })
                    item.bidders.push(b);
                  }
                })
              });
              _self.orderList = [..._self.orderList, ...orderList];
              _self.$nextTick(() => {
                  _self.$refs.scrollerBottom.reset()
              })

              _self.showLoading = false;
              if( orderList.length < _self.pageSize ){
                _self.loadtext = _self.loadnomore;
              }else{
                _self.loadtext = _self.loadmore;
                _self.onFetching = false
                _self.pageNo++;
              }
            }
          }
        })
      },

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import '../../assets/css/employer/guzhu.css';
  .gu-mokuai .dingdan {
    background: url('../../../static/images/fabubj.png');
    background-size: 100% 100%;
  }
  .gu-mokuai .shouchang {
    background: url('../../../static/images/dingdanbj.png');
    background-size: 100% 100%;
  }
  .header .header-right .hongdian{
    width: 0.1rem;
    height: 0.1rem;
    background: red;
    border-radius: 50%;
    position: absolute;
    right: 0;
    top: 0.1rem;
  }
  .bg{
    background:#fefefe;
  }
  .bg_click {
    background:#f3f3f3;
  }

</style>
