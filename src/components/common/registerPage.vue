<!-- 页面修改完成 -->
<!-- 用户登录界面 -->
<template>
  <div id="login">
    <div class="bg"></div>
    <el-row class="main-container">
      <el-col :lg="8" :xs="16" :md="10" :span="10">
        <div class="top">
          <i class="iconfont icon-kaoshi"></i
          ><span class="title">在线考试系统</span>
        </div>
        <div class="bottom">
          
          <div class="container">
            <p class="title">账号注册</p> 
            <el-form
              :label-position="labelPosition"
              label-width="80px"
              :model="formLabelAlign"
            >
              <el-form-item label="用戶類型">
                <el-select v-model="formLabelAlign.type" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  >
                  </el-option>
                </el-select>
              </el-form-item>
              <span v-if="formLabelAlign.type == '2'">
                <el-form-item label="电话">
                  <el-input
                    v-model.number="studentRegister.tel"
                    placeholder="请输入电话后获取验证码"
                  ></el-input>
                </el-form-item>

                <el-form-item label="验证码">
                  <el-input
                    v-model.number="studentRegister.verification"
                    placeholder="请输入电话后获取验证码"
                  ></el-input>
                </el-form-item>

                <el-form-item label="昵称">
                  <el-input
                    v-model.number="studentRegister.studentName"
                    placeholder="请输入昵称"
                  ></el-input>
                </el-form-item>

                <el-form-item label="入学年份">
                  <el-input
                    v-model="studentRegister.grade"
                    placeholder="请输入入学年份"
                  ></el-input>
                </el-form-item>

                <el-form-item label="专业">
                  <el-input
                    v-model="studentRegister.major"
                    placeholder="请输入专业"
                  ></el-input>
                </el-form-item>

                <el-form-item label="班级">
                  <el-input
                    v-model="studentRegister.clazz"
                    placeholder="请输入班级"
                  ></el-input>
                </el-form-item>

                <el-form-item label="学院">
                  <el-input
                    v-model="studentRegister.institute"
                    placeholder="请输入学院"
                  ></el-input>
                </el-form-item>

                <el-form-item label="email">
                  <el-input
                    v-model="studentRegister.email"
                    placeholder="请输入email"
                  ></el-input>
                </el-form-item>

                <el-form-item label="密码">
                  <el-input
                    v-model="studentRegister.pwd"
                    placeholder="请输入密码"
                    type="password"
                  ></el-input>
                </el-form-item>

                <el-form-item label="身份证">
                  <el-input
                    v-model="studentRegister.cardId"
                    placeholder="请输入身份证"
                  ></el-input>
                </el-form-item>

                <el-form-item label="性别">
                  <el-input
                    v-model="studentRegister.sex"
                    placeholder="请输入性别"
                  ></el-input>
                </el-form-item>

                <div class="submit">
                  <el-button
                    type="primary"
                    class="row-login"
                    @click="StudentRegister()"
                    >学生注册</el-button
                  >
                </div>
                <div class="submit">
                  <el-button
                    type="primary"
                    class="row-login"
                    @click="StudentVerification()"
                    >学生验证码</el-button
                  >
                </div>
              </span>

              <span v-if="formLabelAlign.type == '1'">
                <el-form-item label="电话">
                  <el-input
                    v-model.number="teacherRegister.tel"
                    placeholder="请输入电话后获取验证码"
                  ></el-input>
                </el-form-item>

                <el-form-item label="验证码">
                  <el-input
                    v-model.number="teacherRegister.verification"
                    placeholder="请输入电话后获取验证码"
                  ></el-input>
                </el-form-item>

                <el-form-item label="昵称">
                  <el-input
                    v-model="teacherRegister.teacherName"
                    placeholder="请输入昵称"
                  ></el-input>
                </el-form-item>

                <el-form-item label="学院">
                  <el-input
                    v-model="teacherRegister.institute"
                    placeholder="请输入学院"
                  ></el-input>
                </el-form-item>

                <el-form-item label="email">
                  <el-input
                    v-model="teacherRegister.email"
                    placeholder="请输入email"
                  ></el-input>
                </el-form-item>

                <el-form-item label="密码">
                  <el-input
                    v-model="teacherRegister.pwd"
                    placeholder="请输入密码"
                    type="password"
                  ></el-input>
                </el-form-item>

                <el-form-item label="身份证">
                  <el-input
                    v-model="teacherRegister.cardId"
                    placeholder="请输入身份证"
                  ></el-input>
                </el-form-item>

                <el-form-item label="性别">
                  <el-input
                    v-model="teacherRegister.sex"
                    placeholder="请输入性别"
                  ></el-input>
                </el-form-item>

                <el-form-item label="职称">
                  <el-input
                    v-model="teacherRegister.type"
                    placeholder="请输入职称"
                  ></el-input>
                </el-form-item>

                <div class="submit">
                  <el-button
                    type="primary"
                    class="row-login"
                    @click="SubTeacherRegister()"
                    >教师注册</el-button
                  >
                </div>
                <div class="submit">
                  <el-button
                    type="primary"
                    class="row-login"
                    @click="teacherVerification()"
                    >教师验证码</el-button
                  >
                </div>
              </span>
            </el-form>
            <el-button @click="getBack()" type="primary" size="small" >返回</el-button>
          </div>
        </div>
      </el-col>
    </el-row>
    <el-row class="footer">
      <el-col>
        <p class="msg2">
          版权所有 ©2022 保留所有权利
          <a href="https://qingyun.run/" target="_blank">qingyun.run</a>
        </p>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import store from "@/vuex/store";
import { mapState } from "vuex";
import { encrypt } from "../../utils/jsencrypt";
export default {
  data() {
    return {
      role: 2,
      labelPosition: "left",
      options: [
        {
          value: "1",
          label: "教師",
        },
        {
          value: "2",
          label: "學生",
        },
      ],
      value: "2",
      formLabelAlign: {
        type: "2",
      },

      studentRegister: {
        //填空题提交内容
        studentName: "", //试卷名称
        grade: "", //难度等级选中值
        major: "", //正确答案
        clazz: "", //对应章节
        institute: "", //题目
        tel: "", //解析
        pwd: "",
        cardId: "",
        sex: "",
        verification: "",
      },

      teacherRegister: {
        //填空题提交内容
        teacherName: "", //教师姓名
        institute: "", //学院
        sex: "", //性别
        tel: "", //电话
        email: "", //邮箱
        pwd: "", //密码
        cardId: "", //身份证
        type: "", //职称
        verification: "", //验证码
      },
    };
  },
  methods: {
    getBack() {
    this.$router.push({ path: "/" });
    },
    teacherVerification() {
      // console.log("验证码为" + studentRegister.tel);
      this.$axios({
        url: "/api/auth/teacherGetVerification",
        method: "Post",
        data: {
          tel: this.teacherRegister.tel,
        },
      })
        .then((res) => {
          if (res.data.code == 200) {
            this.$message({
              //成功修改提示
              message: "验证码为" + res.data.data,
              type: "success",
            });
          } else {
            this.$message({
              //成功修改提示
              message: "验证码获取失败",
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },

    SubTeacherRegister() {
      this.$axios({
        url: "/api/auth/teacherRegister",
        method: "Post",
        data: {
          teacherName: this.teacherRegister.teacherName,
          institute: this.teacherRegister.institute,
          tel: this.teacherRegister.tel,
          pwd: encrypt(this.teacherRegister.pwd),
          cardId: this.teacherRegister.cardId,
          sex: this.teacherRegister.sex,
          email: this.teacherRegister.email,
          type: this.teacherRegister.type,
          verification: this.teacherRegister.verification,
        },
      })
        .then((res) => {
          if (res.data.code == 200) {
              this.$router.push({path: '/'})
              this.$message({
              //成功修改提示
              message: "注册成功,你的账号ID为" + res.data.data,
              type: "success",
            });
          } else {
            this.$message({
              //成功修改提示
              message: res.data.message,
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },

    StudentVerification() {
      // console.log("验证码为" + studentRegister.tel);
      this.$axios({
        url: "/api/auth/StudentGetVerification",
        method: "Post",
        data: {
          tel: this.studentRegister.tel,
        },
      })
        .then((res) => {
          if (res.data.code == 200) {
            this.$message({
              //成功修改提示
              message: "验证码为" + res.data.data,
              type: "success",
            });
          } else {
            this.$message({
              //成功修改提示
              message: "验证码获取失败",
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },

    StudentRegister() {
      this.$axios({
        url: "/api/auth/StudentRegister",
        method: "Post",
        data: {
          studentName: this.studentRegister.studentName,
          grade: this.studentRegister.grade,
          major: this.studentRegister.major,
          clazz: this.studentRegister.clazz,
          institute: this.studentRegister.institute,
          tel: this.studentRegister.tel,
          pwd: encrypt(this.studentRegister.pwd),
          cardId: this.studentRegister.cardId,
          sex: this.studentRegister.sex,
          email: this.studentRegister.email,
          verification: this.studentRegister.verification,
        },
      })
        .then((res) => {
          if (res.data.code == 200) {
              this.$router.push({path: '/'})
              this.$message({
              //成功修改提示
              message: "注册成功,你的账号ID为" + res.data.data,
              type: "success",
            });
          } else {
            this.$message({
              //成功修改提示
              message: res.data.message,
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },
  },
};
</script>

<style lang="scss" scoped>
.remind {
  border-radius: 4px;
  padding: 10px 20px;
  display: flex;
  position: fixed;
  right: 20px;
  bottom: 50%;
  flex-direction: column;
  color: #606266;
  background-color: #fff;
  border-left: 4px solid #409eff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
.container {
  margin-bottom: 32px;
}
.container .el-radio-group {
  margin: 30px 0px;
}
a:link {
  color: #ff962a;
  text-decoration: none;
}
#login {
  font-size: 14px;
  color: #000;
  background-color: #fff;
}
#login .bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  overflow-y: auto;
  height: 100%;
  background: url("../../assets/img/loginbg.png") center top / cover no-repeat;
  background-color: #b6bccdd1 !important;
}
#login .main-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
#login .main-container .top {
  margin-top: 100px;
  font-size: 30px;
  color: #ff962a;
  display: flex;
  justify-content: center;
}
#login .top .icon-kaoshi {
  font-size: 80px;
}
#login .top .title {
  margin-top: 20px;
}
#login .bottom {
  display: flex;
  justify-content: center;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
#login .bottom .title {
  text-align: center;
  font-size: 30px;
}
.bottom .container .title {
  margin: 30px 0px;
}
.bottom .submit .row-login {
  width: 100%;
  background-color: #04468b;
  border-color: #04468b;
  margin: 20px 0px 10px 0px;
  padding: 15px 20px;
}
.bottom .submit {
  display: flex;
  justify-content: center;
}
.footer {
  margin-top: 50px;
  text-align: center;
}
.footer .msg1 {
  font-size: 18px;
  color: #fff;
  margin-bottom: 15px;
}
.footer .msg2 {
  font-size: 14px;
  color: #e3e3e3;
  margin-top: 70px;
}
.bottom .options {
  margin-bottom: 40px;
  color: #ff962a;
  display: flex;
  justify-content: space-between;
}
.bottom .options > a {
  color: #ff962a;
}
.bottom .options .register span:nth-child(1) {
  color: #8c8c8c;
}
</style>
