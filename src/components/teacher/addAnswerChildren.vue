// 添加题库
<template>
  <div class="add">
    <el-tabs v-model="activeName">
      <el-tab-pane name="first">
        <span slot="label"><i class="el-icon-circle-plus"></i>添加试题</span>
        <section class="append">
          <ul>
            <li>
              <span>题目类型:</span>
              <el-select
                v-model="optionValue"
                placeholder="请选择题型"
                class="w150"
              >
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </li>
            <li v-if="optionValue == '选择题'">
              <span>所属章节：</span>
              <el-input
                placeholder="请输入对应章节"
                v-model="postChange.section"
                class="w150"
                clearable
              >
              </el-input>
            </li>
            <li v-if="optionValue == '填空题'">
              <span>所属章节：</span>
              <el-input
                placeholder="请输入对应章节"
                v-model="postFill.section"
                class="w150"
                clearable
              >
              </el-input>
            </li>
            <li v-if="optionValue == '判断题'">
              <span>所属章节：</span>
              <el-input
                placeholder="请输入对应章节"
                v-model="postJudge.section"
                class="w150"
                clearable
              >
              </el-input>
            </li>
            <li v-if="optionValue == '选择题'">
              <span>难度等级:</span>
              <el-select
                v-model="postChange.level"
                placeholder="选择难度等级"
                class="w150"
              >
                <el-option
                  v-for="item in levels"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </li>
            <li v-if="optionValue == '填空题'">
              <span>难度等级:</span>
              <el-select
                v-model="postFill.level"
                placeholder="选择难度等级"
                class="w150"
              >
                <el-option
                  v-for="item in levels"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </li>
            <li v-if="optionValue == '判断题'">
              <span>难度等级:</span>
              <el-select
                v-model="postJudge.level"
                placeholder="选择难度等级"
                class="w150"
              >
                <el-option
                  v-for="item in levels"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </li>
            <li v-if="optionValue == '选择题'">
              <span>正确选项:</span>
              <el-select
                v-model="postChange.rightAnswer"
                placeholder="选择正确答案"
                class="w150"
              >
                <el-option
                  v-for="item in rights"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </li>
          </ul>
          <!-- 选择题部分 -->
          <div class="change" v-if="optionValue == '选择题'">
            <div class="title">
              <el-tag>题目:</el-tag
              ><span
                >在下面的输入框中输入题目,形如--DNS
                服务器和DHCP服务器的作用是（）</span
              >
              <el-input
                type="textarea"
                rows="4"
                v-model="postChange.question"
                placeholder="请输入题目内容"
                resize="none"
                class="answer"
              >
              </el-input>
            </div>
            <div class="options">
              <ul>
                <li>
                  <el-tag type="success">A</el-tag>
                  <el-input
                    placeholder="请输入选项A的内容"
                    v-model="postChange.answerA"
                    clearable=""
                  >
                  </el-input>
                </li>
                <li>
                  <el-tag type="success">B</el-tag>
                  <el-input
                    placeholder="请输入选项B的内容"
                    v-model="postChange.answerB"
                    clearable=""
                  >
                  </el-input>
                </li>
                <li>
                  <el-tag type="success">C</el-tag>
                  <el-input
                    placeholder="请输入选项C的内容"
                    v-model="postChange.answerC"
                    clearable=""
                  >
                  </el-input>
                </li>
                <li>
                  <el-tag type="success">D</el-tag>
                  <el-input
                    placeholder="请输入选项D的内容"
                    v-model="postChange.answerD"
                    clearable=""
                  >
                  </el-input>
                </li>
              </ul>
            </div>
            <div class="title">
              <el-tag>解析:</el-tag><span>在下面的输入框中输入题目解析</span>
              <el-input
                type="textarea"
                rows="4"
                v-model="postChange.analysis"
                placeholder="请输入答案解析"
                resize="none"
                class="answer"
              >
              </el-input>
            </div>
            <div class="submit">
              <el-button size="small" type="primary" @click="changeSubmit()"
                >立即添加</el-button
              >
            </div>
              <el-upload
                class="upload-demo"
                ref="upload"
                action="https://jsonplaceholder.typicode.com/posts/"
                :on-preview="handlePreview"
                :on-remove="handleRemove"
                :on-success="handleSuccess"
                :file-list="fileList"
                :auto-upload="false"
                :http-request="handleUpload"
              >
                <el-button slot="trigger" size="small" type="primary"
                  >选取文件</el-button
                >
                <el-button
                  style="margin-left: 10px"
                  size="small"
                  type="success"
                  @click="submitUpload"
                  >上传到服务器</el-button
                >
              </el-upload>
          </div>
          <!-- 填空题部分 -->
          <div class="change fill" v-if="optionValue == '填空题'">
            <div class="title">
              <el-tag>题目:</el-tag
              ><span
                >输入题目,形如--从计算机网络系统组成的角度看，计算机网络可以分为()和()。注意需要考生答题部分一定要用括号（英文半角）括起来。</span
              >
              <el-input
                type="textarea"
                rows="4"
                v-model="postFill.question"
                placeholder="请输入题目内容"
                resize="none"
                class="answer"
              >
              </el-input>
            </div>
            <div class="fillAnswer">
              <el-tag>正确答案:</el-tag>
              <el-input v-model="postFill.answer"></el-input>
            </div>
            <div class="title analysis">
              <el-tag type="success">解析:</el-tag
              ><span>下方输入框中输入答案解析</span>
              <el-input
                type="textarea"
                rows="4"
                v-model="postFill.analysis"
                placeholder="请输入答案解析"
                resize="none"
                class="answer"
              >
              </el-input>
            </div>
            <div class="submit">
              <el-button type="primary" @click="fillSubmit()"
                >立即添加</el-button
              >
            </div>
              <el-upload
                class="upload-demo"
                ref="upload"
                action="https://jsonplaceholder.typicode.com/posts/"
                :on-preview="handlePreview"
                :on-remove="handleRemove"
                :on-success="handleSuccess"
                :file-list="fileList"
                :auto-upload="false"
                :http-request="handleFillQuestionUpload"
              >
                <el-button slot="trigger" size="small" type="primary"
                  >选取文件</el-button
                >
                <el-button
                  style="margin-left: 10px"
                  size="small"
                  type="success"
                  @click="submitFillQuestionUpload"
                  >上传到服务器</el-button
                >
              </el-upload>
          </div>
          <!-- 判断题 -->
          <div class="change judge" v-if="optionValue == '判断题'">
            <div class="title">
              <el-tag>题目:</el-tag><span>在下面的输入框中输入题目</span>
              <el-input
                type="textarea"
                rows="4"
                v-model="postJudge.question"
                placeholder="请输入题目内容"
                resize="none"
                class="answer"
              >
              </el-input>
            </div>
            <div class="judgeAnswer">
              <el-radio v-model="postJudge.answer" label="T">正确</el-radio>
              <el-radio v-model="postJudge.answer" label="F">错误</el-radio>
            </div>
            <div class="title">
              <el-tag>解析:</el-tag><span>在下面的输入框中输入题目解析</span>
              <el-input
                type="textarea"
                rows="4"
                v-model="postJudge.analysis"
                placeholder="请输入答案解析"
                resize="none"
                class="answer"
              >
              </el-input>
            </div>
            <div class="submit">
              <el-button type="primary" @click="judgeSubmit()"
                >立即添加</el-button
              >
            </div>
            <el-upload
                class="upload-demo"
                ref="upload"
                action="https://jsonplaceholder.typicode.com/posts/"
                :on-preview="handlePreview"
                :on-remove="handleRemove"
                :on-success="handleSuccess"
                :file-list="fileList"
                :auto-upload="false"
                :http-request="handleJudgeQuestionUpload"
              >
                <el-button slot="trigger" size="small" type="primary"
                  >选取文件</el-button
                >
                <el-button
                  style="margin-left: 10px"
                  size="small"
                  type="success"
                  @click="submitJudgeQuestionUpload"
                  >上传到服务器</el-button
                >
              </el-upload>
          </div>
        </section>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileList: [],
      changeNumber: null, //选择题出题数量
      fillNumber: null, //填空题出题数量
      judgeNumber: null, //判断题出题数量
      activeName: "first", //活动选项卡
      options: [
        //题库类型
        {
          value: "选择题",
          label: "选择题",
        },
        {
          value: "填空题",
          label: "填空题",
        },
        {
          value: "判断题",
          label: "判断题",
        },
      ],
      difficulty: [
        //试题难度
        {
          value: "简单",
          label: "简单",
        },
        {
          value: "一般",
          label: "一般",
        },
        {
          value: "困难",
          label: "困难",
        },
      ],
      difficultyValue: "简单",
      levels: [
        //难度等级
        {
          value: "1",
          label: "1",
        },
        {
          value: "2",
          label: "2",
        },
        {
          value: "3",
          label: "3",
        },
        {
          value: "4",
          label: "4",
        },
        {
          value: "5",
          label: "5",
        },
      ],
      rights: [
        //正确答案
        {
          value: "A",
          label: "A",
        },
        {
          value: "B",
          label: "B",
        },
        {
          value: "C",
          label: "C",
        },
        {
          value: "D",
          label: "D",
        },
      ],
      paperId: null,
      optionValue: "选择题", //题型选中值
      subject: "", //试卷名称用来接收路由参数
      examCode: "",
      teacherID: this.$cookies.get("cid"),
      postChange: {
        //选择题提交内容
        subject: "", //试卷名称
        level: "", //难度等级选中值
        rightAnswer: "", //正确答案选中值
        section: "", //对应章节
        question: "", //题目
        analysis: "", //解析
        answerA: "",
        answerB: "",
        answerC: "",
        answerD: "",
        teacherID: this.$cookies.get("cid"),
        examCode: this.$route.query.examCode,
      },
      postFill: {
        //填空题提交内容
        subject: "", //试卷名称
        level: "", //难度等级选中值
        answer: "", //正确答案
        section: "", //对应章节
        question: "", //题目
        analysis: "", //解析
        teacherID: this.$cookies.get("cid"),
        examCode: this.$route.query.examCode,
      },
      postJudge: {
        //判断题提交内容
        subject: "", //试卷名称
        level: "", //难度等级选中值
        answer: "", //正确答案
        section: "", //对应章节
        question: "", //题目
        analysis: "", //解析
        teacherID: this.$cookies.get("cid"),
        examCode: this.$route.query.examCode,
      },
      postPaper: {
        //考试管理表对应字段
        paperId: null,
        questionType: null, // 试卷类型 1--选择题  2--填空题   3--判断题
        questionId: null,
      },
    };
  },
  created() {
    this.getParams();
  },
  methods: {

       //文件上传设置
    submitJudgeQuestionUpload(){
      let list = document.getElementsByClassName(
        "el-upload-list__item is-ready"
      );
      if (list.length == 0) {
        this.$message({
          type: "warning",
          message: "请选择需要导入的模板！",
        });
        return;
      }
      this.$refs.upload.submit();
    },


      handleJudgeQuestionUpload(param) {
      console.log("进行上传");
      var fileObj = param.file;
      // FormData 对象
      var form = new FormData();
      // 文件对象
      form.append("file", fileObj);
      form.append("teacherID", this.$cookies.get("cid"));
      form.append("examID", this.$route.query.examCode);
      this.$axios({
        headers: {
          Authorization: this.$cookies.get("token"),
          "Content-Type": "multipart/form-data",
        }, //设置的请求头
        url: "/api/uploadQuestion/upLoadJudgeQuestionForm",
        method: "Post",
        data: form,
      })
        .then((res) => {
          if (res.data.code == 200) {
            this.handleSuccess();
          } else {
            this.$message({
              //成功修改提示
              message: "批量上传失败",
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },


    //文件上传设置
    submitFillQuestionUpload(){
      let list = document.getElementsByClassName(
        "el-upload-list__item is-ready"
      );
      if (list.length == 0) {
        this.$message({
          type: "warning",
          message: "请选择需要导入的模板！",
        });
        return;
      }
      this.$refs.upload.submit();
    },


      handleFillQuestionUpload(param) {
      console.log("进行上传");
      var fileObj = param.file;
      // FormData 对象
      var form = new FormData();
      // 文件对象
      form.append("file", fileObj);
      form.append("teacherID", this.$cookies.get("cid"));
      form.append("examID", this.$route.query.examCode);
      this.$axios({
        headers: {
          Authorization: this.$cookies.get("token"),
          "Content-Type": "multipart/form-data",
        }, //设置的请求头
        url: "/api/uploadQuestion/upLoadFillQuestionForm",
        method: "Post",
        data: form,
      })
        .then((res) => {
          if (res.data.code == 200) {
            this.handleSuccess();
          } else {
            this.$message({
              //成功修改提示
              message: "批量上传失败",
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },

    

    submitUpload() {
      let list = document.getElementsByClassName(
        "el-upload-list__item is-ready"
      );
      if (list.length == 0) {
        this.$message({
          type: "warning",
          message: "请选择需要导入的模板！",
        });
        return;
      }
      this.$refs.upload.submit();
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    handleSuccess() {
      this.$message({
        //成功修改提示
        message: "批量上传成功",
        type: "success",
      });
      console.log("上传完成");
      this.fileList = [];
      this.getExamInfo();
    },

    handleUpload(param) {
      console.log("进行上传");
      var fileObj = param.file;
      // FormData 对象
      var form = new FormData();
      // 文件对象
      form.append("file", fileObj);
      form.append("teacherID", this.$cookies.get("cid"));
      form.append("examID", this.$route.query.examCode);
      this.$axios({
        headers: {
          Authorization: this.$cookies.get("token"),
          "Content-Type": "multipart/form-data",
        }, //设置的请求头
        url: "/api/uploadQuestion/upLoadMultiQuestionForm",
        method: "Post",
        data: form,
      })
        .then((res) => {
          if (res.data.code == 200) {
            this.handleSuccess();
          } else {
            this.$message({
              //成功修改提示
              message: "批量上传失败",
              type: "error",
            });
          }
        })
        .catch((err) => {});
    },

    //测试
    // handleClick(tab, event) {
    //   console.log(tab, event);
    // },
    create() {
      this.$axios({
        // url: '/api/item',
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/CreateExam/itemByTeacherID",
        method: "post",
        data: {
          changeNumber: this.changeNumber,
          fillNumber: this.fillNumber,
          judgeNumber: this.judgeNumber,
          paperId: this.paperId,
          teacherId: this.$cookies.get("cid"),
          subject: "计算机网络", //题目数量太少，指定为计算机网络出题
        },
      }).then((res) => {
        console.log(res);
        let data = res.data;
        if (data.code == 200) {
          setTimeout(() => {
            this.$router.push({ path: "/selectAnswer" });
          }, 1000);
          this.$message({
            message: data.message,
            type: "success",
          });
        } else if (data.code == 400) {
          this.$message({
            message: data.message,
            type: "error",
          });
        }
      });
    },
    getParams() {
      let subject = this.$route.query.subject; //获取试卷名称
      let paperId = this.$route.query.paperId; //获取paperId
      let examCode = this.$route.query.examCode; //获取paperId
      this.paperId = paperId;
      this.subject = subject;
      this.postPaper.paperId = paperId;
      this.examCode = examCode;
    },
    changeSubmit() {
      //选择题题库提交
      this.postChange.subject = this.subject;
      this.postChange.teacherID = this.teacherID;
      this.postChange.examCode = this.examCode;
      this.$axios({
        //提交数据到选择题题库表
        // url: '/api/MultiQuestion',
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/uploadQuestion/MultiQuestionByTeacherID",
        method: "post",
        data: {
          ...this.postChange,
        },
      })
        .then((res) => {
          //添加成功显示提示
          let status = res.data.code;
          if (status == 200) {
            this.$message({
              message: "已添加到题库",
              type: "success",
            });
            this.postChange = {};
            (this.postChange.teacherID = this.$cookies.get("cid")),
              (this.postChange.examCode = this.$route.query.examCode);
          }
        })
        .then(() => {
          this.$axios(
            {
              headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
              url: `/api/uploadQuestion/multiQuestionId`,
              method: "Get",
            }
            // `/api/multiQuestionId`
          ).then((res) => {
            //获取当前题目的questionId
            let questionId = res.data.data.questionId;
            this.postPaper.questionId = questionId;
            this.postPaper.questionType = 1;
            this.$axios({
              headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
              url: "/api/ExamPaper/paperManage",
              method: "Post",
              data: {
                ...this.postPaper,
              },
            });
          });
        });
    },
    fillSubmit() {
      //填空题提交
      this.postFill.subject = this.subject;
      this.postFill.teacherID = this.teacherID;
      this.postFill.examCode = this.examCode;
      this.$axios({
        //  url: '/api/fillQuestion',
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/uploadQuestion/fillQuestionByTeacherID",
        method: "post",
        data: {
          ...this.postFill,
        },
      })
        .then((res) => {
          let status = res.data.code;
          if (status == 200) {
            this.$message({
              message: "已添加到题库",
              type: "success",
            });
            this.postFill = {};
          }
        })
        .then(() => {
          this.$axios(
            {
              headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
              url: `/api/uploadQuestion/fillQuestionId`,
              method: "Get",
            }
            // `/api/fillQuestionId`
          ).then((res) => {
            //获取当前题目的questionId
            let questionId = res.data.data.questionId;
            this.postPaper.questionId = questionId;
            this.postPaper.questionType = 2;
            this.$axios({
              headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
              url: "/api/ExamPaper/paperManage",
              method: "Post",
              data: {
                ...this.postPaper,
              },
            });
          });
        });
    },
    judgeSubmit() {
      //判断题提交
      this.postJudge.subject = this.subject;
      this.postJudge.teacherID = this.teacherID;
      this.postJudge.examCode = this.examCode;
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/uploadQuestion/judgeQuestionByTeacherID",
        // url: '/api/judgeQuestion',
        method: "post",
        data: {
          ...this.postJudge,
        },
      })
        .then((res) => {
          let status = res.data.code;
          if (status == 200) {
            this.$message({
              message: "已添加到题库",
              type: "success",
            });
            this.postJudge = {};
            (this.postJudge.teacherID = this.$cookies.get("cid")),
              (this.postJudge.examCode = this.$route.query.examCode);
          }
        })
        .then(() => {
          this.$axios(
            {
              headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
              url: `/api/uploadQuestion/judgeQuestionId`,
              method: "Get",
            }
            // `/api/judgeQuestionId`
          ).then((res) => {
            //获取当前题目的questionId
            let questionId = res.data.data.questionId;
            this.postPaper.questionId = questionId;
            this.postPaper.questionType = 3;
            this.$axios({
              headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
              url: "/api/ExamPaper/paperManage",
              method: "Post",
              data: {
                ...this.postPaper,
              },
            });
          });
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.add {
  margin: 0px 40px;
  .box {
    padding: 0px 20px;
    ul li {
      margin: 10px 0px;
      display: flex;
      align-items: center;
      .el-input {
        width: 6%;
      }
      .w150 {
        margin-left: 20px;
        width: 7%;
      }
    }
  }
  .el-icon-circle-plus {
    margin-right: 10px;
  }
  .icon-daoru-tianchong {
    margin-right: 10px;
  }
  .append {
    margin: 0px 20px;
    ul {
      display: flex;
      align-items: center;
      li {
        margin-right: 20px;
      }
    }
    .change {
      margin-top: 20px;
      padding: 20px 16px;
      background-color: #e7f6f6;
      border-radius: 4px;
      .title {
        padding-left: 6px;
        color: #2f4f4f;
        span:nth-child(1) {
          margin-right: 6px;
        }
        .answer {
          margin: 20px 0px 20px 8px;
        }
        .el-textarea {
          width: 98% !important;
        }
      }
      .options {
        ul {
          display: flex;
          flex-direction: column;
        }
        ul li {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 98%;
          margin: 10px 0px;
          span {
            margin-right: 20px;
          }
        }
      }
      .submit {
        display: flex;
        justify-content: center;
        align-items: center;
      }
    }
    .fill {
      .fillAnswer {
        display: flex;
        justify-content: center;
        align-items: center;
        span {
          margin-right: 6px;
        }
        .el-input {
          width: 91% !important;
        }
      }
      .analysis {
        margin-top: 20px;
        margin-left: 5px;
      }
    }
    .judge {
      .judgeAnswer {
        margin-left: 20px;
        margin-bottom: 20px;
      }
    }
    .w150 {
      width: 150px;
    }
    li:nth-child(2) {
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}
.upload-demo {
  padding: 0px 20px;
}
</style>


