<template>
    <el-container class="home-container">
        <!-- header-->
        <el-header>
            <div>
                <img style="width:50px;height:50px;" src="../assets/logo1.jpg" alt="">
                <span>Liovee小区后台管理系统</span>
            </div>
            <el-button type="info" @click="logout"> 退出</el-button>
        </el-header>
    <el-container>
        <!-- aside-->
        <el-aside :width="isCollapse ? '64px' : '200px'">
            <div class="toggle-button" @click="toggleCollapse">
                |||
            </div>
        <!--侧边栏菜单区域 -->
            <el-menu
      class="el-menu-vertical-demo"
      :router="true"
      background-color="#333744"
      text-color="#fff"
      active-text-color="#409BFF" unique-opened :collapse="isCollapse" :collapse-transition="false"
      :default-active="activePath">
      <!--一级菜单 -->
      <el-submenu  :index="item.id + ''" v-for="item in menulist" :key="item.id">
      <!--一级菜单的模板 -->
        <template slot="title">
          <i :class="iconsObj[item.id]"></i>
          <span>{{item.authName}}</span>
        </template>
        <!--二级菜单 -->
        <el-menu-item v-for="subItem in item.children" :key="subItem.id" :index="'/' + subItem.path" @click="saveNavState('/' + subItem.path)">
            <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{subItem.authName}}</span>
            </template>
        </el-menu-item>
      </el-submenu>
    </el-menu>
        </el-aside>
        <!-- main-->
        <el-main>
            <router-view></router-view>
        </el-main>
    </el-container>
    </el-container>
</template>
<script>
export default {
    data(){
        return {
            menulist:[],
            iconsObj:{
                '125':'iconfont icon-icon_black-20',
                '103':'iconfont icon-icon_black-27',
                '101':'iconfont icon-icon_black-15',
                '102':'iconfont icon-icon_black-32',
                '145':'iconfont icon-icon_black-11'
            },
            isCollapse:false,
            //被激活的链接地址
            activePath:''
        }
    },
    created(){
        this.getMenuList()
        this.activePath = window.sessionStorage.getItem('activePath')
    },
    methods:{
        logout:function(){
            window.sessionStorage.clear();
            this.$router.push("/login");
        },

        async getMenuList(){
            const {data:res} = await this.$http.get('menus')
            // console.log(res)
            if(res.meta.status !== 200) return this.$message.error(res.meta.msg)
            this.menulist = res.data
        },
        //点击按钮切换按钮折叠与展开
        toggleCollapse(){
            this.isCollapse = !this.isCollapse
        },
        //保存链接的激活状态
        saveNavState(activePath) {
            window.sessionStorage.setItem('activePath',activePath)
            this.activePath = activePath
        }
    }
}
</script>

<style scoped>
    .home-container{
        height: 100%;
    }
    .el-header{
        background-color: #373d41;
        display:flex;
        justify-content: space-between;
        padding-left:0;
        align-items: center;
        color: #fff;
        font-size: 20px;

    }
    .el-header>div{
        display: flex;
        align-items: center;
    }
    .el-header>div span{
        margin-left:15px;
    }
    .el-aside{
        background-color: #333744;
    }
    .el-aside .el-menu{
        border-right: none;
    }
    .el-main{
        background-color: #eaedf1;
    }
    .iconfont{
        margin-right: 15px;;
    }
    .toggle-button{
        text-align: center;
        background-color: #4A5056;
        font-size: 10px;
        line-height: 20px;
        color: #fff;
        letter-spacing: 0.2em;
        cursor: pointer;
    }
</style>