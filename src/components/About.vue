<template>
  <div>
    <div class="topbar">
      <div class="container">
        <div class="row">
          <div @click="toAbout"> 
            <!-- <el-input
              suffix-icon="el-icon-shopping-bag-1"
              v-model="input"
              placeholder="请输入套餐"
            ></el-input> -->
            <img src="../assets/logo.png" alt="">
          </div>
          <div class="marginA animate__animated animate__zoomInDown"></div>
          <div class="rightF">
            <el-menu
              class="el-menu-demo"
              mode="horizontal"
              @select="handleSelect"
            >
              <el-menu-item index="1" @click="toSelect">自选套餐</el-menu-item>
              <el-menu-item index="2" @click="toRevision">修图</el-menu-item>
              <el-submenu index="3">
                <template slot="title">{{data.username}}</template>
                <el-menu-item index="2-1" @click="toMessage">我的信息</el-menu-item>
                <el-menu-item @click="toOrderDetail" index="2-2"
                  ><el-badge  :value="psOrderCreate" :max="99" class="item">
                    我的订单
                  </el-badge></el-menu-item
                >
                
                <el-menu-item index="2-3" @click="loginOut">登出</el-menu-item>
              </el-submenu>
              <el-menu-item index="4">余额:￥{{data.money}}</el-menu-item>
            </el-menu>
            <!-- <el-menu
              class="el-menu-demo"
              mode="horizontal"
              @select="handleSelect"
            >
              <el-menu-item index="1">自选套餐</el-menu-item>
              <el-submenu index="2">
                <template slot="title">我的工作台</template>
                <el-menu-item index="2-1">选项1</el-menu-item>
                <el-menu-item index="2-2">选项2</el-menu-item>
                <el-menu-item index="2-3">选项3</el-menu-item>
                <el-submenu index="2-4">
                  <template slot="title">选项4</template>
                  <el-menu-item index="2-4-1">选项1</el-menu-item>
                  <el-menu-item index="2-4-2">选项2</el-menu-item>
                  <el-menu-item index="2-4-3">选项3</el-menu-item>
                </el-submenu>
              </el-submenu>
              <el-menu-item index="3">消息中心</el-menu-item>
              <el-menu-item index="4"> 订单管理</el-menu-item>
            </el-menu> -->
          </div>
        </div>
      </div>
    </div>
    <div class="showImg">
      <el-carousel height="500" :interval="5000" arrow="always">
        <el-carousel-item
          :height="height"
          v-for="(img, index) in list"
          :key="index"
        >
          <img @click="tomain(index)" v-bind:src="img.url" />
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="saleMessage">
      <p class="text margin0 padding20">古装源摄影店(步行街店)</p>
      <el-rate
        class="padding20"
        v-model="value"
        disabled
        show-score
        text-color="#ff9900"
        score-template="{value}"
      >
      </el-rate>
      <span class="font16">人均￥1944</span>
      <p class="font16 padding20">个性写真 <span>步行街></span></p>
      <p class="font16 padding20">营业中 <span>周一至周日 08:30-21:00</span></p>
      <p class="alignR" @click="toOther">></p>
      <div class="location">
        <div class="leftP">
          <p class="padding20">
            镜湖区步行街新百大厦对面，老必胜客旁边楼梯上三楼
          </p>
          <p class="padding20">距新百大厦步行410m</p>
        </div>
        <div class="rightP">
          <div class="borderR">
            <i class="el-icon-house"></i><span>地址</span>
          </div>
          <div class="phone">
            <i class="el-icon-phone-outline"></i><span>电话</span>
          </div>
        </div>
      </div>
    </div>
    <div class="gettip">
      <div class="getOne">
        <div class="part1">优惠</div>
        <div class="part2">
          <p>网络订单加送两张精修底片</p>
          <p>1090人感兴趣</p>
        </div>
        <div class="part3">立即领取></div>
      </div>
      <div class="getTwo">
        <div class="part1">领券</div>
        <div class="part2">
          <p>两张精修入册入底</p>
          <p>立即领券></p>
        </div>
        <div class="part3">立即领券></div>
      </div>
    </div>
    <div class="main">
      <div style="padding-top:15px;background-color:white">
        <div
        @click="toWelcome(index)"
        class="contain"
        v-for="(item, index) in quertList"
        :key="index"
      >
        <div class="picture">
          <img
            style="max-width:100%;max-height:100%;width:100%"
            class="imgS"
            :src="item.referencePicture"
            alt=""
          />
          <!-- <img
            style="max-width:100;max-height:100%;width:100%"
            class="imgS"
            src="../assets/tt2.jpg"
            alt=""
          /> -->
        </div>
        <div class="infoT">
          {{ item.packName }}
        </div>

        <div class="price">
          <span class="rmb">￥</span
          ><span class="moneyM">{{ item.packPrice }}</span>
        </div>
      </div>
      <div
        class="loadmore"
        v-infinite-scroll="loadMore"
        infinite-scroll-disabled="busy"
        infinite-scroll-distance="10"
      ></div>
    </div>
      </div>
    <!-- <div class="divide">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-size="10"
        :total="100"
      >
      </el-pagination>
    </div> -->
    <div class="pictureLow">
      <p style="margin-left:20px;font-size:20px;margin-top:15px">商家作品</p>
      <div
        class="containd"
        v-for="(item) in even(photoList)"
        :key="item.photoId"
      >
        <img style="max-width:100%;max-height:100%;width:100%" :src="item.photoUrl" alt="" />
      </div>
      <div style="margin-left:20px;font-size:20px" @click="flag = !flag">加载更多...</div>
    </div>
    <div style="width:100%;background: #282c31;">
      <div class="footer">
      <div class="wrapper">
        <div class="Lmain">
          <span class="link">
            <a href="">简介</a>
            <a href="">联系我们</a>
            <a href="">友情链接</a>
            <a href="">用户服务协议</a>
            <a href="">隐私权声明</a>
            <a href="">法律投诉声明</a>
            <a href="">网上有害信息举报专区</a>
          </span>
          <!-- <span class="logo"></span> -->
          <span class="txt">
            <p>版权所有 &nbsp;&nbsp;&nbsp;安徽工程大学</p>
            <p>违法和不良信息举报电话: 13001902730</p>
            <p>举报邮箱: fengniao@fengniao.com</p>
          </span>
        </div>
        <div class="aside">
          <div style="margin-bottom:10px;text-align:center">关注公众号</div>
          <div style="width:144px;height:140px;margin:0 auto"><img style="width:100%;height:100%" src="../assets/erweima.png" alt=""></div>
        </div>
      </div>
    </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      websocket:'',
      psOrderCreate:null,
      photoList: [],
      value: 4.7,
      busy: false,
      quertList: [],
      pageNum: 0,
      pageSize: 5,
      data: {},
      height: "500",
      input: "",
      flag: false,
      list: [
        { url: require("../assets/demo1.jpg") },
        { url: require("../assets/demo2.jpg") },
        { url: require("../assets/demo3.jpg") },
        { url: require("../assets/demo4.jpg") },
        { url: require("../assets/demo5.jpg") },
      ],
    };
  },
  methods: {
    even: function (photoList) {
      const flag = this.flag;
      if (flag) {
        return photoList;
      } else {
        return photoList.slice(16);
      }
    },
    // async handleCurrentChange() {
    //   var pageNum = this.pageNum++;
    //   var pageSize = this.pageSize++;
    //   const res = await axios({
    //     method: "get",
    //     url: "/shootingPackage/queryAllPacket",
    //     params: { pageNum, pageSize },
    //   });
    //   this.quertList = res.data.data.list;
    // },
    // async handleSizeChange() {
    //   var pageNum = this.pageNum++;
    //   var pageSize = this.pageSize++;
    //   const res = await axios({
    //     method: "get",
    //     url: "/shootingPackage/queryAllPacket",
    //     params: { pageNum, pageSize },
    //   });
    //   this.quertList = res.data.data.list;
    // },
    toSelect(){
      this.$router.push("/select")
    },
    toMessage(){
      this.$router.push("/message")
    },
    toAbout(){
      this.$router.push("/about")
    },
    async getPhoto() {
      var pageNum = 1;
      var pageSize = 50;
      const res = await axios({
        method: "get",
        url: "/rights/queryAllPhotoUrls",
        params: { pageNum, pageSize },
      });
      console.log(res.data.data.list);
       this.photoList = res.data.data.list;
      // photoList.map(async (item) => {
      //   const photoId = item.photoId;
      //   // const des = await axios.get("/rights/queryPhoto",photoId);
      //   // console.log(des);
      //   // return des;
      //   const res = await axios({
      //     method: "post",
      //     url: "/rights/queryPhoto",
      //     params: { photoId },
      //   });
      //   this.photoList.push(res.data.data);
      // });
      //  const res = axios({
      //   method: "get",
      //   url: "/rights/queryPhoto",
      //   params:photoList[1].photoId
      // });
    },
    toOther() {
      this.$router.push("/main");
    },
    async getGoodsList(flag) {
      var pageNum = this.pageNum;
      var pageSize = this.pageSize;
      const res = await axios({
        method: "get",
        url: "/shootingPackage/queryAllPacket",
        params: { pageNum, pageSize },
      });
      if (flag) {
        // 多次加载数据
        this.quertList = this.quertList.concat(res.data.data.list);
        if (res.data.data.list.length == 0) {
          this.busy = true;
        } else {
          this.busy = false;
        }
      } else {
        // 第一次加载数据
        this.quertList = res.data.data.list;
        // 当第一次加载数据完之后，把这个滚动到底部的函数触发打开
        this.busy = false;
      }
      
    },
    loadMore: function () {
      this.busy = true;
      //多次加载数据
      setTimeout(() => {
        this.pageNum++;
        this.getGoodsList(true);
      }, 1000);
    },
    toOrderDetail(){
      this.$router.push("/orderDetail")
    },
    loginOut() {
      axios.get("/user/userLoginOut");
      this.$message.success("退出成功");
      this.$router.push("/login");
    },
    async getMeal() {
      var pageNum = this.pageNum;
      var pageSize = this.pageSize;
      const res = await axios({
        method: "get",
        url: "/shootingPackage/queryAllPacket",
        params: { pageNum, pageSize },
      });
      console.log(res.data.data.list);
      this.quertList = res.data.data.list;
    },
    async getUserinfo() {
      const res = await axios.get("/main/getUserInfo");
      this.data = res.data.data;
    },
    toRevision(){
        this.$router.push("/revision")
      },
    tomain: function (index) {
      // console.log(index)
      localStorage.setItem("index", JSON.stringify(index));
      this.$router.push("/welcome");
    },
    toWelcome: function (index) {
      const referencePicture = this.quertList[index].referencePicture
      localStorage.setItem(
        "referencePicture",
        referencePicture
      );
      localStorage.setItem(
        "trueV",
        JSON.stringify(this.quertList[index].packId)
      );
      this.$router.push("/welcome");
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    
  },
  created() {
    this.getUserinfo();
    this.getPhoto();
    this.getMeal();
  },
  // computed:{
  //   devList:function(){
  //     console.log(this.photoList)
  //     return this.photoList.slice(6);
  //   }
  // }
};
</script>
<style scoped>
a {
  text-decoration: none;
  color: black;
}
.el-carousel__item > img[data-v-c226fde6] {
    height: 500px;
    width: 100%;
}
.el-menu {
  background-color: #1a1a1c;
}
.el-menu--horizontal > .el-menu-item.is-active {
  border-bottom: 2px solid #fefeff;
}
.topbar {
  background: #1a1a1c;
  padding: 20px;
}
.moneyM {
  margin-left: 4px;
  vertical-align: baseline;
  text-align: left;
  line-height: 25px;
  font-family: PingFangSC-Medium;
  font-size: 18px;
  color: #fd3f31;
}
.loadmore {
  height: 1px;
}
.picture {
  display:flex;
  align-items:center;
  justify-content: center;
  width: 236px;
  height: 218px;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  font-size: 12px;
  margin-top:15px;
  border: 1px solid #f2f2f2;
  background: #fff;
}
.rmb {
  vertical-align: baseline;
  font-family: PingFangSC-Medium;
  font-size: 18px;
  color: #fd3f31;
}
.price {
  height: 25px;
  padding: 6px 0 0;
  line-height: 1.6;
}
.infoT {
  height: 40px;
  white-space: normal;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  font-family: PingFangSC-Regular;
  font-size: 14px;
  color: #9b9b9b;
  line-height: 20px;
  padding-top: 10px;
}
.container {
  width: 1200px;
  margin: 0 auto;
}
.divide {
  width: 1350px;
  margin: 0 auto;
}
.row {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin-right: -15px;
  margin-left: -15px;
  text-align: center;
  color: white;
  justify-content: space-between;
  font-family: "Poppins", sans-serif;
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 18px;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
.el-carousel__item > img {
  height: 500px;
  width: 100%;
}
.showImg {
  width: 1300px;
  margin: 0 auto;
  height: 440px;
}
.photoShow {
  width: 236px;
  height: 218px;
}
.rightF {
  width: 430px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.el-menu.el-menu--horizontal {
  border: none;
}
.el-menu--horizontal > .el-menu-item.is-active {
  color: #ccc;
}
.imgS {
  width: 80%;
}
.main {
  width: 1300px;
  margin: 0 auto;
  /* display: flex;*/
  margin-top: 20px;
  margin-bottom: 20px;
  font-size: 0;
  font-family: "helvetica neue", tahoma, "hiragino sans gb", stheiti,
    "wenquanyi micro hei", sans-serif;
  border-top: 1px solid #f2f2f2;
  border-left: 1px solid #f2f2f2;
}
.contain {
  width: 260px;
  height: 320px;
  box-sizing: border-box;
  display: inline-block;
  font-size: 12px;
  border: 1px solid #f2f2f2;
  border-left: none;
  background: #fff;
  vertical-align: top;
  padding-left: 10px;
}
.containd {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 260px;
  height: 208px;
  box-sizing: border-box;
  font-size: 12px;
  border: 1px solid #f2f2f2;
  border-top: none;
  border-left: none;
  background: #fff;
  display:inline-block;
  vertical-align: top;
}
.footer {
  width: 1200px;
  margin: 0 auto;
  color: #fff;
  height: 200px;
    background: #282c31;
}
.footer .Lmain .link a {
    font-size: 14px;
    line-height: 26px;
    color: #959ba2;
    display: inline-block;
    margin-right: 45px;
}
.warpper{
  width:100%;
}
.Lmain{
  width: 78%;
    height: 155px;
    float: left;
    margin: 26px 0 0 0;
    border-right:1px solid #14161a;
}
.link{
  width: 100%;
  text-align: center;
    height: 26px;
    float: left;
}
/* .logo{
  clear: both;
    width: 148px;
    height: 108px;
    float: left;
    margin-top: 40px;
    background: url(../assets/logo.png) no-repeat;
    margin-left:50px;
} */
.txt{
  width: 100%;
    height: 74px;
    float: left;
    margin: 24px 0 0 0px;
    font-size: 16px;
    line-height: 20px;
    color: #666c74;
    margin-left: 50px;
}
.aside{
  width: 21%;
    float: left;
    height: 168px;
    background-position: -1364px -490px;
    margin-top: 24px;
    display: flex;
    flex-direction: column;
    justify-content: center;
}
.showImg >>> .el-carousel__container {
  height: 440px;
}
.saleMessage {
  width: 1300px;
  margin: 0 auto;
  background-color: #fff;
  margin-bottom: 20px;
}
.gettip {
  width: 1300px;
  margin: 0 auto;
  background-color: #fff;
}
.text {
  font-family: "Poppins", sans-serif;
  color: black;
  font-size: 25px;
}
.el-rate {
  display: inline;
  margin-right: 23px;
}
.margin0 {
  margin-top: 0px;
}
.font16 {
  font-size: 16px;
}
.alignR {
  text-align: right;
  padding-right: 20px;
}
.location {
  width: 100%;
  display: flex;
  align-items: center;
}
.leftP {
  flex: 10;
}
.rightP {
  flex: 2;
  display: flex;
  height: 70%;
}
.borderR {
  border-right: 1px solid grey;
  flex: 1;
  padding-top: 6px;
  text-align: center;
  height: 100%;
  flex-flow: column;
}
.phone {
  flex: 1;
  padding-top: 6px;
  text-align: center;
  height: 100%;
  flex-flow: column;
}
.getOne {
  width: 90%;
  border-bottom: 1px solid #eee;
  height: 100px;
  display: flex;
  padding-left: 20px;
  align-items: center;
}
.getTwo {
  width: 90%;
  height: 100px;
  display: flex;
  padding-left: 20px;
  align-items: center;
}
.part1 {
  flex: 1;
  color: rgb(197, 180, 27);
}
.part2 {
  flex: 9;
}
.part3 {
  color: rgb(197, 180, 27);
  flex: 1;
  font-size: 13px;
}
.getTwo {
  width: 90%;
  height: 100px;
}
.padding20 {
  padding-left: 20px;
}
.pictureLow {
  padding-top:1px;
  width: 1300px;
  margin: 0 auto;
  background-color: #fff;
}
.el-menu--horizontal .el-menu-item:not(.is-disabled):focus, .el-menu--horizontal .el-menu-item:not(.is-disabled):hover{
  border-bottom:2px solid #fff !important;
  background:none ;
  color:#ccc;
}
.el-menu--horizontal .el-menu-item:not(.is-disabled):hover{
  border-bottom:2px solid #fff;
}
.el-menu--horizontal > .el-menu-item.is-active[data-v-c226fde6]{
  border:none;
}
.el-menu--horizontal>.el-menu-item{
  border-bottom:none;
}
</style>