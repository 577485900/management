<template>
  <el-container class="home_container">
    <el-header>
      <div>
        <img src="../assets/icon.png" alt="">
        <span>学海无涯苦作舟</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle_button" @click="toggleCollapse">|||</div>
        <el-menu
          background-color="red"
          text-color="#fff"
          active-text-color="blue"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath">
          <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
            <template slot="title">
              <i :class="iconsObj[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item :index="subItem.path" v-for="subItem in item.children" :key="subItem.id"
            @click="saveNavState(subItem.path)">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menuList: [],
      iconsObj: {
        125: 'el-icon-s-custom',
        103: 'el-icon-s-order',
        101: 'el-icon-s-goods',
        102: 'el-icon-s-promotion',
        145: 'el-icon-s-home'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')

      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)

      this.menuList = res.data
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
  .home_container{
    height: 100%;
  }

  .el-header{
    background-color: black;
    display: flex;
    justify-content: space-between;
    padding-left: 0;
    align-items: center;
    color: #fff;
    font-size: 20px;
      div{
        display: flex;
        align-items: center;

        img{
          width: 50px;
          height: 50px;
        }

        span{
          margin-left: 15px;
        }
      }
    }

  .el-aside{
    background-color: red;
    .el-menu{
      border-right: none;
    }
  }

  .el-main{
    background-color: green;
  }

  .toggle_button{
    background-color: blue;
    font-size: 10px;
    line-height: 24px;
    color: #fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
  }
</style>
