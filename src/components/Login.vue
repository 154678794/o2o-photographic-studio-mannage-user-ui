<template>
  <div class="main">
    <div class="login_container" v-if="isShow">
      <div class="login_box">
        <!--登录表单区域-->
        <h3 class="main-text">欢迎登陆O2O摄影平台</h3>
        <el-form
          ref="loginFormRef"
          :rules="loginFormRules"
          class="login_form"
          label-width="0px"
          :model="loginForm"
        >
          <!--用户名 -->
          <el-form-item prop="phoneNum">
            <span class="colorRed">*</span>用户名：<el-input
              v-model="loginForm.phoneNum"
              prefix-icon="el-icon-user-solid"
            ></el-input>
          </el-form-item>
          <!--密码 -->
          <el-form-item prop="password">
            <span class="colorRed">*</span>密码：<el-input
              type="password"
              v-model="loginForm.password"
              prefix-icon="el-icon-s-goods"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button @click="toLogin" type="primary" class="new1"
              >登录</el-button
            >
            <el-button @click="changeshow" type="primary">注册</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <div class="login_container" v-else>
      <div class="login_box">
        <!--登录表单区域-->
        <h3 class="main-text">欢迎登陆O2O摄影平台</h3>
        <el-form
          ref="loginFormRef"
          :rules="loginFormRules"
          class="login_form"
          label-width="0px"
          :model="loginForm"
        >
          <!--用户名 -->
          <el-form-item prop="username">
            <span class="colorRed">*</span
            ><span style="text-align: right">用户名：</span>
            <el-input
              v-model="loginForm.username"
              prefix-icon="el-icon-user-solid"
            ></el-input>
          </el-form-item>
          <!-- 真实姓名 -->
          <el-form-item>
            <span style="text-align: right">真实姓名：</span
            ><el-input
              v-model="loginForm.realName"
              prefix-icon="el-icon-user-solid"
            ></el-input>
          </el-form-item>
          <!-- 出生日期 -->
          <el-form-item>
            <span style="text-align: right">出生日期：</span
            ><el-input
              v-model="loginForm.birth"
              prefix-icon="el-icon-user-solid"
            ></el-input>
          </el-form-item>
          <!--密码 -->
          <el-form-item prop="password">
            <span class="colorRed">*</span
            ><span style="text-align: right">密码：</span>
            <el-input
              type="password"
              v-model="loginForm.password"
              prefix-icon="el-icon-s-goods"
            ></el-input>
          </el-form-item>
          <!-- 电话号码 -->
          <el-form-item prop="phoneNum">
            <span class="colorRed">*</span
            ><span style="text-align: right">电话号码：</span>
            <el-input
              v-model="loginForm.phoneNum"
              prefix-icon="el-icon-s-goods"
            ></el-input>
          </el-form-item>
          <!-- 验证码 -->
          <el-form-item prop="msg">
            <button class="primaryB" @click.prevent="getMsg">获取验证码</button>
            <el-input
              v-model="loginForm.msg"
              placeholder="请输入验证码"
            ></el-input>
          </el-form-item>
          <!-- 性别 -->
          <span style="text-align: right">性别：</span>
          <el-radio v-model="radio" label="1">boy</el-radio>
          <el-radio v-model="radio" label="2">girl</el-radio>

          <el-form-item>
            <el-button @click.stop="toRigister" type="primary" class="marginT"
              >注册</el-button
            >
            <el-button @click.stop="changeshow" type="primary" class="marginT"
              >返回登录</el-button
            >
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      isShow: true,
      radio: "",

      loginForm: {
        username: "",
        password: "",
        phoneNum: "",
        realName: "",
        msg: "",
        birth: "",
      },
      //表单验证规则
      loginFormRules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 10, message: "长度在3-10个字符", trigger: "blur" },
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 12, message: "长度在6-12个字符", trigger: "blur" },
        ],
        phoneNum: [
          { required: true, message: "电话号码为必填项", trigger: "blur" },
        ],
        msg: [{ required: true, message: "验证码为必填项", trigger: "blur" }],
      },
    };
  },
  methods: {
    changeshow: function () {
      this.isShow = !this.isShow;
    },
    async getMsg() {
      var phone = this.loginForm.phoneNum;
      const res = await axios.get("user/sendMsg", {
        params: { phone },
      });
      console.log(res);
    },
    async toRigister() {
      var userRegistVo = this.loginForm;
      console.log(userRegistVo);
      const res = await axios.post("user/userRegist", userRegistVo);
      //console.log(res)
      if (res.data == "注册成功") {
        this.$message.success("恭喜你 注册成功 请登录");
        this.isShow = !this.isShow;
      } else {
        this.$message.error("注册失败");
      }
    },
    async toLogin() {
      var generalUserEntity = this.loginForm;
      const res = await axios.post("user/userLogin", generalUserEntity);
      console.log(res.data);
      if (res.data.data === true) {
        const els = axios.post("/user/setJSessionId", generalUserEntity, {
          withCredentials: true,
        });
        console.log(els);
        this.$router.push("/about")
      } else {
        this.$message.error("登陆失败");
      }
    },
  },
  mounted() {
    axios.get("/user/cookieValid");
  },
};
</script>
<style scoped>
.main {
  background-image: url(../assets/login_image.jpg);
  background-repeat: no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login_container {
  height: 360px;
  width: 450px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login_box {
  border-radius: 6px;
  background: #fff;
  width: 400px;
  padding: 25px 20px 5px 20px;
}
.main-text {
  text-align: center;
  margin: 0 auto 30px auto;
  color: #707070;
}
.login_form {
  margin-top: 40px;
  padding: 0 20px;
  box-sizing: border-box;
}
.colorRed {
  color: red;
}
.el-input {
  position: relative;
  font-size: 14px;
  display: inline-block;
  width: 75%;
  float: right;
}
.el-form-item__error {
  position: absolute;
  left: 100px;
}
.el-form-item {
  margin-bottom: 25px;
}
.new1 {
  margin-left: 70px;
  margin-right: 60px;
}
.primaryB {
  float: right;
  width: 85px;
  height: 40px;
  margin-left: 5px;
  color: #fff;
  background-color: #409eff;
  border-color: #409eff;
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  border: 1px solid #dcdfe6;
  -webkit-appearance: none;
  text-align: center;
  transition: 0.1s;
  font-weight: 500;
  padding: 3px 8px;
  font-size: 14px;
  border-radius: 4px;
}
.marginT {
  margin-top: 15px;
}
</style>

