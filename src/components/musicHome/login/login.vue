<template>
  <div class="login">
    <div class="img-wrap">
      <div class="img-con">Welcome<br />Please Login</div>
      <div class="form-con">
        <!-- 关闭键 -->
        <i
          class="el-icon-close"
          style="
        position: absolute;
        right: 5px;
        top: 5px;
        font-size: 25px;
        color: #b5b4b4;
        cursor: pointer;
      "
          @click="changeShowLogin(false)"
        ></i>
        <el-form
          ref="loginForm"
          :model="loginForm"
          :rules="rules"
          label-width="70px"
          class="demo-ruleForm"
          labelPosition="left"
          @keyup.enter.native="login()"
        >
          <el-form-item label="手机号" prop="phone">
            <el-input
              v-model="loginForm.phone"
              placeholder="请输入手机号"
            ></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input
              v-model="loginForm.password"
              placeholder="请输入密码"
              show-password
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="login()">登录</el-button>
            <el-button>注册</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  components: {},
  data() {
    return {
      loginForm: {
        phone: "",
        password: "",
      },
      //表单提交规则
      rules: {
        phone: [{ required: true, message: "请输入手机号", trigger: "blur" }],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 8, max: 16, message: "长度在 8 到 16 个字符", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    //点击登录
    async login() {
      await this.$http
        .get("login/cellphone", { params: this.loginForm})
        .then((res) => {
          console.log(res,"res");
          if (res.status !== 200){
            return this.$message.error("登陆失败,请检查登录信息!");
          }
          console.log("输出");
          this.$message.success("登陆成功");
          this.$store.dispatch("saveUserInfo", res.data.profile);
          this.getUserPrivatePlayList(res.data.profile.userId);
          this.$emit("showLogin", false);
        });
    },
    //查询用户私人歌单
    getUserPrivatePlayList(userId) {
      this.$http
        .get("user/playlist", { params: { uid: userId } })
        .then((res) => {
          // console.log(res.data);
          this.$store.dispatch("savePersonalList", res.data.playlist);
        });
    },
    //打开登录窗口
    changeShowLogin(isShow) {
      this.$store.dispatch("changeShowLogin", isShow);
    },
  },
};
</script>

<style>
/* .login {
  z-index: 500;
  position: fixed;
  width: 450px;
  height: 500px;
  left: 50%;
  top: 10%;
  background: #fff;
  box-shadow: #cecccc 0px 0px 2px 2px;
  transform: translate(-50%);
} */

.login {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(0, 0, 0, 0.6);
  width: 100%;
  height: 100%;
  z-index: 500;
}

.login .img-wrap {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  background: url("https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1605520228908&di=a3cef04f54954aa8ae02374fc35cc804&imgtype=0&src=http%3A%2F%2Fimg.pconline.com.cn%2Fimages%2Fupload%2Fupc%2Ftx%2Fitbbs%2F1011%2F23%2Fc0%2F5963394_1290487925056_1024x1024.jpg")
    no-repeat center left;
  width: 650px;
  height: 350px;
  box-shadow: rgb(141, 140, 140) 0 0 5px 0px;
  /* margin: 100px 180px; */
  overflow: hidden;
  z-index: 501;
}
.login .img-wrap .img-con {
  flex: 1;
  margin-top: 30px;
  margin-left: 30px;
  font-size: 40px;
  line-height: 40px;
  font-weight: 900;
  color: #444;
  animation: lightSpeedInLeft 1.5s linear;
}
.login .form-con {
  flex: 1;
  width: 450px;
  display: flex;
  justify-content: center;
  align-items: center;
  /* margin: 20px auto; */
  background-color: #fff;
  padding: 20px;
  animation: fadeInRight 0.8s linear;
}
.login .el-button--primary {
  background-color: #fccfd8;
  border-color: #fccfd8;
  transition: all 0.3s;
}
.login .el-button--primary:hover {
  background-color: #efa5b1;
  border-color: #efa5b1;
}
.login .el-button--default:hover {
  background-color: #efa5b1;
  border-color: #efa5b1;
  color: white;
}
.login .el-radio__input.is-checked .el-radio__inner {
  background-color: #efa5b1;
  border-color: #efa5b1;
}
.login .el-radio__input.is-checked + .el-radio__label {
  color: #efa5b1;
}
.login .el-form-item:last-child {
  margin-bottom: 0;
}

/* 分界 */
.login .logForm {
  position: absolute;
  width: 350px;
  height: 150px;
  left: 10%;
  top: 53%;
}
.login .logBtn {
  position: absolute;
  width: 70%;
  padding: 10px 15px;
  background: #ea4848;
  text-align: center;
  color: #fff;
  cursor: pointer;
  border-radius: 20px;
  top: 78%;
  left: 50%;
  transform: translateX(-50%);
}
.login .logBtn:hover {
  background: #c72e2e;
}
</style>
