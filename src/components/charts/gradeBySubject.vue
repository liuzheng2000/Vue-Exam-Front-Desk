
<template>

  <div class="all">
    <el-table  :data="pagination.records" border  id="subjectScore">
      <el-table-column fixed="left" prop="subjectName" label="科目" width="400"></el-table-column>
      <el-table-column prop="studentName" label="姓名" width="380"></el-table-column>
      <el-table-column prop="score" label="期末成绩" width="380"></el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagination.current"
      :page-sizes="[6, 10]"
      :page-size="pagination.size"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagination.total"
      class="page"
    ></el-pagination>
    <el-button @click="getBack()" type="primary" size="small">返回</el-button>
      <el-button @click="exportExcel()" type="primary" size="small"
      >导出
    </el-button>
  </div>

</template>

<script>
import * as XLSX from "xlsx/xlsx.mjs";
export default {
  data() {
    return {
      pagination: {
        //分页后的考试信息
        current: 1, //当前页
        total: null, //记录条数
        size: 6 //每页条数
      }
    };
  },
  created() {
    this.getStudentScoreByTeacher();
  },
  methods: {
  exportExcel() {
      // Acquire Data (reference to the HTML table)
      // var table_elt = document.getElementById("examInfo");
      // Extract Data (create a workbook object from the table)
      // var workbook = XLSX.utils.table_to_book(table_elt);
      var workbook = XLSX.utils.book_new();
      // Process Data (add a new row)
      var ws = XLSX.utils.table_to_sheet(
        document.getElementById("subjectScore")
      );
      // XLSX.utils.sheet_add_aoa(ws, [["Created " + new Date().toISOString()]], {
      //   origin: -1,
      // });
      XLSX.utils.book_append_sheet(workbook, ws, "Sheet1");
      // Package and Release Data (`writeFile` tries to write and save an XLSB file)
      XLSX.writeFile(workbook, "科目考试成绩明细列表.xlsb");
    },
    getStudentScoreByTeacher() {
    let subjectID = this.$route.query.examCode
    //分页查询所有试卷信息
      // this.$axios(`/api/students/${this.pagination.current}/${this.pagination.size}`).then(res => {
        this.$axios({
          // `/${this.pagination.current}/${this.pagination.size}/${studentId}`
        headers: { Authorization: this.$cookies.get("token") },  //设置的请求头
        // url: '/api/ScoreBySubjectToTeacher',
        url: '/api/ExamScore/ScoreBySubjectToTeacher',
        method: 'post',
        data: {
          current:this.pagination.current,
          size:this.pagination.size,
          subjectID:subjectID,
          teacherID:this.$cookies.get("cid")
        }}
        )  
          .then(res => {
        this.pagination = res.data.data;
      }).catch(error => {});
    },

    //改变当前记录条数
    handleSizeChange(val) {
      this.pagination.size = val;
      this.getStudentScoreByTeacher();
    },

    //改变当前页码，重新发送请求
    handleCurrentChange(val) {
      this.pagination.current = val;
      this.getStudentScoreByTeacher();
    },

    //返回上一页面
    getBack() {
    this.$router.push({ path: "/selectExamToPart" });
    }
  }
};
</script>
<style lang="scss" scoped>
.all {
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
  .el-table tr {
    background-color: #dd5862 !important;
  }
}
.el-table .warning-row {
  background: #000 !important;
}

.el-table .success-row {
  background: #dd5862;
}
</style>
