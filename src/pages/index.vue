<!-- 
  @author:liuyr & xupf
-->
<template>
  <div class="index">
    <!-- 轮播 -->
    <div class="index_el">
      <div class="index_el-carousel">
        <!-- <el-carousel @change="carouselChange" ref="carousel" height="475px" arrow="never" :interval="interval"> -->
        <el-carousel ref="carousel" height="475px" arrow="never" :interval="interval">
          <el-carousel-item v-for="(item,index) in wheels" :key="index">
            <img :src="item.carousel_image" alt="">
          </el-carousel-item>
        </el-carousel>
      </div>
      <!-- 左侧列表 -->
      <div class="index_types">
        <ul>
          <!-- <li class="pointer" @click="changeCarousel($event,index)" v-for="(item,index) in typesArr" :key="index"> -->
          <li @mouseenter="showTechnology(item,index)" @mouseleave="hideTechnology" class="pointer" v-for="(item,index) in catalogues" :key="index">
            <span>{{item.catalogue_name}}</span>
            <span><i class="fa fa-chevron-right"></i></span>
            <div class="index_types_detail" v-if="index==technologyIndex">
              <div><span>相关技术</span></div>
              <ul>
                <li class="pointer" @click="toTechnology(item,technology)" v-for="(technology,num) in catalogueTwo" :key="num">
                  {{technology.catalogue_name}}
                </li>
              </ul>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <!-- 排行榜单 -->
    <div class="index_hot_list">
      <div class="index_list_title">
        <span>排行榜单</span>
        <span class="pointer">更多<i class="fa fa-chevron-right"></i></span>
      </div>
      <div class="index_list_content">
        <div class="index_list_video">
          <ul>
            <li v-for="(item,index) in videoList" :key="index">
              <img class="pointer" @click="toVideoDetail(item.video_id)" :src="item.vr_image" alt="">
              <!-- 视频信息 -->
              <div v-if="index==0" class="index_video_info">
                <span @click="toVideoDetail(item.video_id)" class="pointer">
                  {{item.video_name}} - {{item.video_user}}
                </span>
                <span class="pointer">
                  <i class="fa fa-thumbs-up"></i> {{item.video_favor}}
                </span>
              </div>
              <div v-else class="index_video_info">
                <div @click="toVideoDetail(item.video_id)" class="pointer">
                  {{item.video_name}}
                </div>
                <div>
                  <span>
                    <small>{{item.video_user}}</small>
                  </span>
                  <span class="pointer">
                    <i class="fa fa-thumbs-up"></i> {{item.video_favor}}
                  </span>
                </div>
              </div>
              <!-- <div class="index_video_index">{{index+1}}</div>
              <div class="pointer index_video_play"><i class="fa fa-play-circle-o"></i></div> -->
            </li>
          </ul>
        </div>
        <div class="index_list_doc">
          <div class="index_doc_title">
            <span>文章榜单</span>
          </div>
          <div class="index_doc_content">
            <ul>
              <li v-for="(item,index) in docList" :key="index" class="pointer">
                <div v-if="index<3">
                  <img class="index_doc_index" :src="docImgsList[index]" />
                  <span @click="toDocDetail(item.docs_id)" class="docsName" :title="item.docs_name">{{item.docs_user}}-{{item.docs_name}}</span>
                  <span><i class="fa fa-thumbs-up"></i> {{item.docs_favor}}</span>
                </div>
                <div v-else>
                  <span class="index_doc_index">{{index+1}}</span>
                  <span @click="toDocDetail(item.docs_id)" class="docsName" :title="item.docs_name">{{item.docs_user}}-{{item.docs_name}}</span>
                  <span class="pointer"><i class="fa fa-thumbs-up"></i> {{item.docs_favor}}</span>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
    <!-- 免费视频教程与您分享 -->
    <div class="index_free_video_list">
      <div class="index_list_title">
        <span>免费视频教程与您分享</span>
        <span class="pointer">更多<i class="fa fa-chevron-right"></i></span>
      </div>
      <div class="index_free_content"> 
        <ul>
          <li @click="toVideoDetail(item.id)" v-for="(item,index) in freeVideos" :key="index" v-show="index<freeNum">
            <div class="index_free_item pointer">
              <img :src="item.vr_image" alt="">
              <div class="index_free_name" :title="item.vr_name">
                {{item.vr_name}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <!-- 最新动态 -->
    <div class="index_news_list">
      <div class="index_list_title">
        <span>最新动态</span>
      </div>
      <div class="index_news_content">
        <ul>
          <li v-for="(item,index) in lastResourceList" :key="index" v-show="index<lastResourceListNum">
            <div class="index_news_item">
              <div class="index_news_type">
                <span>{{item.catalogue_name}}</span>
                <div class="index_news_line"></div>
              </div>
              <!-- 文章内容 -->
              <div @click="toSourceDetail(item.object_id,item.objtype)" class="index_news_doc_content pointer">
                <div class="index_news_img">
                  <img :src="item.img" alt="">
                </div>
                <div class="index_news_info">
                  <div class="index_news_doc_content_name">
                    {{item.name}}
                  </div>
                  <div class="index_news_doc_content_info">
                    <span>作者: {{item.username}}</span>
                    <span>{{item.obj_created_time | dateFormat}}</span>
                  </div>
                  <div class="index_news_doc_content_content">
                    {{item.obj_desc}}
                  </div>
                </div>
              </div>
              <div class="index_news_video_doc">
                <div class="index_news_video_list">
                  <span>最新视频</span>
                  <ul>
                    <li @click="toVideoDetail(videoItem.id)" class="video_news_name" v-for="(videoItem,index) in item.videoSource" :key="index" v-show="index<videoNum">
                      <span>{{videoItem.vr_name}}</span>
                      <span>{{videoItem.vr_created_time | dateFormat}}</span>
                    </li>
                  </ul>
                </div>
                <div class="index_news_doc_list">
                  <span>最新文章</span>
                  <ul>
                    <li @click="toDocDetail(docItem.id)" class="doc_news_name" v-for="(docItem,index) in item.docSource" :key="index" v-show="index<DocNum">
                      <span>{{docItem.dr_name}}</span>
                      <span>{{docItem.dr_created_time | dateFormat}}</span>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <!-- 学习资源推荐 -->
    <div class="index_learn_list">
      <div class="index_list_title">
        <span>学习资源推荐</span>
        <span class="pointer">更多<i class="fa fa-chevron-right"></i></span>
        <span class="pointer"><i class="fa fa-plus"></i>申请杰普认证</span>
      </div>
      <div class="index_learn_choice">
        <span ref="learnTag" @click="changeChoice(item.id,index)" class="pointer" v-for="(item,index) in catalogues" :key="index">{{item.catalogue_name}}</span>
      </div>
      <div class="index_learn_content"> 
        <div class="index_learn_item" v-for="(item,index) in teacher" :key="index">
          <div class="learn_author_info">
            <div class="learn_author_img">
              <img :src="item.imgUrl" alt="">
            </div>
            <div class="learn_author_name">
              <div><span>{{item.user}}</span><i style="color:#0299FF" class="fa fa-vimeo"></i></div>
              <div>{{item.ranks}}</div>
            </div>
            <div class="learn_author_num">
              共{{item.childs[0].docs.count}}篇文章
            </div>
          </div>
          <div class="index_learn_doc">
            <ul>
              <li @click="toDocDetail(doc.id)" class="pointer" v-for="(doc,index) in item.childs[0].docs.data" :key="index">
                <!-- 需要字段，存储当前视频、文档的图标 -->
                <!-- <FileIcon :fa="doc.icon"/> -->
                <i class="fa fa-file-pdf-o"></i>
                <span>{{doc.dr_name}}</span>
                <span>{{doc.dr_created_time | dateFormat}}</span>
              </li>
            </ul>
          </div>
        </div>
        <!-- <i class="fa fa-file-pdf-o"></i>    pdf
        <i class="fa fa-file-word-o"></i>   word
        <i class="fa fa-file-powerpoint-o"></i>   ppt
        <i class="fa fa-file-text-o"></i>   txt
        <i class="fa fa-file-o"></i>  其他 -->
      </div>
    </div>
    <!-- 合作院校 -->
    <div class="index_school_list">
      <div class="index_list_title">
        <span>合作院校</span>
      </div>
      <div class="index_school_content">
        <div class="index_school_item" v-for="(item,index) in schoolList" :key="index">
          <img :src="item.school_image" alt="" :title="item.school_name">
          <div class="index_school_border"></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import FileIcon from '@/components/fileicon.vue'
import axios from '../http/axios'
import {mapActions,mapState,mapGetters,mapMutations} from 'vuex'
export default {
  components:{
    FileIcon
  },
  data(){
    return {
      learnArr:[],
      lastResourceListNum:4,
      DocNum:5,
      videoNum:5,
      freeNum:5,
      technologyIndex:100,
      fatest:'fa-file-o',
      interval:3000,
      docImgsList:['https://github.com/pluslicy/assets/blob/master/resource/首页/u553.png?raw=true','https://github.com/pluslicy/assets/blob/master/resource/首页/u554.png?raw=true','https://github.com/pluslicy/assets/blob/master/resource/首页/u555.png?raw=true'],
    }
  },
  computed:{
    ...mapState('home',['wheels','schoolList','videoList','docList','catalogues','catalogueTwo','freeVideos','lastResourceList','teacher'])
  },
  //vue实例初始化
  created(){
    // 查找轮播图
    this.findWheels()
    // 查找学校
    this.findSchool()
    // 查找视频榜单
    this.findVideoList()
    // 查找文档榜单
    this.findDocList()
    // 查找一级编目
    this.findCataLogue_first()
    // 查找免费视频
    this.findVideoForFree()
    // 最新动态
    this.findLastResource()
  },
  mounted(){
    // 使用定时器，否则无法获取到按钮
    setTimeout(()=>{
      // 获取学习资源推荐模块的三个按钮，放入数组中
      this.learnArr = this.$refs.learnTag;
      // 使用三目运算符的目的是，在挂载完毕时，获取到的按钮有可能为undefined，为了避免报错
      this.learnArr[0] == undefined ? '1' : this.learnArr[0].click()
    },1000)
  },
  //事件绑定的方法
  methods:{
    ...mapActions('home',['findWheels','findSchool','findVideoList','findDocList','findCataLogue','findCataLogue_two','findFreeVideo','findLastResource','findTeacher']),
    // 免费视频
    findVideoForFree(){
      let obj = {
        vr_permission:1
      }
      this.findFreeVideo(obj)
    },
    // 一级编目
    findCataLogue_first(){
      let obj = {
        cata_level_num:1
      }
      this.findCataLogue(obj)
    },
    // 二级编目
    findCataLogue_second(obj){
      obj.cata_level_num = 2
      this.findCataLogue_two(obj)
    },
    // 通过编目进入列表
    toTechnology(item,tow){
      this.$router.push({
        path:'/video',
        query:{cataOne:JSON.stringify(item),cataTwo:JSON.stringify(tow)}
      })
    },
    //轮播左侧展示技术点
    showTechnology(item,index){
      // 通过一级编目的id查询二级编目
      let obj = {
        cat_catalogue:item.id
      }
      this.findCataLogue_second(obj)

      this.technologyIndex = index;
      $('.index .index_types>ul>li').css({
        "background":"none",
        "color":"white"
      });
      $($('.index .index_types>ul>li')[index]).css({
        "background":"rgba(230,230,230,0.8)",
        "color":"black"
      });
    },
    hideTechnology(){
      this.technologyIndex = 100;
      $('.index .index_types>ul>li').css({
        "background":"none",
        "color":"white"
      });
    },
    // 榜单排行、最新动态、免费视频 视频跳转
    toVideoDetail(id){
      this.$router.push({
        path:'/video/content',
        query:{videoId:id}
      })
    },
    //【排行榜单】文章跳转，【最新动态】文章跳转，【学习资源推荐】文章跳转
    toDocDetail(id){
      this.$router.push({
        path:'/doc/content',
        query:{id:id}
      })
    },
    // 【最新动态】资源跳转
    toSourceDetail(id,type){
      // if(type == 'docs_album'){

      // }
      if(type == 'docs'){
        this.toDocDetail(id)
      }
      // if(type == 'video_album'){

      // }
      if(type == 'video'){
        this.toVideoDetail(id)
      }
    },
    //通过轮播控制左侧列表
    /* carouselChange(newIndex,oldIndex){
      $('.index .index_types li').css("background","none");
      $($('.index .index_types li')[newIndex]).css("background","#66CCFF");
    }, */
    //通过左侧列表控制轮播
    /* changeCarousel(event,index){
      $('.index .index_types li').css("background","none");
      let $li;
      if(event.target.tagName=='SPAN'){
        $li = $(event.target).closest('LI');
      }else{
        $li = $(event.target);
      }
      $li.css("background","#66CCFF");
      this.$refs.carousel.setActiveItem(index);
    }, */
    //用户选择不同的类别
    changeChoice(id,index){
      //id传递给后台
      let obj = {
        catalogue:id
      }
      // this.$refs.refName.$el.click();
      this.findTeacher(obj)
      //index 用来改变背景颜色和字体颜色
      $('.index .index_learn_choice span').css({
        'backgroundColor':'#F2F2F2',
        'color':'black'
      });
      $($('.index .index_learn_choice span')[index]).css({
        'backgroundColor':'#1069FF',
        'color':'white'
      });
    },
  },  
}
</script>
<style scoped>
  .index *{
    margin:0;
    padding:0;
  }
  .index ul{
    list-style: none;
  }
  .index{
    padding-bottom:70px;
  }
  .index img{
    display: block;
  }
  /* 设置统一的样式 */
  .index .pointer{
    cursor: pointer;
  }
  /* 轮播*/
  .index .index_el{
    position: relative;
  }
  .index .index_el-carousel img{
    height:475px;
    width:100%;
  }
  /* 轮播左侧列表 */
  .index .index_types{
    position: absolute;
    z-index:10;
    left:0;
    top:0;
    background-color: rgba(0,0,0,0.5);
    height:475px;
    font-size:16px;
    font-weight: bold;
    color: white;
  }
  .index .index_types>ul>li{
    width:180px;
    line-height: 45px;
    padding:0px 25px;
    position: relative;
  }
  .index .index_types>ul>li:first-child{
    margin-top:10px;
    background-color:'#66CCFF';
  }
  .index .index_types>ul>li span:nth-child(2){
    float:right;
  }
  .index .index_types .index_types_detail{
    width: 260px;
    position: absolute;
    left:230px;
    top:0px;
    background-color: rgba(230,230,230,0.8);
    padding:10px;
    font-size:12px;
    line-height: 12px;
  }
  .index .index_types .index_types_detail>div{
    border-bottom:1px solid #666666;
  }
  .index .index_types .index_types_detail>div span{
    background-color:#666666;
    display: inline-block;
    padding:4px 6px;
    color: white;
  }
  .index .index_types .index_types_detail li{
    float: left;
    padding:10px 13px 0 0;
    color: black;
  }
  /* 排行榜单 */
  /* 与其他共用 */
  .index .index_list_title{
    padding:30px 0 15px;
    font-weight: 900;
  }
  .index .index_list_title>span:first-child{
    font-size:26px;
    line-height: 28px;
    margin-right:8px;
    /*font-weight: bold;*/
  }
  .index .index_list_title>span:last-child{
    color:#666666;
    font-size:14px;
  }
  .index .index_hot_list .index_list_content{
    overflow: hidden;
    padding-bottom:5px;
  }
  .index .index_hot_list .index_list_content .index_list_video{
    float:left;
    width: 1005px;
  }
  .index .index_hot_list .index_list_content .index_list_doc{
    margin-left:1020px;
    width: 210px;
    box-shadow: #aaa 5px 5px 5px; 
    border-left:1px solid #F6F6F6;
  }
  .index .index_hot_list .index_list_content .index_list_video >ul li{
    width: 230px;
    height: 185px;
    float: left;
    box-shadow: #aaa 5px 5px 5px; 
    position: relative;
  }
  .index .index_hot_list .index_list_content .index_list_video >ul li:nth-child(2),.index .index_hot_list .index_list_content .index_list_video >ul li:nth-child(3){
    margin-bottom: 13px;
  }
  .index .index_hot_list .index_list_content .index_list_video >ul li:nth-child(2),.index .index_hot_list .index_list_content .index_list_video >ul li:nth-child(4){
    margin-right:15px;
  }
  .index .index_hot_list .index_list_content .index_list_video img{
    width: 100%;
    height: 140px;
  }
  .index .index_hot_list .index_list_content .index_list_video >ul li:first-child{
    width: 515px;
    height: 383px;
    margin-right: 15px;
  }
  .index .index_hot_list .index_list_content .index_list_video >ul li:first-child img{
    width: 100%;
    height: 100%;
  }
  .index .index_list_video li:first-child>div.index_video_info{
    position: absolute;
    bottom: 0;
    background-color: rgba(0,0,0,0.5);
    color: white;
    width: 100%;
    height: 45px;
    line-height: 45px;
    font-weight: bold;
  }
  .index .index_list_video li .index_video_info span:first-child{
    color:#9C9C9C;
    font-size:15px;
  }
  .index .index_list_video li .index_video_info>div{
    padding:0 15px;
    line-height: 16px;
  }
  .index .index_list_video li .index_video_info>div:first-child{
    padding-top:6px;
  }
  .index .index_list_video li .index_video_info span:last-child{
    float:right;
    color:#1069FF;
    font-size:10px;
  }
  .index .index_list_video li:first-child .index_video_info span:first-child{
    margin-left:30px;
    color:white;
  }
  .index .index_list_video li:first-child .index_video_info span:last-child{
    float:right;
    margin-right: 30px;
    color:white;
  }
  /* 索引和开关按钮 */
  .index .index_list_video li>div.index_video_index{
    position: absolute;
    left:20px;
    top:10px;
    color:rgba(255,255,255,0.7);
    font-size:14px;
    font-weight: bold;
  }
  .index .index_list_video li:first-child>div.index_video_index{
    position: absolute;
    left:25px;
    top:15px;
    font-size:18px;
  }
  .index .index_list_video li>div.index_video_play{
    position: absolute;
    font-size:40px;
    line-height: 140px;
    left:95px;
    top:0;
    color:rgba(255,255,255,0.7);
  }
  .index .index_list_video li:first-child>div.index_video_play{
    position: absolute;
    font-size:60px;
    line-height: 338px;
    left:230px;
    top:0;
  }
  /* 文章榜单 */
  /* .index .index_list_doc{
  } */
  .index .index_list_doc .index_doc_title{
    background-color: #3585FE;
    font-size:16px;
    line-height: 38px;
    color: white;
    font-weight: bold;
    padding-left:10px;
  } 
  .index .index_list_doc .index_doc_content li:hover{
    background-color:rgba(240,240,240,0.5);
  }
  .docsName{
    width: 120px;
    display: inline-block;
    height: 30px;
  }
  .index .index_list_doc .index_doc_content li{
    overflow: hidden;
    margin:5px 0;
    padding:0 10px;
    font-size:12px;
    font-weight: 900;
  }
  .index .index_list_doc .index_doc_content .index_doc_index{
    width:20px;
    margin-right:10px;
    text-align: center;
    font-size:20px;
    color:#666666;
  }
  .index .index_list_doc .index_doc_content span{
    line-height: 38px;
    float: left;
  }
  .index .index_list_doc .index_doc_content span:last-child{
    float: right;
    font-weight: normal;
    color:#666666;
  }
  .index .index_list_doc .index_doc_content span:last-child::after{
    clear: both;
  }
  .index .index_list_doc .index_doc_content img{
    width: 20px;
    height: 30px;
    margin-top:4px;
    float: left;
  }
  /* 免费视频教程与您分享 */
  .index .index_free_video_list .index_list_title{
    padding:25px 0 15px;
    font-weight: bold;
  }
  .index .index_free_video_list .index_free_content ul{
    overflow: hidden;
  }
  .index .index_free_video_list .index_free_content li{
    float: left;
    margin-left:10px;
    padding-bottom:10px;
  }
  .index .index_free_video_list .index_free_content li:first-child{
    margin-left:0;
  }
  .index .index_free_video_list .index_free_item:hover{
    box-shadow: #aaa 0 0 10px;
  }
  .index .index_free_video_list .index_free_item img{
    width: 240px;
    height: 145px;
    display: block;
  }
  .index .index_free_video_list .index_free_item .index_free_name{
    border:1px solid #ccc;
    box-sizing: border-box;
    border-top: none;
    text-align: center;
    line-height: 45px;
    font-size:16px;
    width: 240px;
    padding:0 20px;
    height: 46px;
    font-weight: bold;
    overflow: hidden;
  }
  /* 最新动态 */
  .index .index_news_list .index_list_title span{
    color:black;
    padding:20px 0 15px;
    font-size:28px;
    line-height: 28px;
  }
  .index .index_news_list .index_news_content>ul{
    overflow: hidden;
  }
  .index .index_news_list .index_news_content>ul>li{
    width:610px;
    float: left;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(odd){
    margin-right:20px;
  }
  .index .index_news_list .index_news_content>ul>li .index_news_type{
    position: relative;
    border-bottom: 1px solid #E6E6E6;
    color:#3585FE;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(2) .index_news_type{
    color:#EC385A;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(3) .index_news_type{
    color:#47A496;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(4) .index_news_type{
    color:#FF9933;
  }
  .index .index_news_list .index_news_content>ul>li .index_news_type span{
    display: block;
    font-size:22px;
    font-weight: bold;
    width: 110px;
    text-align: center;
  }
  .index .index_news_list .index_news_content>ul>li .index_news_line{
    width: 110px;
    height: 5px;
    position: absolute;
    bottom:-3px;
    left:0;
    background-color:#3585FE;
    z-index:10;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(2) .index_news_line{
    background-color:#EC385A;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(3) .index_news_line{
    background-color:#47A496;
  }
  .index .index_news_list .index_news_content>ul>li:nth-child(4) .index_news_line{
    background-color:#FF9933;
  }
  .index .index_news_list .index_news_doc_content{
    overflow: hidden;
    margin-bottom:10px;
  }
  .index .index_news_list .index_news_doc_content>div{
    float:left;
  }
  .index .index_news_list .index_news_doc_content img{
    width: 220px;
    height: 110px;
    margin-top:20px;
  }
  .index .index_news_list .index_news_doc_content .index_news_info{
    height: 120px;
    width: 370px;
    padding-left:20px;
    margin-top:20px;
    padding-top:10px;
  }
  .index .index_news_list .index_news_info .index_news_doc_content_name{
    font-size: 20px;
    line-height: 20px;
    margin-bottom:5px;
  }
  .index .index_news_list .index_news_info .index_news_doc_content_info{
    font-size: 12px;
    color:#424242;
    /* margin-bottom:2px; */
  }
  .index .index_news_list .index_news_info .index_news_doc_content_info span:last-child{
    float: right;
    margin-right:20px;
  }
  .index .index_news_list .index_news_info .index_news_doc_content_content{
    font-size: 12px;
    line-height: 20px;
    height:58px;
    position:relative;
    /* text-align: justify; */
    overflow: hidden;
  }
  .index .index_news_list .index_news_info .index_news_doc_content_content::after {
    content:"...";
    font-size: 12px;
    line-height: 16px;
    position:absolute;
    bottom:0;
    right:0;
    background:white;
  }
  .index .index_news_list .index_news_video_doc{
    overflow: hidden;
    margin-bottom:20px;
  }
  .index .index_news_list .index_news_video_doc ul{
    list-style-type: initial;
    padding-left: 15px;

  }
  .index .index_news_list .index_news_video_doc>div{
    float: left;
    width: 280px;
    position: relative;
  }
  .video_news_name,
  .doc_news_name{
    height: 20px;
    cursor: pointer;
  }
  .video_news_name span:first-child,
  .doc_news_name span:first-child{
    height: 16px;
    width: 120px;
    overflow: hidden;
    display: inline-block;
  }
  .index .index_news_list .index_news_video_doc .index_news_video_list{
    padding-right:20px;
    border-right:2px dashed #7B7B7B;
  }
  .index .index_news_list .index_news_video_doc .index_news_doc_list{
    padding-left:20px;
  }
  .index .index_news_list .index_news_video_doc>div>span{
    font-size: 14px;
    font-weight: bold;
    margin-bottom:10px;
  }
  .index .index_news_list .index_news_video_doc>div li{
    font-size: 12px;
    line-height: 20px;
  }
  .index .index_news_list .index_news_video_doc>div ul span:last-child{
    position: absolute;
    right:20px;
  }
  .index .index_news_list .index_news_video_doc .index_news_doc_list ul span:last-child{
    position: absolute;
    right:0px;
  }
  /* 学习资源推荐 */
  .index .index_learn_list .index_list_title{
    /* margin-top:-20px; */
    padding: 10px 0 15px;
  }
  .index .index_learn_list .index_list_title span:nth-child(2){
    color:#666666;
    font-size:16px;
  }
  .index .index_learn_list .index_list_title span:last-child{
    color:#018001;
    /* background-color: #CFE9C7; */
    margin-left:30px;
    padding:0 5px;
  }
  .index .index_learn_list .index_learn_choice span{
    display: inline-block;
    background-color: #F2F2F2;
    padding:2px 16px;
    margin-right:10px;
    font-weight: bold;
  }
  .index .index_learn_list .index_learn_choice span:first-child{
    background-color: #1069FF;
    color:white;
  }
  .index .index_learn_content{
    overflow: hidden;
  }
  .index .index_learn_content .index_learn_item{
    width:400px;
    float:left;
    padding:20px 0;
  }
  .index .index_learn_content .index_learn_item:nth-child(3n+1),.index .index_learn_content .index_learn_item:nth-child(3n+2){
    padding-right:20px;
  }
  .index .index_learn_content .index_learn_item:first-child,.index .index_learn_content .index_learn_item:nth-child(2),.index .index_learn_content .index_learn_item:nth-child(3){
    border-bottom:1px solid #eee;
  }  
  .index .index_learn_item .learn_author_info{
    overflow: hidden;
    margin-bottom:15px;
  }
  .index .index_learn_item .learn_author_info>*{
    float:left;
  }
  .index .index_learn_item .learn_author_info img{
    width: 61px;
    height: 61px;
  }
  .index .index_learn_item .learn_author_info .learn_author_name{
    margin-left:10px;
  }
  .index .index_learn_item .learn_author_info .learn_author_name div:first-child{
    font-size:14px;
    line-height: 14px;
    margin-top:13px;
    font-weight: bold;
  }
  .index .index_learn_item .learn_author_info .learn_author_name span:first-child{
    margin-right:5px;
  }
  .index .index_learn_item .learn_author_info .learn_author_name div:nth-child(2){
    font-size:12px;
    line-height: 13px;
    margin-top:10px;
    color: #747474;
  }
  .index .index_learn_item .learn_author_info .learn_author_num{
    line-height: 61px;
    float:right;
    font-size:12px;
    color: #747474;
    margin-right:20px;
  }
  .index .index_learn_item .index_learn_doc li{
    line-height:30px;
  }
  .index .index_learn_item .index_learn_doc li>span:nth-child(2){
    margin-left:6px;
    font-size:12px;
  }
  .index .index_learn_item .index_learn_doc li>span:nth-child(3){
    float:right;
    font-size:12px;
  }
  /* 合作院校 */
  .index .index_school_list .index_list_title{
    padding-top:10px;
    padding-bottom: 20px;
  }
  .index .index_school_list .index_list_title span{
    color:black;
    padding:20px 0 15px;
    font-size:28px;
    line-height: 28px;
  }
  .index .index_school_list .index_school_content{
    overflow: hidden;
  }
  .index .index_school_list .index_school_content .index_school_item{
    position: relative;
    float:left;
    margin-right:51px;
    margin-bottom:20px;
  }
  .index .index_school_list .index_school_content .index_school_item:nth-child(5n){
    margin-right:0;
  }
  .index .index_school_list .index_school_content .index_school_item img,.index .index_school_list .index_school_content .index_school_item .index_school_border{
    width: 202px;
    height: 80px;
    border:1px solid #DDDDDD;
  }
  .index .index_school_list .index_school_content .index_school_item img{
    position:absolute;
    top:5px;
    left:5px;
    box-shadow: #aaa 5px 5px 5px; 
  }
</style>