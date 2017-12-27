<template>
  <div>
    <!--头部导航-->
    <xheader2 :title="title" :rightClass="rightClass" :collectFlag="collectFlag"></xheader2>
    <!--主体内容-->
    <div class="od-condent">
      <!--订单确认后详情页-->
      <div class="ddxq">
        <div class="ddxq-top">
          <div class="ddxq-img" @click="toUrl('emporderimgs')">
            <img src='../../../static/images/bj.jpg'>
            <div class="num">{{imgSize}}</div>
          </div>
          <div class="ddxq-jianjie">
            <div class="jianjie-top">{{order.project_title}}</div>
            <div class="jianjie-bottom">
              <div class="db-mianji"><span>{{getNameById(order.project_type)}}</span>/<span class="mianji">500</span><span class="mianji">m²</span></div>
              <div class="db-yushuan"><span>预算</span><span class="jiage">￥</span><span class="jiage">{{order.project_budget}}</span></div>
            </div>
          </div>
        </div>
        <div class="ddxq-list">
          <div class="ddxq-box">
            <div class="box-left">
              <span><img src='../../../static/images/employer/leixin.png'></span><span>预计完成：</span>
            </div>
            <div class="box-right">
              <span>{{order.project_endTime}}</span>
            </div>
          </div>
          <div class="ddxq-box">
            <div class="box-left">
              <span><img src='../../../static/images/employer/04.png'></span><span>交易状态：</span>
            </div>
            <div class="box-right">
              <span>建筑设计</span>
            </div>
          </div>
          <div class="ddxq-box">
            <div class="box-left">
              <span><img src='../../../static/images/employer/02.png'></span><span>设计深度：</span>
            </div>
            <div class="box-right">
              <span v-for="item in order.project_depth">{{getDepthName(item)}}</span>
            </div>
          </div>
          <div class="ddxq-box">
            <div class="box-left">
                <span><img src='../../../static/images/employer/03.png'></span><span>工时：</span>
            </div>
            <div class="box-right">
              <span>{{order.project_workHours}}小时</span>
            </div>
          </div>
          <div class="ddxq-box" style="border: none;">
            <div class="box-left">
              <span><img src="../../../static/images/employer/06.png"/></span><span>地区：</span>
            </div>
            <div class="box-right">
              <span>{{order.project_region}}</span>
            </div>
          </div>
        </div>
        <div class="ddxq-bottom">
          <div class="biaoqi">订单详情</div>
          <div class="neirong">
            <span>{{order.project_describe}}</span>
          </div>
          <div class="chakangengduo">点击查看更多</div>
        </div>
      </div>
      <!--抢单列表设计师-->
      <div class="od-renshu">
        <div class="tu"></div>
        <p v-if="orderBidders.length>0">已有<span>{{orderBidders.length}}</span>位设计师抢单</p>
        <p v-if="orderBidders.length=0">还没有设计师抢单</p>
        <div class="gengduo" @click="toUrl('emporderparts')"></div>
      </div>

      <div class="od-list">

        <div class="qdsjs-list"  v-for="bidder in orderBidders">
          <div class="qdsjs-time">
            <p>2017-10-10</p>
          </div>
          <div class="qdsjs-box">
            <div class="qb-top">
              <div class="qt-touxiang">
                <img src='../../../static/images/bj.jpg'>
              </div>
              <div class="qt-nichen">
                <span>设计师小A</span>
              </div>
              <div class="chat"><img src='../../../static/images/employer/miaomiao.png'></div>
              <div class="qt-jiage">
                <div class="qt-time"><span>20小时</span></div>
                <div class="qt-jingqian"><span>￥</span><span>5000</span></div>
              </div>
            </div>
            <div class="qb-content">
              已有多年建筑设计工作经验，从事建筑方案到效果图，再到建筑施工图都可以完成 再到建筑施工图都可以完成，再到建筑施工图都可以完成，再到建筑施工图都可以完成，

            </div>
            <div class="qb-botton">
              选择设计师
            </div>
          </div>
        </div>

      </div>
    </div>
    <!--底部-->
    <div class="od-botton">
      <div class="mmliaotian">
        <span><img src='../../../static/images/employer/miaomiao.png'></span>
        <span>喵喵聊天</span>
      </div>
      <div class="lijiqiangdan"@click="toUrl('orderqiangdan')">立即抢单</div>
    </div>
    <!--弹窗-->
    <div class="tishikuang">
      <div class="tisi">
        收藏成功
      </div>
    </div>
  </div>
</template>

<script>
  import xheader2 from '../../components/header/xheader2.vue'
  import common from '../../../static/common';
  export default {
    components: {
      xheader2
    },
    data () {
      return {
        title: '订单详情',
        rightClass: 'collect',
        cover: {
          backgroundImage: 'url(' + require('../../../static/images/bj.jpg') + ')'
        },

        order_id:null,
        user_id:null,
        collectFlag:0,
        order:{},
        orderBidders:[],
        imgSize:0
      }
    },
    created: function () {
      console.log("id:"+this.$route.query.id)
      this.order_id = this.$route.query.id;
      this.userInfo = common.getObjStorage("userInfo");
      this.user_id = this.userInfo._id;
      this.zhishu();
      this.initData();
    },
    mounted: function () {
    },
    methods: {
      toUrl: function (pagename) {
        this.$router.push({name: pagename})
      },
      getNameById(id){
        return common.getNameByTypeId(id);
      },
      getDepthName(num){
        return num==0?"方案":num==1?"扩出":num==2?"施工":"";
      },
      // 订单详情字数限制
      zhishu () {
       var neirong = $(".neirong").text();
          $(".neirong").each(function() {
            var maxwidth = 80;
            if($(this).text().length > maxwidth) {
              $(this).text($(this).text().substring(0, maxwidth));
              $(this).html($(this).html() + '...');
            }
          });
        //点击查看更多
        var flag = true
        $('.ddxq-bottom .chakangengduo').click(function() {
          if(flag == true) {
            $(this).html('点击查看更多')
            $(".neirong").each(function() {
             let maxwidth = 80;
              if($(this).text().length > maxwidth) {
                $(this).text($(this).text().substring(0, maxwidth));
                $(this).html($(this).html() + '...');
              }
            });
            flag = false;
          } else {
            $(this).html('收起')
            $(".neirong").html(neirong)
            flag = true;
          }
        });
      },

      initData(){
        var _self = this;
        if( common.isNull(_self.order_id) == true ){
          return;
        }
        var params = {
          interfaceId:'getProjectDetail',
          order_id:_self.order_id,
          user_id:_self.user_id
        }
        console.log("params:"+JSON.stringify(params))
        _self.$axios.post('/api/mongoApi',{
          params:params
        }).then((response)=>{
          console.log(response);
          var data = response.data.data;
          if( data ){
            _self.collectFlag = data.collectFlag;
            //订单
            var order = data.order || {};
            order.bidders = [];
            var orderBidders = data.orderBidders || [];
            var bidders = data.bidders || [];
            orderBidders.forEach(function (b,j) {
              bidders.forEach(function (u,j) {
                if( b.user_id == u._id ){
                  b.user_name = u.user_name;
                  b.img = u.img;
                }
              })
            })
            _self.orderBidders = orderBidders;
            _self.order = order;
            if( _self.order.imgs ){
              _self.imgSize = _self.order.imgs.length;
            }
          }
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import '../../assets/css/employer/order.css';
  .od-condent .od-renshu .tu {
    background: url('../../../static/images/timer.png') left center no-repeat;
    background-size: .5rem .5rem;
  }
  .od-condent .od-renshu .gengduo {
    background: url('../../../static/images/more.png') center center no-repeat;
    background-size: .5rem .5rem;
  }
  .tishikuang .tisi {
    position: fixed;
    width: 2rem;
    height: 1.2rem;
    border-radius: 0.1rem;
    background: #8d8d8d;
    text-align: center;
    line-height: 1.2rem;
    color: white;
    left:2.75rem;
    top: 30%;
    font-size: 0.32rem;
    display: none;
  }
</style>
