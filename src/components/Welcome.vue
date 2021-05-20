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
            <el-menu
              class="el-menu-demo"
              mode="horizontal"
              @select="handleSelect"
            >
              <el-menu-item index="1"  @click="toSelect">自选套餐</el-menu-item>
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
    <div class="mainCon animate__animated animate__lightSpeedInRight">
      <img class="imageS" :src="referencePicture? referencePicture:list[Number(index)].url" alt="" />
    </div>
    <div class="article">
      <p class="text margin0 padding20">{{trueV.packName}}</p>
      <el-rate
        class="padding20"
        v-model="value"
        show-score
        text-color="#ff9900"
        score-template="{value}"
      >
      </el-rate>
      <span class="font20">￥{{ trueV.packPrice }}</span>
      <p class="font16 padding20">
        个性写真 <span>步行街</span
        ><span class="floatRL" @click="handleClose">点我下单</span>
      </p>
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
          <div class="borderR" @click="toMap">
            <i class="el-icon-house"></i><span>地址</span>
          </div>
          <div class="phone">
            <i class="el-icon-phone-outline"></i><span>电话</span>
          </div>
        </div>
      </div>
    </div>
    <div class="get">
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
    <div class="detail">
      <div class="top">
        <div class="fourT">
          <div class="fram fontx"><a href="#c1">商品详情</a></div>
          <div class="fram fontx"><a href="#c2">购买须知</a></div>
          <div class="fram fontx"><a href="#c3">网友评价</a></div>
          <div class="fram fontx"><a href="#c4">相关推荐</a></div>
        </div>
      </div>
      <div class="mainM">
        <div class="reduce">
          <p class="fontx"><a name="c1">商品介绍</a></p>
          <div class="know">
            <div class="flexR">
              <div class="iconfont icon-xiangce"></div>
              <div class="column">
                <div>服装</div>
                <div>三套</div>
              </div>
            </div>
            <div class="flexR">
              <div class="iconfont icon-jiandao"></div>
              <div class="column">
                <div>拍摄</div>
                <div>60张</div>
              </div>
            </div>
            <div class="flexR">
              <div class="iconfont icon-jiaojuan"></div>
              <div class="column">
                <div>精修</div>
                <div>15张</div>
              </div>
            </div>
            <div class="flexR">
              <div class="iconfont icon-fuzhuang"></div>
              <div class="column">
                <div>底片</div>
                <div>14张</div>
              </div>
            </div>
          </div>
        </div>
        <div class="reduce">
          <p class="fontx">亮点</p>
          <p>{{ trueV.highlights.split("\\")[0] }}</p>
          <p>{{ trueV.highlights.split("\\n")[1] }}</p>
        </div>
        <div class="reduce">
          <p class="fontx">拍摄特色</p>
          <div class="other">
            <p>拍摄特色</p>
            <p>
              <span class="colore">拍摄人数</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.photographersNum }}人</span
              >
            </p>
            <p>
              <span class="colore">拍摄风格</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.shootingStyle
              }}</span>
            </p>
          </div>
          <div class="other">
            <p class="fontx">造型</p>
            <p>
              <span class="colore">服装</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.modelingClothing }}套/人</span
              >
            </p>
            <p>
              <span class="colore">化妆</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.makeUp }}套/人</span
              >
            </p>
          </div>
          <div class="other">
            <p class="fontx">场景</p>
            <p>
              <span class="colore">内景数量</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.interiorNum }}个</span
              >
            </p>
            <p>
              <span class="colore">外景数量</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.locationNum }}个</span
              >
            </p>
          </div>
          <div class="other">
            <p class="fontx">拍摄&成品</p>
            <p>
              <span class="colore">拍摄张数</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.shotNum }}张</span
              >
            </p>
            <p>
              <span class="colore">精修张数</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.refinedNum }}张</span
              >
            </p>
            <p>
              <span class="colore">底片赠送</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>14张</span>
            </p>
            <p>
              <span class="colore">相册</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.albumNum }}个</span
              >
            </p>
            <p>
              <span class="colore">相框</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span
                >{{ trueV.frameNum }}个</span
              >
            </p>
            <p>
              <span class="colore">相册说明</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.albumDescription
              }}</span>
            </p>
            <p>
              <span class="colore">相框说明</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.frameDescription
              }}</span>
            </p>
            <p>
              <span class="colore">其他产品</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.otherProducts
              }}</span>
            </p>
          </div>
          <div class="other">
            <p class="fontx">其他</p>
            <p>
              <span class="colore">其他费用</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.otherExpenses
              }}</span>
            </p>
            <p>
              <span class="colore">取件方式</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.pickMethod
              }}</span>
            </p>
            <p>
              <span class="colore">取件说明</span
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>{{
                trueV.pickInstructions
              }}</span>
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="detail">
      <div class="reduce">
        <p class="fontx"><a name="c2">购买须知</a></p>
        <p class="fontC">有效期</p>
        <p class="marginR2">从购买之日起360天内有效</p>
        <p class="fontC">预约信息</p>
        <p class="marginR2">请您提前七天预约</p>
        <p class="fontC">可用时间</p>
        <p class="marginR2">10:00:00至19:00:00</p>
        <p class="fontC">温馨提示</p>
      </div>
    </div>
    <div class="detail">
      <div class="head">
        <a name="c3">{{ comment.length }}条评价</a>
      </div>
      <div class="metion" v-for="item in comment" :key="item.userId">
        <div class="iconfont icon-subscriber-square metionL"></div>
        <div class="metionR">
          <div class="flexD">
            <div class="flexB">
              <div>{{ item.username }}</div>
              <div>{{ item.uploadTime }}</div>
            </div>
            <div class="comment">{{ item.comment }}</div>
          </div>
        </div>
      </div>
    </div>
    <div class="detail">
      <textarea
        class="texta"
        rows="10"
        placeholder="请输入您的评价"
        v-model="submitC"
      ></textarea>
      <div><el-button @click="tosubmit">提交</el-button></div>
    </div>
    <el-dialog title="提示" :visible.sync="dialogVisible" width="30%">
      <div class="block">
        <span class="demonstration">请选择预约时间</span>
        <el-date-picker
          v-model="appointmentTime"
          type="datetime"
          placeholder="选择日期时间"
          :picker-options="pickerOptions"
        >
        </el-date-picker>
      </div>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="toBook">确 定</el-button>
      </span>
    </el-dialog>
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
      appointmentTime: "",
      dialogVisible: false,
      comment: [],
      data: {},
      input: "",
      index: 0,
      value: 4.7,
      activeIndex: "1",
      trueV: {},
      submitC: "",
      list: [
        { url: require("../assets/demo1.jpg") },
        { url: require("../assets/demo2.jpg") },
        { url: require("../assets/demo3.jpg") },
        { url: require("../assets/demo4.jpg") },
        { url: require("../assets/demo5.jpg") },
      ],
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() < Date.now();
        },
      },
    };
  },
  methods: {
    /**获取地图定位*/
    getLocation() {
      let _that = this;
      let geolocation = location.initMap("map-container"); //定位
      AMap.event.addListener(geolocation, "complete", (result) => {
        _that.lat = result.position.lat;
        _that.lng = result.position.lng;
        _that.province = result.addressComponent.province;
        _that.city = result.addressComponent.city;
        _that.district = result.addressComponent.district;
      });
    },
    async getUserinfo() {
      const res = await axios.get("/main/getUserInfo");
      this.data = res.data.data;
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    toOther() {
      this.$router.push("/main");
    },
    handleClose() {
      this.dialogVisible = true;
    },
    loginOut() {
      axios.get("/user/userLoginOut");
      this.$message.success("退出成功");
      this.$router.push("/login");
    },
   getNowFormatDate(date) {
    var seperator1 = "-";//设置成自己想要的日期格式 年/月/日
    var seperator2 = ":";//设置成自己想要的时间格式 时:分:秒
    var month = date.getMonth() + 1;//月
    var strDate = date.getDate();//日
    var hours = date.getHours();
    var minutes = date.getMinutes();
    var seconds = date.getSeconds();
    if (month >= 1 && month <= 9)
    {
        month = "0" + month;
    }
    if (strDate >= 0 && strDate <= 9)
    {
        strDate = "0" + strDate;
    }
    if (hours >= 0 && hours <= 9)
    {
        hours = "0" + hours;
    }
    if (minutes >= 0 && minutes <= 9)
    {
        minutes = "0" + minutes;
    }
    if (seconds >= 0 && seconds <= 9)
    {
        seconds = "0" + seconds;
    }
    var currentdate = date.getFullYear() + seperator1 + month + seperator1 + strDate
         + " " + hours + seperator2 + minutes
         + seperator2 + seconds;
    return currentdate;
    },
    async toBook() {
      const packd = JSON.parse(localStorage.getItem("trueV"));
      const money = this.trueV.packPrice;
      const appoint = this.appointmentTime;
      const appointmentTime = this.getNowFormatDate(appoint)
      console.log(this.data);
      console.log(packd);
      const orderEntity = {
        packId: packd,
        price: money,
        appointmentTime: appointmentTime,
      };
      if (this.appointmentTime !== "") {
        this.dialogVisible = false;
        this.$confirm("确定下单嘛?", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning",
        })
          .then(async () => {
            console.log(orderEntity)
            const res = await axios.post("/customOrder/sendOrder", orderEntity);
            console.log(res);
            if (res.data.data === "订单插入成功") {
              this.$message({
                type: "success",
                message: "下单成功!",
              });
            }
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "已取消",
            });
          });
      } else {
        this.$message.error("请输入预约时间！");
        return;
      }
      // console.log(res);
    },
    toOrderDetail() {
      this.$router.push("/orderDetail");
    },
    toMap() {
      this.getLocation(); // 调用获取地理位置
    },
    toSelect(){
      this.$router.push("/select")
    },
    toAbout(){
        this.$router.push("/about")
    },
    toRevision(){
        this.$router.push("/revision")
      },
      toMessage(){
      this.$router.push("/message")
    },
    async getComment() {
      const packId = JSON.parse(localStorage.getItem("trueV"));
      const res = await axios({
        method: "get",
        url: "/comment/queryComment",
        params: { packId },
      });
      this.comment = res.data.data;
      //console.log(this.comment);
    },
    async tosubmit() {
      const comment = this.submitC;
      const packId = JSON.parse(localStorage.getItem("trueV"));
      const userId = this.data.userId;
      const commentEntity = {
        comment: comment,
        packId: packId,
        userId: userId,
      };
      const res = await axios.post("/comment/writeComment", commentEntity);
      // console.log(res)
      if (!res.data.data) {
        this.$message.error("请先购买套餐再评论");
        this.submitC = "";
      } else {
        this.$message.success("评论成功");
        this.submitC = "";
      }
    },
  },

  // mounted() {
  //   this.getLocation(); // 调用获取地理位置
  // },
  async created() {
    this.getUserinfo();
    this.getComment();
    const referencePicture = localStorage.getItem('referencePicture')
    this.referencePicture = referencePicture
    const packId = JSON.parse(localStorage.getItem("trueV"));
    const index = JSON.parse(localStorage.getItem("index"));
    this.index = index;
    const res = await axios({
      method: "get",
      url: "/shootingPackage/queryPacket",
      params: { packId },
    });
    this.trueV = res.data.data;
    //this.trueV = trueV
    console.log(this.trueV);
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
  text-align: center;
  width: 100%;
  height: 26px;
  float: left;
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
  margin-left:50px;
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
.block {
  margin: 0 0 30px 0;
}
.dialog-footer {
  display: flex;
  justify-content: space-around;
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
.el-date-editor{
  margin-left:20px;
}
</style>