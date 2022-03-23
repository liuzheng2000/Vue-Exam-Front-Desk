<template>
  <div id="login">
    <div class="bg"></div>
    <el-row class="main-container">
      <el-col :lg="8" :xs="16" :md="10" :span="10">
        <div class="top"></div>
        <div class="bottom bottom-1">
          <div class="container">
            <video ref="video" width="160" height="100" autoplay></video>
            <div class="submit">
              <el-button type="primary" @click="callCamera">开启摄像头</el-button>
            </div>
          </div>
        </div>
        <div class="bottom bottom-2">
          <div class="container">
           <canvas ref="canvas" width="160" height="100"></canvas>
            <div class="submit">
              <el-button type="primary" @click="photograph">拍照</el-button>
              <el-button type="primary" @click="uploadPhoto">上传</el-button>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
    <el-row class="footer"> </el-row>
  </div>
</template>   
<script>
export default {
  data() {
    return {
      headImgSrc:
        "https://qingyun-test.oss-cn-hangzhou.aliyuncs.com/img/image-20220308163919778.png?x-oss-process=style/qingyun",
    };
  },
  methods: {
    // 调用摄像头
    callCamera() {
      // H5调用电脑摄像头API
      navigator.mediaDevices
        .getUserMedia({
          video: true,
        })
        .then((success) => {
          // 摄像头开启成功
          this.$refs["video"].srcObject = success;
          // 实时拍照效果
          this.$refs["video"].play();
          console.log(this.$refs["video"])
        })
        .catch((error) => {
          console.error("摄像头开启失败，请检查摄像头是否可用！");
        });
    },
    // 拍照
    photograph() {
      let ctx = this.$refs["canvas"].getContext("2d");
      // 把当前视频帧内容渲染到canvas上
      ctx.drawImage(this.$refs["video"], 0, 0, 160, 100);
      // 转base64格式、图片格式转换、图片质量压缩
      let imgBase64 = this.$refs["canvas"].toDataURL("image/jpeg", 0.7); // 由字节转换为KB 判断大小
      this.headImgSrc = imgBase64;
      let str = imgBase64.replace("data:image/jpeg;base64,", "");
      let strLength = str.length;
      let fileLength = parseInt(strLength - (strLength / 8) * 2); // 图片尺寸  用于判断
      let size = (fileLength / 1024).toFixed(2);
      console.log(size); // 上传拍照信息  调用接口上传图片 .........
      console.log(this.headImgSrc);
      
    },
    // 关闭摄像头
    closeCamera() {
      if (!this.$refs["video"].srcObject) {
        this.dialogCamera = false;
        return;
      }
      let stream = this.$refs["video"].srcObject;
      let tracks = stream.getTracks();
      tracks.forEach((track) => {
        track.stop();
      });
      this.$refs["video"].srcObject = null;
    },
       uploadPhoto() { //分页查询所有试卷信息
        this.$axios(
        {
        url:  `/PhotoAPI/photo/uploadImage`,
        method: "Post",
        data:{
            id:19531,
            imageUrl: this.headImgSrc,
        }
        }          
        ).then(res => {
            console.log("图片处理完成")
      }).catch(error => {
      })
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
  bottom: 20%;
  flex-direction: column;
  color: #606266;
  background-color: #fff;
  border-left: 4px solid #409eff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
.bottom {
  margin-bottom: 80px;
  margin-top: 80px;
}
.container {
  margin-bottom: 80px;
  margin-top: 80px;
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