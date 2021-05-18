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
            <img src="../assets/logo.png" alt="" />
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
                <el-menu-item index="2-1" @click="toMessage"
                  >我的信息</el-menu-item
                >
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
    <div class="message">
      <p class="fontx">个人信息</p>
      <p>
        <span class="colore">用户名</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <input class="inputStyle" type="text" v-model="data.username" />
      </p>
      <p>
        <span class="colore">真实姓名</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
          class="inputStyle"
          type="text"
          v-model="data.realName"
        />
      </p>
      <p>
        <span class="colore">密码</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
          class="inputStyle"
          type="password"
          v-model="passwordd"
        />
      </p>
      <p>
        <span class="colore">确认密码</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input
          class="inputStyle"
          type="password"
          v-model="quert"
          @input="alert11()"
        />
      </p>
      <span v-if="isShow" style="color: red">确认密码有误</span>
      <p>
        <span class="colore">电话号码</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <!-- <input
                class="inputStyle"
                type="text"
                v-model="data.phoneNum"
              /> -->
        <span style="display: inline-block; width: 310px; text-align: left">{{
          data.phoneNum
        }}</span>
      </p>
      <p>
        <span class="colore" style="position: relative; left: -103px">性别</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <el-radio
          style="position: relative; left: -103px"
          v-model="data.sex"
          label="1"
          >男</el-radio
        >
        <el-radio
          style="position: relative; left: -103px"
          v-model="data.sex"
          label="2"
          >女</el-radio
        >
      </p>
      <p>
        <span class="colore">生日</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <!-- <input
                class="inputStyle"
                type="text"
                v-model="data.birth"
              /> -->
        <span style="display: inline-block; width: 310px; text-align: left">{{
          data.birth
        }}</span>
      </p>
      <p>
        <span class="colore">余额</span
        >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <!-- <input
                class="inputStyle"
                type="text"
                v-model="data.money"
              /> -->
        <span style="display: inline-block; width: 310px; text-align: left">{{
          data.money
        }}</span>
      </p>
      <el-button @click="ModifiedM" :disabled="isShow||!quert">修改提交</el-button>
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
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      data: {},
      passwordd: "",
      quert: "",
      isShow: false,
    };
  },
  methods: {
    alert11() {
    if(this.quert === ''){
        this.isShow = false
    }else{
        this.isShow = !(this.quert == this.passwordd)
    }
    },
    toMessage() {
      this.$router.push("/message");
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    toOrderDetail() {
      this.$router.push("/orderDetail");
    },
    toRevision() {
      this.$router.push("/revision");
    },
    loginOut() {
      axios.get("/user/userLoginOut");
      this.$message.success("退出成功");
      this.$router.push("/login");
    },
    async getUserinfo() {
      const res = await axios.get("/main/getUserInfo");
      this.data = res.data.data;
      console.log(this.data);
    },
    async ModifiedM() {
      const generalUserEntity = {
        birth: this.data.birth,
        money: this.data.money,
        username: this.data.username,
        sex: this.data.sex,
        realName: this.data.realName,
        phoneNum: this.data.phoneNum,
        password: this.data.password,
      };
      console.log(generalUserEntity);
      // const res = await axios.post("/userManager/updateUser",)
    },
    toSelect() {
      this.$router.push("/select");
    },
  },
  created: function () {
    this.getUserinfo();
  },
};
</script>
<style scoped>
a {
  text-decoration: none;
  color: black;
}
.header {
  width: 100%;
  text-align: center;
  font-size: 24px;
  height: 50px;
  font-weight: bold;
  padding-top: 10px;
}
.colore {
  display: inline-block;
  width: 70px;
  color: rgba(124, 121, 121, 0.616);
}
.fontx {
  font-size: 20px;
}
.inputStyle {
  font-family: sans-serif;
  font-size: 100%;
  line-height: 1.15;
  width: 300px;
  height: 25px;
  margin: 5px 0;
  border: none !important;
  padding: 8px 10px;
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  background: #fff;
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
.message {
  padding-top: 8px;
  text-align: center;
  width: 1200px;
  height: 700px;
  margin: 0 auto;
  background-color: #fff;
}
.borderB {
  border-bottom: 2px solid #ccc;
}
.marginB {
  margin-bottom: 20px;
}
.footer {
  width: 100%;
  background: #597e72;
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
  padding-left: 90px;
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
.el-menu.el-menu--horizontal {
    border-bottom: none !important;
}
</style>