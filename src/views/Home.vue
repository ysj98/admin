<template>
  <div class="home">
    <el-container>
      <div style="height: 100%; width: 100%">
        <el-aside>
          <div class="acccls">
            <acccls></acccls>
          </div>
        </el-aside>
      </div>
    </el-container>
    <el-container>
      <!--header-->
      <el-header>
        <div class="headercls">
          <div class="header-user-con">
            <div class="collapse-btn" @click="collapseChage">
              <i class="el-icon-menu"></i>
              &nbsp;
            </div>
            <div class="logo">
              <b>admin</b>
              &nbsp;后台管理系统
            </div>

            <!--                <el-radio-group v-model="isCollapse" style="margin-bottom: 20px;">
                  &lt;!&ndash;    <el-radio-button :label="false">展开</el-radio-button>
                      <el-radio-button :label="true">收起</el-radio-button>&ndash;&gt;
                  </el-radio-group>-->
            <!-- 全屏显示 -->
            <div class="btn-fullscreen" @click="handleFullScreen">
              <el-tooltip class="fullcls" effect="dark" :content="fullscreen ? `取消全屏` : `全屏`" placement="bottom">
                <i class="el-icon-rank"></i>
              </el-tooltip>
            </div>

            <!-- 用户头像 -->
            <div class="user-avator">
              <img src="../assets/images/login/loginmsg.jpg" />
            </div>
            <!-- 用户名下拉菜单 -->
            <el-dropdown class="user-name" trigger="click" @command="handleCommand">
              <span class="el-dropdown-link">
                admin
                <!-- {{ sessionData.loginName }} -->
                <i class="el-icon-caret-bottom"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <a href="https://github.com/ysj98/admin" target="_blank">
                  <el-dropdown-item>项目仓库</el-dropdown-item>
                </a>
                <el-dropdown-item divided command="loginout">退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </div>
        </div>
      </el-header>
      <el-main style="margin: 0; height: 100%">
        <div class="content-box" :class="{ 'content-collapse': collapse }">
          <tags></tags>
          <div class="content">
            <transition name="move" mode="out-in">
              <keep-alive :include="tagsList">
                <router-view></router-view>
              </keep-alive>
            </transition>
          </div>
        </div>
      </el-main>
    </el-container>

    <a target="_blank" style="position: fixed; z-index: 999999; color: #3a8ee6; left: 97%; top: 40%" href="https://jq.qq.com/?_wv=1027&k=5Wv4Y8x">
      <img style="width: 30px; height: 30px" target="_blank" src="../assets/images/qq.png" alt="点击加入全栈交流群" title="全栈交流群" />
    </a>
  </div>
</template>

<script>
import tags from '@/components/tags.vue'
import bus from '@/components/bus.js'
import Acccls from '../components/acccls'
export default {
  name: 'home',
  components: {
    Acccls,
    tags
  },
  data() {
    return {
      sessionData: [],
      fullscreen: false,
      isCollapse: false,
      tagsList: [],
      collapse: false
    }
  },
  methods: {
    // 用户名下拉菜单选择事件
    handleCommand(command) {
      if (command == 'loginout') {
        sessionStorage.removeItem('loginMsg')
        this.$router.push('/login')
      }
    },
    // 侧边栏折叠
    collapseChage() {
      this.isCollapse = !this.isCollapse
    },
    // 全屏事件
    handleFullScreen() {
      let element = document.documentElement
      if (this.fullscreen) {
        if (document.exitFullscreen) {
          document.exitFullscreen()
        } else if (document.webkitCancelFullScreen) {
          document.webkitCancelFullScreen()
        } else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen()
        } else if (document.msExitFullscreen) {
          document.msExitFullscreen()
        }
      } else {
        if (element.requestFullscreen) {
          element.requestFullscreen()
        } else if (element.webkitRequestFullScreen) {
          element.webkitRequestFullScreen()
        } else if (element.mozRequestFullScreen) {
          element.mozRequestFullScreen()
        } else if (element.msRequestFullscreen) {
          // IE11
          element.msRequestFullscreen()
        }
      }
      this.fullscreen = !this.fullscreen
    },
    handleOpen(key, keyPath) {
      console.log(key, keyPath)
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath)
    }
  },
  created() {
    this.sessionData = JSON.parse(sessionStorage.getItem('loginMsg'))

    bus.$on('collapse', msg => {
      this.collapse = msg
    })

    // 只有在标签页列表里的页面才使用keep-alive，即关闭标签之后就不保存到内存中了。
    bus.$on('tags', msg => {
      let arr = []
      for (let i = 0, len = msg.length; i < len; i++) {
        msg[i].name && arr.push(msg[i].name)
      }
      this.tagsList = arr
    })
  }
}
</script>

<style scoped>
.home {
  position: relative;
  top: 0;
  width: 100%;
  height: 100%;
}

.headercls {
  position: absolute;
  left: 0%;
  width: 100%;
  height: 70px;
  background-color: rgba(90, 179, 255, 1);
}
.headercls .collapse-btn {
  color: #fff;
  float: left;
  cursor: pointer;
  margin-top: 12px;
  margin-left: 10px;
  font-size: 22px;
}
.headercls .logo {
  color: #fff;
  float: left;
  width: 250px;
  margin-top: 10px;
  font-size: 22px;
}
.headercls .logo > b {
  font-family: 华文隶书;
}
.acccls {
  position: absolute;
  height: 100%;
  width: 100%;
}

.user-name {
  position: absolute;
  top: 27px;

  left: 90%;
}
.user-avator {
  display: inline-block;
  float: right;
  margin-right: 11%;
  margin-top: -22px;
}
.user-avator img {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.el-dropdown-link {
  color: #fff;
  cursor: pointer;
}
.btn-fullscreen {
  position: relative;
  height: 40px;
  width: 40px;
  left: 97%;
  top: -5px;
  margin-right: 5px;
  font-size: 24px;
  transform: rotate(47deg);
  color: #fff;
}
.btn-fullscreen .fullcls {
  border: 2px solid #fff;
}
.btn-fullscreen .fullcls:hover {
  border: 2px solid #fff;
  cursor: pointer;
}
</style>
