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
    <div class="revision">
      <div class="mainimage">
        <div class="imageItem" v-for="(item, index) in list" :key="index">
          <img style="max-width:100%;max-height:100%;width:100%" :src="item" alt="" />
        </div>
      </div>
      <div style="margin-left: 26px; margin-top: 20px">
        <el-button @click="downLoadP">下载图片</el-button>
      </div>
      <div
        style="
          margin-left: 26px;
          margin-top: 20px;
          display: flex;
          justify-content: space-between;
        "
      >
        <div style="display: flex">
          <textarea
            style="
          border:2px solid #eee
            border-radius: 5px;
            background-color: #fff;
            width: 455px;
            height: 140px;
            padding: 10px;
            resize: none;
          "
            placeholder="请输入"
            v-model="psMsg"
          ></textarea>
          <el-button @click="tosubmitV">提交</el-button>
        </div>

        <div
          style="
            display: flex;
            flex-direction: column;
            margin-right: 200px;
            align-items: center;
            justify-content: center;
          "
        >
          <a href="javascript:;" class="a-upload">
            <input
              type="file"
              name=""
              id="file"
              @change="popFileName"
            />点击这里上传文件
          </a>

          <!-- <a href="javascript:;" class="file"
        >选择文件
        <input type="file" name="" id="" />
      </a> -->
          <el-button @click="publish" style="margin-top: 20px">发布</el-button>
          <!-- <input type="file" name="test"  id="file" />
      <el-button class="punlishB" type="primary" 
        >发布</el-button
      > -->
        </div>
      </div>
      
    </div>
    <div style="width:100%;background: #282c31; position:absolute;bottom:0px;">
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
      psMsg: "",
      data: {},
      list: [],
      downloadPhotoVo: {
        orderType: "3",
        userId: "2",
        orderId: "2",
      },
      photoId: 0,
    };
  },
  methods: {
    toSelect() {
      this.$router.push("/select");
    },
    toAbout(){
      this.$router.push("/about");
    },
    toRevision() {
      this.$router.push("/revision");
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    toOrderDetail() {
      this.$router.push("/orderDetail");
    },
    toMessage(){
      this.$router.push("/message")
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
    open2() {
        const h = this.$createElement
        this.$notify({
          title: '提示',
          message: h('i', { style: 'color: teal'}, '请先上传照片文件之后再提交呦'),
          offset: 100
        });
      },
    popFileName() {
      var selectedFile = document.getElementById("file").files[0];
      var reader = new FileReader();
      reader.readAsText(selectedFile);
      reader.onload = function () {};
    },
    popFileName() {
      var selectedFile = document.getElementById("file").files[0];
      var reader = new FileReader();
      reader.readAsText(selectedFile);
      reader.onload = function () {};
      // var path = document.getElementById("file").value;
      // this.file_path = path
    },
    async publish() {
      const userId = this.data.userId
      var formData = new FormData();
      console.log(document.getElementById("file").files[0]);
      // console.log(photo)
      formData.append("photo", document.getElementById("file").files[0]);
      formData.append("orderType", 3);
      formData.append("userId", userId);
      //console.log(formData)
      const res = await axios.post("/rights/uploadPhoto", formData);
      this.photoId = res.data.data.split(":")[1];
    },
    async queryPhoto() {
      const res = await axios.get("/photoshop/queryPhotos");
      this.list = res.data.data;
      console.log(res);
    },
    async tosubmitV() {
      // var message = {
      //     "photoId":this.photoId,
      //     "psMsg":this.psMsg
      // }
      var photoId = this.photoId;
      var psMsg = this.psMsg;
      console.log(this.photoId);
      const res = await axios({
          url:"/photoshop/userSendMsg",
          method:'get',
          params: { photoId, psMsg }
    });
    if(res.data=="插入成功"){
        this.$message.success(res.data)
    }
    this.queryPhoto();
    },
    async downLoadP() {
      var orderType = 3;
      var userId = 2;
      var downloadPhotoVo = this.downloadPhotoVo;
      console.log(downloadPhotoVo);
      const res = await axios.post("/rights/downloadPhoto", downloadPhotoVo);
      console.log(res);
    },
  },
  created: function () {
    this.getUserinfo();
    this.queryPhoto();
    this.open2();
    // this.queryPhoto();
    // this.downLoadP();
  },
};
</script>
<style scoped>
a {
  text-decoration: none;
  color: black;
}
.mainimage {
  margin-top: 15px;
  width: 1150px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
}
.imageItem {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 210px;
  height: 220px;
  background-color: #eee;
  margin-right: 15px;
  margin-top:10px;
  margin-bottom:10px;
}
.header {
  width: 100%;
  text-align: center;
  font-size: 24px;
  height: 50px;
  font-weight: bold;
  padding-top: 10px;
}

textarea {
  border: none;
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
  background-color: -internal-light-dark(rgb(255, 255, 255), rgb(59, 59, 59));
  -webkit-rtl-ordering: logical;
  flex-direction: column;
  resize: auto;
  cursor: text;
  white-space: pre-wrap;
  overflow-wrap: break-word;
  column-count: initial !important;
  margin: 0em;
  font: 400 13.3333px Arial;
  border-width: 1px;
  border-style: solid;
  border-color: -internal-light-dark(rgb(118, 118, 118), rgb(133, 133, 133));
  border-image: initial;
  padding: 2px;
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
.revision {
  width: 1200px;
  min-height: 600px;
  margin: 0 auto;
  background-color: #fff;
  padding-top: 20px;
  padding-bottom:20px;
  margin-bottom:10px;
}
.borderB {
  border-bottom: 2px solid #ccc;
}
.marginB {
  margin-bottom: 20px;
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
    margin-top:15px;
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
  border: none !important;
}

.a-upload {
  padding: 4px 10px;
  height: 20px;
  line-height: 20px;
  position: relative;
  cursor: pointer;
  color: #888;
  background: #fafafa;
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
  display: inline-block;
  *display: inline;
  *zoom: 1;
}

.a-upload input {
  position: absolute;
  font-size: 100px;
  right: 0;
  top: 0;
  opacity: 0;
  filter: alpha(opacity=0);
  cursor: pointer;
}

.a-upload:hover {
  color: #444;
  background: #eee;
  border-color: #ccc;
  text-decoration: none;
}

.file {
  position: relative;
  display: inline-block;
  background: #d0eeff;
  border: 1px solid #99d3f5;
  border-radius: 4px;
  padding: 4px 12px;
  overflow: hidden;
  color: #1e88c7;
  text-decoration: none;
  text-indent: 0;
  line-height: 20px;
}
.file input {
  position: absolute;
  font-size: 100px;
  right: 0;
  top: 0;
  opacity: 0;
}
.file:hover {
  background: #aadffd;
  border-color: #78c3f3;
  color: #004974;
  text-decoration: none;
}
</style>