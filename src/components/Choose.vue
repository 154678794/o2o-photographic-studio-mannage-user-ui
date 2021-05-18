<template>
  <div>
    <div class="topbar">
      <div class="container">
        <div class="row">
          <div>
            <!-- <el-input
              suffix-icon="el-icon-shopping-bag-1"
              v-model="input"
            background: url(../assets/logo.png) no-repeat;
              placeholder="请输入套餐"
            ></el-input> -->
            <img @click="toAbout" src="../assets/logo.png" alt="" />
          </div>
          <div class="marginA animate__animated animate__zoomInDown"></div>
          <div class="rightF">
            <el-menu class="el-menu-demo" mode="horizontal">
              <el-menu-item index="1" @click="toSelect">自选套餐</el-menu-item>
              <el-menu-item index="2" @click="toRevision">修图</el-menu-item>
              <el-submenu index="3">
                <template slot="title">{{ data.username }}</template>
                <el-menu-item index="2-1" @click="toMessage">我的信息</el-menu-item>
                <el-menu-item @click="toOrderDetail" index="2-2"
                  ><el-badge :value="200" :max="99" class="item">
                    我的订单
                  </el-badge></el-menu-item
                >
                <el-menu-item index="2-3" @click="loginOut">登出</el-menu-item>
              </el-submenu>
              <el-menu-item index="4">余额:￥{{ data.money }}</el-menu-item>
            </el-menu>
          </div>
        </div>
      </div>
    </div>
    <div class="main">
      <div
        style="
          width: 100%;
          min-height: 500px;
          display: flex;
          flex-direction: row;
        "
      >
        <div
          class="imageC"
          v-for="(item, index) in picture"
          :key="item.photoId"
        >
          <img
            style="max-width: 100%; max-height: 100%;width:100%"
            :src="item.photoUrl"
            @click.prevent="makeChange(index)"
            :class="currentIndex.indexOf(index) !== -1 ? 'actived' : ''"
            alt=""
          />
        </div>
      </div>
      <div class="dada">
        <el-button @click="cancle">取消</el-button>
        <el-button @click="modifiedI">提交</el-button>
      </div>
    </div>
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
          <div style="text-align:center;margin-bottom:15px">关注公众号</div>
          <div style="width: 144px; height: 140px; margin: 0 auto">
            <img
              style="width: 100%; height: 100%"
              src="../assets/erweima.png"
              alt=""
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script type="text/javascript" src='https://webapi.amap.com/maps?v=1.4.11&key=7ab53b28352e55dc5754699add0ad862&plugin=AMap.PlaceSearch'></script>
<script>
import axios from "axios";
import { location } from "../config/map";
export default {
  data() {
    return {
      currentIndex: [],
      picture: [],
      data: {},
      input: "",
      value: 4.7,
      activeIndex: "1",
      trueV: {},
      submitC: "",
    };
  },
  methods: {
    toSelect(){
      this.$router.push("/select")
    },
    toMessage(){
      this.$router.push("/message")
    },
    async getUserinfo() {
      const res = await axios.get("/main/getUserInfo");
      this.data = res.data.data;
    },
    toOther() {
      this.$router.push("/main");
    },
    cancle() {
      this.$router.push("/orderDetail");
    },
    toRevision(){
        this.$router.push("/revision")
      },
    async modifiedI() {
      var checkPhotoVoList = [];
      var orderId = JSON.parse(localStorage.getItem("orderId"));
      for (var i = 0; i < this.currentIndex.length; i++) {
        checkPhotoVoList.push({
          photoId: this.picture[this.currentIndex[i]].photoId,
          orderId: orderId,
        });
      }
      if (checkPhotoVoList.length > 0) {
        const res = await axios.post(
          "/selectPhoto/changeCheckPhotos",
          checkPhotoVoList
        );
        //console.log(res)
        if (res.data.msg === "check更新成功") {
          this.$message.success("提交成功");
        }
      }else{
        this.$message.error("请至少选择一张图片");
      }
    },
    open() {
        this.$notify({
          message: '点击图片即可选择你喜欢的图片呦！再次点击也可以取消哒!!',
          offset: 300
        });
      },
    makeChange(indexJ) {
      console.log(indexJ);
      console.log(indexJ in this.currentIndex);
      if (this.currentIndex.indexOf(indexJ) !== -1) {
        // console.log("indexJ in this.currentIndex");
        // console.log(this.currentIndex);
        // this.currentIndex.forEach((item, index) => {
        //   if (item == indexJ) {
        //     console.log(index);
        //     this.currentIndex.splice(index,1);
        //     console.log(this.currentIndex);
        //   }
        // });
        this.currentIndex = this.currentIndex.filter((item) => {
          return item !== indexJ;
        });
        console.log(this.currentIndex);
      } else {
        this.currentIndex.push(indexJ);
        console.log(this.currentIndex);
      }

      //    console.log(this.currentIndex);
    },
    loginOut() {
      axios.get("/user/userLoginOut");
      this.$message.success("退出成功");
      this.$router.push("/login");
    },
    toOrderDetail() {
      this.$router.push("/orderDetail");
    },
    toChoose() {
      this.$router.push("/choose");
    },
    toAbout(){
      this.$router.push("/about");
    },
    async getAllChoose() {
      const orderId = JSON.parse(localStorage.getItem("orderId"));
      const res = await axios({
        method: "get",
        url: "/selectPhoto/queryGroupPhotos",
        params: { orderId },
      });
      this.picture = res.data.data;
      console.log(res);
    },
  },

  async created() {
    this.getAllChoose();
    this.getUserinfo();
    this.open();
  },
};
</script>
<style scoped>
a {
  text-decoration: none;
  color: black;
}
.actived {
  background-color: #ccc;
}
.dada {
  margin: 0 auto;
  justify-content: space-between;
}
.el-menu {
  background-color: #597e82;
}
.el-menu--horizontal > .el-menu-item.is-active {
  border-bottom: 2px solid #fefeff;
}
.topbar {
  background: #1a1a1c;
  padding: 20px;
}
.container {
  width: 1200px;
  margin: 0 auto;
}
.active {
  background-color: #fff;
}
.imageC {
  width: 236px;
  height: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.main {
  padding-top:20px;
  background-color: #fff;
  min-height: 600px;
  display: flex;
  width: 1180px;
  margin: 0 auto;
  flex-direction: column;
  padding: 20px 10px 0px 10px;
}
.meal {
  display: flex;
  width: 100%;
  margin-bottom: 20px;
}
.twoButton {
  flex: 2;
  display: flex;
  align-items: center;
}
.row {
  align-items: center;
  display: flex;
  flex-wrap: wrap;
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
.rightF {
  width: 430px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.mainCon {
  width: 70%;
  height: 500px;
  margin: 0 auto;
}
.imageS {
  width: 100%;
  height: 100%;
}
.article {
  width: 70%;
  margin: 0 auto;
  background-color: #fff;
  padding-top: 20px;
}
.padding20 {
  padding-left: 20px;
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
.get {
  width: 70%;
  margin: 0 auto;
  background-color: #fff;
  margin-top: 10px;
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
.footer {
  width: 100%;
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
.warpper {
  width: 100%;
}
.Lmain {
  width: 78%;
  height: 155px;
  float: left;
  margin: 26px 0 0 0;
  border-right: 1px solid #14161a;
}
.link {
  width: 100%;
  height: 26px;
  float: left;
  text-align: center;

}
/* .logo {
  clear: both;
  width: 148px;
  height: 108px;
  float: left;
  margin-top: 40px;
  background: url(../assets/logo.png) no-repeat;
  margin-left: 50px;
} */
.txt {
  width: 100%;
  height: 74px;
  float: left;
  margin: 24px 0 0 0px;
  font-size: 16px;
  line-height: 20px;
  color: #666c74;
  margin-left:240px;
}
.aside {
  width: 21%;
  float: left;
  height: 168px;
  background-position: -1364px -490px;
  margin-top: 24px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.detail {
  padding-top: 20px;
  background-color: #fff;
  width: 70%;
  margin: 0 auto;
  margin-top: 20px;
}
.top {
  margin-top: 20px;
  width: 70%;
  margin: 0 auto;
}
.fourT {
  width: 100%;
  display: flex;
  justify-content: space-between;
}
.fram {
  font-size: 24px !important;
  color: #597e72;
}
.mainM {
  width: 100%；;
}
.reduce {
  padding-bottom: 10px;
  border-bottom: 1px solid #eee;
  margin-left: 20px;
}
.know {
  width: 80%;
  padding: 0 10px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
}
.flexR {
  display: flex;
}
.column {
  display: flex;
  flex-direction: column;
}
.other {
  margin-bottom: 20px;
}
.head {
  width: 100%;
  padding: 15px 0;
  margin-left: 20px;
}
.metion {
  padding: 10px 0;
  width: 100%;
  margin-left: 20px;
  border-top: 1px solid #eee;
  display: flex;
}
.metionL {
  flex: 1;
}
.metionR {
  flex: 10;
  margin-right: 20px;
}
.flexD {
  flex-direction: column;
}
.flexB {
  display: flex;
  justify-content: space-between;
}
.texta {
  width: 99%;
  border: none;
}
.fontx {
  font-size: 20px;
}
.colore {
  color: rgba(124, 121, 121, 0.616);
}
.fontC {
  color: orange;
}
.marginR2 {
  margin-left: 10px;
}
.el-button {
  background-color: #282c31;
  border-color: #282c31;
  color: #fff;
}
*:focus {
  outline: none;
}
.font20 {
  font-size: 20px;
}
.el-menu--horizontal .el-menu-item:not(.is-disabled):focus,
.el-menu--horizontal .el-menu-item:not(.is-disabled):hover {
  border-bottom: 2px solid #fff !important;
  background: none;
  color: #ccc;
}
.el-menu--horizontal .el-menu-item:not(.is-disabled):hover {
  border-bottom: 2px solid #fff;
}
.el-menu--horizontal > .el-menu-item.is-active[data-v-c226fde6] {
  border: none;
}
.el-menu--horizontal > .el-menu-item {
  border-bottom: none;
}
.el-menu {
  background-color: #1a1a1c;
}
.el-menu.el-menu--horizontal {
  border: none;
}
.floatRL {
  float: right;
  margin-right: 50px;
  color: rgb(197, 180, 27);
  cursor: pointer;
}
</style>