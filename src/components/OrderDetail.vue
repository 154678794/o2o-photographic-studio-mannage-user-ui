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
      <div class="meal" v-for="(item, index) in book" :key="item.orderId">
        <div style="flex: 6">
          <div style="font-size: 25px">{{ packName[index] }}</div>
          <div style="font-size: 20px; color: rgb(197, 180, 27); display: flex">
            价格:￥{{ item.price }}
            <div style="margin-left: 25px; color: black">
              支付状态:{{ item.hasPay == "是" ? "已支付" : "未支付" }}
            </div>
          </div>
          <div style="font-size: 15px">预约时间:{{ item.appointmentTime }}</div>
          <div style="font-size: 15px">下单时间:{{ item.orderTime }}</div>
        </div>
        <div style="flex:1;display:flex;justify-content:center;align-items:center">
          <el-button
            @click="toChoose(item.orderId)"
            style="background-color: #14161a"
            :disabled="item.hasPay==='是'?false:true"
            >选片</el-button
          >
        </div>
        <div class="twoButton">
          <el-button
            @click="toQuit(item.orderId,item.price)"
            style="background-color: #14161a"
            >取消订单</el-button
          >
          <el-button :disabled="item.hasPay==='是'?true:false"
            @click="toPay(item.orderId, index)"
            style="background-color: #14161a"
            >支付</el-button
          >
        </div>
      </div>
    </div>
    <div style="width:100%;background: #282c31;">
      <div class="footer" >
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
  </div>
</template>
<script type="text/javascript" src='https://webapi.amap.com/maps?v=1.4.11&key=7ab53b28352e55dc5754699add0ad862&plugin=AMap.PlaceSearch'></script>
<script>
import axios from "axios";
import { location } from "../config/map";
export default {
  data() {
    return {
      packName: [],
      book: [],
      comment: [],
      data: {},
      input: "",
      index: 0,
      value: 4.7,
      activeIndex: "1",
      trueV: {},
      submitC: "",
    };
  },
  methods: {
    async getUserinfo() {
      const res = await axios.get("/main/getUserInfo");
      this.data = res.data.data;
    },
    toOther() {
      this.$router.push("/main");
    },
    toSelect(){
      this.$router.push("/select")
    },
    toMessage(){
      this.$router.push("/message")
    },
    toAbout(){
      this.$router.push("/about")
    },
    toRevision(){
        this.$router.push("/revision")
      },
    loginOut() {
      axios.get("/user/userLoginOut");
      this.$message.success("退出成功");
      this.$router.push("/login");
    },
    toOrderDetail() {
      this.$router.push("/orderDetail");
    },
    toChoose(orderId){
      this.$router.push("/choose")
      localStorage.setItem("orderId", JSON.stringify(orderId));
    },
    async toPay(id, price) {
      const orderId = id;
      const orderEntity = {
        orderId: orderId,
      };

      this.$confirm("确认支付?", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(async () => {
         if (Number(this.data.money) >= Number(price)) {
            const money = this.data.money;
            const res = await axios.post(
              "/customOrder/updateHasPay",
              orderEntity
            );
            this.data.money = money - price;
            this.$message({
              type: "success",
              message: "支付成功!",
            });
            this.getAllBook();
          } else {
            this.$message.error("余额不足");
          }
        // console.log(this.data.money)
        // console.log(price)
        //  }
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
    async toQuit(id, index) {
      const orderId = id;

      this.$confirm("确定删除订单?", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(async () => {
          const res = await axios({
            method: "get",
            url: "/adminCustomOrder/deleteOrder",
            params: { orderId },
          });
          this.$message({
            type: "success",
            message: "删除成功!",
          });
          this.packName.splice(index, 1);
          this.getAllBook();
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
    async getAllBook() {
      const res = await axios.get("/packOrder/searchOrder");
      console.log(res);
      this.book = res.data.data;
      this.book.forEach(async (item) => {
        const packId = JSON.parse(item.packId);
        const des = await axios({
          method: "get",
          url: "/shootingPackage/queryPacket",
          params: { packId },
        });
        this.packName.push(des.data.data.packName);
      });
    },
  },

  async created() {
    this.getAllBook();
    this.getUserinfo();
  },
};
</script>
<style scoped>
a {
  text-decoration: none;
  color: black;
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
.main {
  padding-top:20px;
  padding-left:20px;
  background-color: #fff;
  min-height: 600px;
  display: flex;
  flex-direction: column;
  width: 1100px;
  margin: 0 auto;
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
  margin-left:50px;
  color: #666c74;
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
  background-color: #597e72;
  border-color: #597e72;
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