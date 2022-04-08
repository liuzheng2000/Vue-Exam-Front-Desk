//查询所有考试
<template>
  <div class="exam">
    <el-table id="examInfo" :data="pagination.records" border>
      <el-table-column
        prop="source"
        label="试卷名称"
        width="180"
      ></el-table-column>
      <el-table-column
        prop="description"
        label="介绍"
        width="200"
      ></el-table-column>
      <el-table-column
        prop="institute"
        label="所属学院"
        width="120"
      ></el-table-column>
      <el-table-column
        prop="major"
        label="所属专业"
        width="200"
      ></el-table-column>
      <el-table-column prop="grade" label="年级" width="100"></el-table-column>
      <el-table-column
        prop="examDate"
        label="考试日期"
        width="120"
      ></el-table-column>
      <el-table-column
        prop="totalTime"
        label="持续时间"
        width="120"
      ></el-table-column>
      <el-table-column
        prop="totalScore"
        label="总分"
        width="120"
      ></el-table-column>
      <el-table-column
        prop="type"
        label="试卷类型"
        width="120"
      ></el-table-column>
      <el-table-column
        prop="tips"
        label="考生提示"
        width="400"
      ></el-table-column>
      <el-table-column label="操作" width="250">
        <template slot-scope="scope">
          <el-button
            @click="ChangeExamChildren(scope.row.paperId, scope.row.examCode)"
            type="primary"
            size="small"
            >自动组卷</el-button
          >
          <el-button
            @click="edit(scope.row.examCode)"
            type="primary"
            size="small"
            >编辑</el-button
          >
          <el-button
            @click="deleteRecord(scope.row.examCode)"
            type="danger"
            size="small"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagination.current"
      :page-sizes="[4, 8, 10, 20]"
      :page-size="pagination.size"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagination.total"
      class="page"
    >
    </el-pagination>
    <!-- 编辑对话框-->
    <el-dialog
      title="编辑试卷信息"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose"
    >
      <section class="update">
        <el-form ref="form" :model="form" label-width="80px">
          <el-form-item label="试卷名称">
            <el-input v-model="form.source"></el-input>
          </el-form-item>
          <el-form-item label="介绍">
            <el-input v-model="form.description"></el-input>
          </el-form-item>
          <el-form-item label="所属学院">
            <el-input v-model="form.institute"></el-input>
          </el-form-item>
          <el-form-item label="所属专业">
            <el-input v-model="form.major"></el-input>
          </el-form-item>
          <el-form-item label="年级">
            <el-input v-model="form.grade"></el-input>
          </el-form-item>
          <el-form-item label="考试日期">
            <el-col :span="11">
              <el-date-picker
                type="date"
                placeholder="选择日期"
                v-model="form.examDate"
                style="width: 100%"
              ></el-date-picker>
            </el-col>
          </el-form-item>
          <el-form-item label="持续时间">
            <el-input v-model="form.totalTime"></el-input>
          </el-form-item>
          <el-form-item label="总分">
            <el-input v-model="form.totalScore"></el-input>
          </el-form-item>
          <el-form-item label="试卷类型">
            <el-input v-model="form.type"></el-input>
          </el-form-item>
          <el-form-item label="考生提示">
            <el-input type="textarea" v-model="form.tips"></el-input>
          </el-form-item>
        </el-form>
      </section>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submit()">确 定</el-button>
      </span>
    </el-dialog>
    <el-button @click="exportExcel()" type="primary" size="small"
      >导出
    </el-button>
    <el-button @click="goPrint()" type="primary" size="small">打印 </el-button>

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
      <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
      <el-button
        style="margin-left: 10px"
        size="small"
        type="success"
        @click="submitUpload"
        >上传到服务器</el-button
      >
    </el-upload>
  </div>
</template>

<script>
import printJS from "print-js";
import * as XLSX from "xlsx/xlsx.mjs";
export default {
  data() {
    return {
      form: {}, //保存点击以后当前试卷的信息
      pagination: {
        //分页后的考试信息
        current: 1, //当前页
        total: null, //记录条数
        size: 4, //每页条数
      },
      dialogVisible: false,
      fileList: [],
    };
  },
  created() {
    this.getExamInfo();
  },
  methods: {
    //文件上传设置
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
      this.$axios({
        headers: {
          Authorization: this.$cookies.get("token"),
          "Content-Type": "multipart/form-data",
        }, //设置的请求头
        url: "/api/ExamTeacher/upLoadForm",
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

    goPrint() {
      let printJSONTemp = JSON.stringify(this.pagination.records);
      console.log(printJSONTemp);
      console.log("打印");
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/ExamTeacher/printPdf",
        method: "Post",
        data: {
          JsonPdf: printJSONTemp,
        },
        responseType: "blob",
      })
        .then((res) => {
          const link = document.createElement("a");
          let blob = new Blob([res.data], { type: "application/pdf" });
          link.style.display = "none";
          link.href = URL.createObjectURL(blob);
          let num = "";
          for (let i = 0; i < 10; i++) {
            num += Math.ceil(Math.random() * 10);
          }
          link.setAttribute("download", "考试信息打印" + num);
          document.body.appendChild(link);
          link.click();
          document.body.removeChild(link);
        })
        .catch((err) => {});
    },
    exportExcel() {
      // Acquire Data (reference to the HTML table)
      // var table_elt = document.getElementById("examInfo");
      // Extract Data (create a workbook object from the table)
      // var workbook = XLSX.utils.table_to_book(table_elt);
      var workbook = XLSX.utils.book_new();
      // Process Data (add a new row)
      var ws = XLSX.utils.table_to_sheet(document.getElementById("examInfo"));
      // XLSX.utils.sheet_add_aoa(ws, [["Created " + new Date().toISOString()]], {
      //   origin: -1,
      // });
      XLSX.utils.book_append_sheet(workbook, ws, "Sheet1");
      // Package and Release Data (`writeFile` tries to write and save an XLSB file)
      XLSX.writeFile(workbook, "考试安排.xlsb");
    },

    ChangeExamChildren(paperId, examCode) {
      //自动组卷 设置
      this.$router.push({
        path: "/addExamChildren",
        query: { paperId: paperId, examCode: examCode },
      });
    },

    edit(examCode) {
      //编辑试卷
      this.dialogVisible = true;
      this.$axios(
        {
          headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
          url: `/api/Examexam/exam/${examCode}`,
          method: "Get",
        }

        // `/api/exam/${examCode}`
      ).then((res) => {
        //根据试卷id请求后台
        if (res.data.code == 200) {
          this.form = res.data.data;
        }
      });
    },

    handleClose(done) {
      //关闭提醒
      this.$confirm("确认关闭？")
        .then((_) => {
          done();
        })
        .catch((_) => {});
    },
    submit() {
      //提交修改后的试卷信息
      this.dialogVisible = false;
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/Examexam/PutExam",
        method: "Post",
        data: {
          ...this.form,
        },
      }).then((res) => {
        if (res.data.code == 200) {
          this.$message({
            //成功修改提示
            message: "更新成功",
            type: "success",
          });
        }
        this.getExamInfo();
      });
    },
    deleteRecord(examCode) {
      this.$confirm("确定删除该记录吗,该操作不可逆！！！", "删除提示", {
        confirmButtonText: "确定删除",
        cancelButtonText: "算了,留着",
        type: "danger",
      })
        .then(() => {
          //确认删除
          this.$axios({
            headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
            url: `/api/Examexam/DeExam/${examCode}`,
            method: "post",
          }).then((res) => {
            this.getExamInfo();
          });
        })
        .catch(() => {});
    },

    getExamInfo() {
      //分页查询所有试卷信息
      this.$axios(
        {
          headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
          url: `/api/Examexam/exams/${this.pagination.current}/${
            this.pagination.size
          }/${this.$cookies.get("cid")}`,
          method: "Get",
        }
        // `/api/exams/${this.pagination.current}/${this.pagination.size}/${this.$cookies.get("cid")}`
      )
        .then((res) => {
          this.pagination = res.data.data;
        })
        .catch((error) => {});
    },

    //改变当前记录条数
    handleSizeChange(val) {
      this.pagination.size = val;
      this.getExamInfo();
    },
    //改变当前页码，重新发送请求
    handleCurrentChange(val) {
      this.pagination.current = val;
      this.getExamInfo();
    },
  },
};
</script>
<style lang="scss" scoped>
.exam {
  padding: 0px 40px;
  .page {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .edit {
    margin-left: 20px;
  }
}
.upload-demo {
  padding: 20px 0px;
}
</style>
