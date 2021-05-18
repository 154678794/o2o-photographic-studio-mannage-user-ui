<template>
  <div>
    <div class="topbar">
      <div class="container">
        <div class="row">
          <div>
            <!-- <el-input
              suffix-icon="el-icon-shopping-bag-1"
              v-model="input"
              placeholder="请输入套餐"
            ></el-input> -->
            <img @click="toAbout" src="../assets/logo.png" alt="" />
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
      <div class="reduce">
        <div class="width:500px">
          <p class="fontx">拍摄特色</p>
          <div class="other">
            <p>
              <span class="colore">相册</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<select
                class="content-type"
                @change="getOption($event)"
              >
                <option  selected ></option>
                <option
                  v-for="(item, index) in list"
                  :key="item.albumId"
                  :value="item.albumId"
                >
                  {{ list[index].albumType }}
                </option>
              </select>
            </p>
            <p>
              <span class="colore">相册数量</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.albumNum"
                :disabled="!orderEntity.albumId"
              />
            </p>
            <p>
              <span class="colore">服装</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<select
                class="content-type"
                 @change="getOption1($event)"
              >
                <option selected ></option>
                <option
                  v-for="(item, index) in list1"
                  :key="item.dressId"
                  :value="item.dressId"
                >
                  {{ list1[index].dressName }}
                </option>
              </select>
            </p>
            <p>
              <span class="colore">服装数量</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.dressNum"
                :disabled="!orderEntity.dressId"
              />
            </p>
            <p>
              <span class="colore">相框</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<select
                class="content-type"
                 @change="getOption2($event)"
              >
                <option selected></option>
                <option
                  v-for="(item, index) in list2"
                  :key="item.frameId"
                  :value="item.frameId"
                >
                  {{ list2[index].frameType }}
                </option>
              </select>
            </p>
            <p>
              <span class="colore">相框数量</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.frameNum"
                :disabled="!orderEntity.frameId"
              />
            </p>
          </div>
        </div>
        <div class="width:500px">
          <div class="other">
            <p class="fontx">拍摄&成品</p>
            <p>
              <span class="colore">拍摄张数</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.takeNum"
              />
            </p>
            <p>
              <span class="colore">精修张数</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.truingNum"
              />
            </p>
            <p>
              <span class="colore">拍摄款式</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.takeType"
              />
            </p>
            <p>
              <span class="colore">拍摄类型</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
                class="inputStyle"
                type="text"
                v-model="orderEntity.descriptionType"
              />
            </p>
            <div class="block">
              <span class="colore" style="width: 99px">预约时间</span>
              <el-date-picker
                v-model="orderEntity.appointmentTime"
                type="datetime"
                placeholder="选择日期时间"
                :picker-options="pickerOptions"
              >
              </el-date-picker>
              
            </div>
          </div>
        </div>
      </div>
      <div class="buttondo" @click="toSubmit">
        <el-button type="info" style="width:130px;margin-bottom:20px">立即下单</el-button>
      </div>
    </div>
    <div style="width:100%;background:#282c31;position:absolute;bottom:0">
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
          <div class="focusOn">关注公众号</div>
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
<script>
import axios from "axios";
export default {
  data() {
    return {
      orderEntity:{
        albumNum:null,
        albumId:null,
        dressId:null,
        frameId:null,
        frameNum:null,
        dressNum:null,
        takeType:'',
        truingNum:null,
        takeNum:null,
        appointmentTime:'',
        descriptionType:''
      },
      index:0,
      index1:1,
      index2:2,
      data:{},
      list: [],
      list1: [],
      list2: [],
      appointmentTime: "",
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() < Date.now();
        },
      },
    };
  },
  methods: {
    getOption(e){
     if(e.target.selectedIndex===0){
       this.orderEntity.albumId = null
     } else{
       this.orderEntity.albumId = this.list[e.target.selectedIndex-1].albumId=this.list[e.target.selectedIndex-1].albumId
     }
    },
    getOption1(e){
      if(e.target.selectedIndex===0){
        this.orderEntity.dressId = null
      }else{
        this.orderEntity.dressId = this.list1[e.target.selectedIndex-1].dressId=this.list1[e.target.selectedIndex-1].dressId
      }
    },
    getOption2(e){
      if(e.target.selectedIndex===0){
        this.orderEntity.frameId = null
      }else{
        this.orderEntity.frameId = this.list2[e.target.selectedIndex-1].frameId= this.list2[e.target.selectedIndex-1].frameId
      }
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
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
      toSelect(){
      this.$router.push("/select")
    },
    async toSubmit(){
      console.log(this.orderEntity)
      if(!this.orderEntity.albumNum){
        this.orderEntity.albumNum = 0
      }
      if(!this.orderEntity.frameNum){
        this.orderEntity.frameNum = 0
      }
      if(!this.orderEntity.dressNum){
        this.orderEntity.dressNum = 0
      }
      const res = await axios.post("/customOrder/sendOrder",this.orderEntity)
      this.orderEntity = {
        albumNum:null,
        albumId:null,
        dressId:null,
        frameId:null,
        frameNum:null,
        dressNum:null,
        takeType:'',
        truingNum:null,
        takeNum:null,
        appointmentTime:'',
        descriptionType:''
      },
      console.log(res)
    },
    toOrderDetail() {
      this.$router.push("/orderDetail");
    },
    loginOut() {
      axios.get("/user/userLoginOut");
      this.$message.success("退出成功");
      this.$router.push("/login");
    },
    async getUserinfo() {
      const res = await axios.get("/main/getUserInfo");
      this.data = res.data.data;
    },
    async getType() {
      var pageNum = 1;
      var pageSize = 50;
      var type = 1;
      const res = await axios({
        url: "/purchaseAndDelivery/queryByType",
        method: "get",
        params: { pageNum, pageSize, type },
      });
      this.list = res.data.data.list;
      console.log(res);
    },
    async getType1() {
      var pageNum = 1;
      var pageSize = 50;
      var type = 2;
      const res = await axios({
        url: "/purchaseAndDelivery/queryByType",
        method: "get",
        params: { pageNum, pageSize, type },
      });
      this.list1 = res.data.data.list;
      console.log(res);
    },
    async getType2() {
      var pageNum = 1;
      var pageSize = 50;
      var type = 3;
      const res = await axios({
        url: "/purchaseAndDelivery/queryByType",
        method: "get",
        params: { pageNum, pageSize, type },
      });
      this.list2 = res.data.data.list;
      console.log(res);
    },
  },
  created: function () {
    this.getUserinfo();
    this.getType();
    this.getType1();
    this.getType2();
  },
};
</script>
<style scoped>
a {
  text-decoration: none;
  color: black;
}
input {
  text-rendering: auto;
  color: -internal-light-dark(black, white);
  letter-spacing: normal;
  word-spacing: normal;
  text-transform: none;
  text-indent: 0px;
  text-shadow: none;
  display: inline-block;
  text-align: start;
  appearance: auto;
}
.el-date-picker{
  width:300px;
}
.buttondo{
  position:relative;
  left:20px;
  width:1180px;
  display: flex;
  justify-content: center;
  background-color: #fff;
}
.inputStyle {
  font-family: sans-serif;
  font-size: 100%;
  line-height: 1.15;
  width: 300px;
  height: 25px;
  margin: 5px 0;
  padding: 8px 10px;
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  background: #fff;
}
.content-type {
  font-size: 14px;
  width: 320px;
  font-weight: 700;
  padding: 5px 40px 5px 10px;
  border: 2px solid #41444e;
  border-radius: 4px;
  background: #f7f7f7
    url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyMCAyMCI+ICAgIDxwYXRoIGQ9Ik0xMy40MTggNy44NTljLjI3MS0uMjY4LjcwOS0uMjY4Ljk3OCAwIC4yNy4yNjguMjcyLjcwMSAwIC45NjlsLTMuOTA4IDMuODNjLS4yNy4yNjgtLjcwNy4yNjgtLjk3OSAwbC0zLjkwOC0zLjgzYy0uMjctLjI2Ny0uMjctLjcwMSAwLS45NjkuMjcxLS4yNjguNzA5LS4yNjguOTc4IDBMMTAgMTFsMy40MTgtMy4xNDF6Ii8+PC9zdmc+)
    right 10px center no-repeat;
  background-size: 20px;
  -webkit-box-shadow: 0 1px 2px 0 rgb(0 0 0 / 25%);
  box-shadow: 0 1px 2px 0 rgb(0 0 0 / 25%);
  font-family: Titillium Web, sans-serif;
  color: #3b4151;
  -webkit-appearance: none;
  appearance: none;
  margin: 3px 0 0;
  border-width: 1px;
  line-height: 1.15;
  height: 40px;
}
.main {
  width: 1200px;
  height: 530px;
  margin: 0 auto;
}
.reduce {
  background-color: #fff;
  padding-bottom: 10px;
  margin-left: 20px;
  display: flex;
  justify-content: space-around;
}
.fontx {
  font-size: 20px;
}
.other {
  margin-bottom: 20px;
}
.colore {
  display: inline-block;
  width: 70px;
  color: rgba(124, 121, 121, 0.616);
}
.header {
  width: 100%;
  text-align: center;
  font-size: 24px;
  height: 50px;
  font-weight: bold;
  padding-top: 10px;
}
.width100 {
  padding-left: 30px;
  margin: 0 auto;
  width: 1270px;
  background-color: #fff;
  height: 80px;
  display: flex;
  align-items: center;
}
.borderB {
  border-bottom: 2px solid #ccc;
}
.marginB {
  margin-bottom: 20px;
}
.footer {
  width: 1200px;
  color: #fff;
}
.topbar {
  background: #1a1a1c;
  padding: 20px;
}
.container {
  width: 1200px;
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
.rightF {
  width: 430px;
  display: flex;
  justify-content: space-between;
  align-items: center;
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
  height: 400px;
  width: 100%;
}
.el-menu--horizontal > .el-menu-item.is-active {
  color: #ccc;
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
.txt {
  width: 100%;
  height: 74px;
  float: left;
  margin: 24px 0 0 0px;
  font-size: 16px;
  line-height: 20px;
  color: #666c74;
  padding-left: 50px;
}
.el-menu.el-menu--horizontal{
  border:none !important;
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
.focusOn {
  margin-bottom: 10px;
  text-align: center;
}
</style>