<template>
  <div >
    <!--头部导航-->
    <div class="header">
      <div class="header-left"@click="goback"><img src="../../../static/images/back.png" /></div>
    </div>
    <div class="content">
      <!--设计师基本信息模块-->
      <div class="xinximokuai">
        <div class="ximk-top">
          <div class="touxiang">
            <img src="../../../static/images/bj.jpg" />
          </div>
          <div class="xiright">
            <div class="nicheng">
              <span class="name">设计师小A</span><span class="leixin">建筑设计</span>
            </div>
            <div class="nianxian">
              <span class="nian">7</span>年 &nbsp;<span class="xuexiao">上海交通大学</span><span class="pinxin">☆ ☆ ☆ ☆</span>
            </div>
            <div class="zhicheng">
              <div class="zc">高级工程师</div>
              <div class="sb-qian">签</div>
              <div class="sb-ysm">已实名</div>
              <div class="sb-yrz">已认证</div>
            </div>
          </div>
        </div>
        <div class="nenrong">
          已有年多建筑设计工作经验，从建筑方案到效果图，在到建筑施工图都可独立完成
        </div>
        <div class="danshu">
          <ul>
            <li>
              <p class="shumu">162</p>
              <p>接单数</p>
            </li>
            <li>
              <p class="shumu">162</p>
              <p>总工时</p>
            </li>
            <li>
              <p class="shumu">162</p>
              <p>时薪</p>
            </li>
          </ul>
        </div>
        <div class="scsjs">
          收藏设计师
        </div>
        <div class="xiaoxi">
          <img src="../../../static/images/designer/xiangqing_liaotian.png" />
        </div>
      </div>
      <!--日历选取-->
      <div class="rili">
        <inline-calendar
          ref="calendar"
          class="inline-calendar-demo">
        </inline-calendar>
      </div>
      <!--滑动轮播-->
      <div class="huadonglunpo">
        <tab :line-width=2 active-color='#fc378c' v-model="index">
          <tab-item class="vux-center" key="0">案列展示</tab-item>
          <tab-item class="vux-center" key="1">个人荣誉</tab-item>
          <tab-item class="vux-center" key="2">我的作品</tab-item>
        </tab>
      </div>
      <div class="lunpo">
        <swiper v-model="index" :show-dots="false"class="swps">
          <swiper-item key="0">
            <div class="alzs-list"@click="toUrl('anliexq')">
              <div class="al-top">
                <div class="touxiang">
                  <img src="../../../static/images/bj.jpg" />
                </div>
                <div class="al-right">
                  <div class="ar-top">
                    案列名称
                  </div>
                  <div class="nenrong">
                    案例简介如果你无法简洁的表达你的想法，那只
                  </div>
                </div>
              </div>
              <div class="al-bottom">
                <div class="al-left">
                  <p><span><img src="../../../static/images/designer/anli_xihuan.png"></span><span>33</span></p>
                  <p><span><img src="../../../static/images/designer/anli_liulan.png"></span><span>44</span></p>
                </div>
                <div class="al-right">
                  2017-12-26
                </div>
              </div>
            </div>
            <div class="alzs-list"@click="toUrl('anliexq')">
              <div class="al-top">
                <div class="touxiang">
                  <img src="../../../static/images/bj.jpg" />
                </div>
                <div class="al-right">
                  <div class="ar-top">
                    案列名称
                  </div>
                  <div class="nenrong">
                    案例简介如果你无法简洁的表达你的想法，那只
                  </div>
                </div>
              </div>
              <div class="al-bottom">
                <div class="al-left">
                  <p><span><img src="../../../static/images/designer/anli_xihuan.png"></span><span>33</span></p>
                  <p><span><img src="../../../static/images/designer/anli_liulan.png"></span><span>44</span></p>
                </div>
                <div class="al-right">
                  2017-12-26
                </div>
              </div>
            </div>
          </swiper-item>
          <swiper-item key="1">
            <div class="alzs-list"@click="toUrl('anliexq')">
              <div class="al-top">
                <div class="touxiang">
                  <img src="../../../static/images/bj.jpg" />
                </div>
                <div class="al-right">
                  <div class="ar-top">
                    案列名称
                  </div>
                  <div class="nenrong">
                    案例简介如果你无法简洁的表达你的想法，那只
                  </div>
                </div>
              </div>
              <div class="al-bottom">
                <div class="al-left">
                  <p><span><img src="../../../static/images/designer/anli_xihuan.png"></span><span>33</span></p>
                  <p><span><img src="../../../static/images/designer/anli_liulan.png"></span><span>44</span></p>
                </div>
                <div class="al-right">
                  2017-12-26
                </div>
              </div>
            </div>
            <div class="alzs-list"@click="toUrl('anliexq')">
              <div class="al-top">
                <div class="touxiang">
                  <img src="../../../static/images/bj.jpg" />
                </div>
                <div class="al-right">
                  <div class="ar-top">
                    案列名称
                  </div>
                  <div class="nenrong">
                    案例简介如果你无法简洁的表达你的想法，那只
                  </div>
                </div>
              </div>
              <div class="al-bottom">
                <div class="al-left">
                  <p><span><img src="../../../static/images/designer/anli_xihuan.png"></span><span>33</span></p>
                  <p><span><img src="../../../static/images/designer/anli_liulan.png"></span><span>44</span></p>
                </div>
                <div class="al-right">
                  2017-12-26
                </div>
              </div>
            </div>
          </swiper-item>
          <swiper-item key="2">
            <div class="alzs-list"@click="toUrl('anliexq')">
              <div class="al-top">
                <div class="touxiang">
                  <img src="../../../static/images/bj.jpg" />
                </div>
                <div class="al-right">
                  <div class="ar-top">
                    案列名称
                  </div>
                  <div class="nenrong">
                    案例简介如果你无法简洁的表达你的想法，那只
                  </div>
                </div>
              </div>
              <div class="al-bottom">
                <div class="al-left">
                  <p><span><img src="../../../static/images/designer/anli_xihuan.png"></span><span>33</span></p>
                  <p><span><img src="../../../static/images/designer/anli_liulan.png"></span><span>44</span></p>
                </div>
                <div class="al-right">
                  2017-12-26
                </div>
              </div>
            </div>
            <div class="alzs-list"@click="toUrl('anliexq')">
              <div class="al-top">
                <div class="touxiang">
                  <img src="../../../static/images/bj.jpg" />
                </div>
                <div class="al-right">
                  <div class="ar-top">
                    案列名称
                  </div>
                  <div class="nenrong">
                    案例简介如果你无法简洁的表达你的想法，那只
                  </div>
                </div>
              </div>
              <div class="al-bottom">
                <div class="al-left">
                  <p><span><img src="../../../static/images/designer/anli_xihuan.png"></span><span>33</span></p>
                  <p><span><img src="../../../static/images/designer/anli_liulan.png"></span><span>44</span></p>
                </div>
                <div class="al-right">
                  2017-12-26
                </div>
              </div>
            </div>
          </swiper-item>
        </swiper>

      </div>
    </div>
  </div>
</template>

<script>
  import {Tab, TabItem, Swiper, SwiperItem, InlineCalendar,} from 'vux'
  export default {
    components: {
      InlineCalendar,
      Tab,
      TabItem,
      Swiper,
      SwiperItem,
    },
    data () {
      return {
        index: 0,
      }
    },
    methods: {
      goback(){
        this.$router.goBack();
      },
      toUrl: function (pagename) {
        this.$router.push({name: pagename})
      },
    }
  }
</script>
<style>
  .vux-swiper{
    height: 100% !important;
    overflow:auto !important;
  }
  .vux-swiper-item{
    height:auto !important;
    background: #f2f2f2 !important;
  }
</style>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import '../../assets/css/designer/sjsgeren.css';
  .xinximokuai{
   height: 4.8rem !important;
  }
  .content .rili{
    margin-top: 0.6rem !important;
  }
  .swps{
    height:13rem !important;
    overflow:auto !important;
  }
</style>
