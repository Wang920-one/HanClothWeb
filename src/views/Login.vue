<template>
  <div id="bgImg">
    <el-container>
      <el-header>
        <router-link to="/index">
          <el-tooltip
            content="回到首页"
            placement="bottom"
            effect="light"
          >
            <img
              class="logologin"
              src="@/assets/images/logo.png"
            />
          </el-tooltip>
        </router-link>
      </el-header>
      <span class="words">欢迎加入华夏衣裳汉服交流网！</span>
      <el-form
        :model="loginForm"
        :rules="rules"
        ref="loginForm"
        label-width="100px"
        class="demo-loginForm"
      >
        <div style="padding-top:20px;font-family:'楷体';font-size:20px;color:#fff">
          <el-tooltip
            content="回到首页"
            placement="bottom"
            effect="light"
          >
            <el-button
              type="primary"
              @click="toIndex"
              class="goback"
              icon="el-icon-back"
            ></el-button>
            </el-tooltip>
              <span>用户登录</span>
        </div>
        <el-form-item
          label="账号"
          prop="userAccount"
        >
          <el-input
            v-model="loginForm.userAccount"
            placeholder="账号"
          ></el-input>
        </el-form-item>
        <el-form-item
          label="密码"
          prop="userPassword"
        >
          <el-input
            type="password"
            v-model="loginForm.userPassword"
            placeholder="密码"
          ></el-input>
        </el-form-item>

        <el-form-item>
          <el-button
            class="buttona"
            type="primary"
            @click="handleLogin()"
          >登录</el-button>
          <el-button
            class="buttona"
            type="primary"
            @click="toRegister"
          >注册</el-button>
        </el-form-item>
        <div>
          <el-tooltip
            content="管理员登录"
            placement="bottom"
            effect="light"
          >
            <el-button
              type="text"
              style="padding-top:0"
              @click="toManage"
            >管理员
            </el-button>
          </el-tooltip>
        </div>
      </el-form>
    </el-container>
  </div>
</template>
<script>
import Footer from "@/components/Footer.vue";
import Head from "@/components/Head.vue";
import ScrollTop from "@/components/ScrollTop.vue";
import { mixin } from "../mixins";
import { LoginIn } from "../api/index";
export default {
  name: "login",
  name: "bgImg",
  mixins: [mixin],
  data() {
    return {
      loginForm: {
        userAccount: "",
        userPassword: "",
      },
      rules: {
        userAccount: [
          { required: true, message: "请输入账号", trigger: "blur" },
        ],
        userPassword: [
          { required: true, message: "请输入密码", trigger: "change" },
        ],
      },
    };
  },
  components: {
    Head,
    Footer,
    ScrollTop,
  },
  methods: {
    handleLogin() {
      let _this = this;
      let params = new URLSearchParams();
      params.append("userAccount", this.loginForm.userAccount);
      params.append("userPassword", this.loginForm.userPassword);
      LoginIn(params)
        .then((res) => {
          if (res.code == 1) {
            _this.$message({
              showClose: true,
              message: "登录成功",
              type: "success",
            });
            _this.$store.commit("setloginIn", true);
            _this.$store.commit("setId", res.userMsg.id);
            _this.$store.commit("setUserName", res.userMsg.userName);
            _this.$store.commit("setuserAccount", res.userMsg.userAccount);
            _this.$store.commit("setUserImage", res.userMsg.userImage);
            setTimeout(function () {
              _this.$router.push({ path: "/" });
            }, 2000);
          } else {
            _this.$message({
              showClose: true,
              message: "账号或者密码错误",
              type: "error",
            });
          }
        })
        .catch((res) => {
          _this.$message({
            showClose: true,
            message: "登录失败",
            type: "error",
          });
        });
    },
    toRegister() {
      this.$router.push("/register");
    },
    toIndex() {
      this.$router.push("/index");
    },
    toManage() {
      this.$router.push("/managelogin");
    },
  },
};
</script>

<style scoped>
#bgImg {
  background-image: url("../assets/images/background/登录.jpg");
  background-size: 100% 100%;
  background-repeat: no-repeat;
  background-attachment: fixed;
  height: 100%;
}
.el-header {
  height: 200px;
  position: fixed;
  top: 0px;
  left: 10px;
}
.goback {
  color: #f7a7a7;
  font-size: 20px;
  padding-top: 0px;
  background-color: #ffffff00;
  border-color: #ffffff00;
}
.goback:hover {
  background-color: #ffffff00;
  border-color: #ffffff00;
}
.words {
  color: #f7a7a7;
  font-family: "楷体";
  font-size: 50px;
  margin-left: 25%;
  margin-top: 10%;
}
.logologin {
  height: 80%;
  margin-top: 10px;
  float: left;
}
.demo-loginForm {
  /* background-image: url("../assets/logo.png"); */
  background-color: #f7a7a750;
  text-align: center;
  max-width: 900px;
  margin-left: 30%;
  margin-right: 35%;
  height: 300px;
  margin-top: 5%;
  margin-bottom: 25%;
  border: lightpink;
  border-radius: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
}
.el-button {
  font-family: "楷体";
  width: 10%;
}
/deep/.shouye {
  margin-top: 10px;
  background-color: #ffffff10;
  border: lightpink;
  float: right;
}
/deep/.shouye:hover {
  /* 鼠标放上去变色 */
  background-color: #f38787c9;
}

/deep/ .el-form-item__label {
  /* 输入框字体颜色 */
  color: rgb(255, 255, 255);
  font-family: "楷体";
  font-size: large;
  margin-top: 10px;
}

/deep/.el-input__inner {
  /* 输入框椭圆 */
  border-radius: 30px;
  height: 40px;
  font-weight: initial;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}
/deep/.buttona {
  font-family: "楷体";
  border-radius: 30px;
  color: #fff;
  margin-right: 20%;
  background-color: #f7a7a7;
  border-color: lightpink;
  width: 20%;
}
/deep/.buttona:hover {
  /* 鼠标放上去变色 */
  border-color: rgb(255, 255, 255);
  background-color: #f38787e5;
}
/deep/.el-form-item__content {
  margin-right: 50px;
}
/deep/.el-form-item__content {
  margin-top: 10px;
}
</style>

