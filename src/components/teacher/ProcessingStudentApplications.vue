//查询所有考试
<template>
  <div class="exam">
    <el-table id="examInfo" :data="pagination.records" border>
      <el-table-column
        prop="StudentID"
        label="申请人"
        width="180"
      ></el-table-column>

      <el-table-column label="操作" width="250">
        <template slot-scope="scope">
          <el-button
            @click="successApply(scope.row.StudentID)"
            type="primary"
            size="small"
            >同意</el-button
          >
          <el-button
            @click="deleteApplyRecord(scope.row.StudentID)"
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
  </div>
</template>

<script>
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
    this.getApplyInfo();
  },
  methods: {
    getApplyInfo() {
      //分页查询所有试卷信息
      this.$axios(
        {
          headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
          url: `/api/ExamTeacher/getStudentApplyTeacher`,
          method: "Post",
          data:{
              teacherId:this.$cookies.get("cid"),
              page: this.pagination.current,
              size: this.pagination.size
          }
        }
      )
        .then((res) => {
        this.pagination = res.data.data;
        console.log(res.data.data);
        })
        .catch((error) => {});
    },

    deleteApplyRecord(StudentID){
            this.$axios(
        {
          headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
          url: `/api/ExamTeacher/deleteApplyRecord`,
          method: "Post",
          data:{
              TeacherID:this.$cookies.get("cid"),
              StudentID:StudentID
          }
        }
      )
    .then((res) => {
     this.getApplyInfo();
    console.log(res.data.data);
    })
    .catch((error) => {});
    },

    successApply(StudentID){
            this.$axios(
        {
          headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
          url: `/api/ExamTeacher/successApply`,
          method: "Post",
          data:{
              TeacherID:this.$cookies.get("cid"),
              StudentID:StudentID
          }
        }
      )
    .then((res) => {
     this.getApplyInfo();
    console.log(res.data.data);
    })
    .catch((error) => {});
    },


    //改变当前记录条数
    handleSizeChange(val) {
      this.pagination.size = val;
      this.getApplyInfo();
    },
    //改变当前页码，重新发送请求
    handleCurrentChange(val) {
      this.pagination.current = val;
      this.getApplyInfo();
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
