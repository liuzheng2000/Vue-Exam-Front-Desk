// 我的考试页面
<template>
  <div id="myExam">
    <div class="title">查询教师</div>
    <div class="wrapper">
      <ul class="top">
      </ul>
      <ul class="paper" v-loading="loading">
        <li
          class="item"
          v-for="(item, index) in pagination"
          :key="index"
        >
          <h4 >{{ item.teacherName}}</h4>
          <span class="name">学院：{{ item.institute }} </span>
          <div class="info">
          <p>性别：{{ item.sex }}<span>   电话：{{ item.tel }}</span></p> 
          <p>Email{{ item.email }}<span>   职称：{{ item.type }}</span></p>
          <p>已加入导师</p>
          </div>
          
        </li>
      </ul>
    </div>


    <div class="wrapper">
      <ul class="top">
      </ul>
      <ul class="paper" v-loading="loading">
        <li
          class="item"
          v-for="(item, index) in NotTeacherIsStudent"
          :key="index"
        >
          <h4 >{{ item.teacherName}}    </h4>
          <span class="name">学院：{{ item.institute }} </span>
          <div class="info">
          <p>性别：{{ item.sex }}<span>   电话：{{ item.tel }}</span></p> 
          <p>Email{{ item.email }}<span>   职称：{{ item.type }}</span></p> 
          <el-button @click="stuApplyTeacher(item.teacherId)" type="primary" size="small">加入导师</el-button>
          </div>
        </li>
      </ul>
    </div>


  </div>
</template>

<script>
export default {
  // name: 'myExam'
  data() {
    return {
      loading: false,
      pagination: {
      },
      NotTeacherIsStudent: {
      },
    };
  },
  created() {
    this.getTeacherInfo();
    this.getIsNotTeacherInfo();
    this.loading = true;
  },
  // watch: {

  // },
  methods: {
    getTeacherInfo() {
      console.log("查詢操作-------");
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") },  //设置的请求头
        url: `/api/ExamTeacher/findTeacherByTeacherAndStudent/${this.$cookies.get("cid")}`,
        method: "Get",
      })
        .then((res) => {
          this.pagination = res.data.data;
          this.loading = false;
          console.log(this.pagination);
        })
        .catch((error) => {
          console.log(error);
        });
    },

      getIsNotTeacherInfo() {
      console.log("查詢操作-------");
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") },  //设置的请求头
        url: `/api/ExamTeacher/findTeacherNotIsStudentByTeacherAndStudent/${this.$cookies.get("cid")}`,
        method: "Get",
      })
        .then((res) => {
          this.NotTeacherIsStudent = res.data.data;
          this.loading = false;
          console.log(this.NotTeacherIsStudent);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    stuApplyTeacher(teacherId){
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") },  //设置的请求头
        url: `/api/ExamTeacher/stuApplyTeacher`,
        method: "Post",
        data:{
          StudentID: this.$cookies.get("cid"),
          teacherID: teacherId
        }
      })
        .then((res) => {
           console.log(res.data.data);
                   this.$alert(
            res.data.data,
            "注意",
            {
              confirmButtonText: "确定",
            }
          );
        })
        .catch((error) => {
          console.log(error);
        });
    }

  }
};
</script>


<style lang="scss" scoped>
.pagination {
  padding: 20px 0px 30px 0px;
  .el-pagination {
    display: flex;
    justify-content: center;
  }
}
.paper {
  h4 {
    cursor: pointer;
  }
}
.paper .item a {
  color: #000;
}
.wrapper .top .order {
  cursor: pointer;
}
.wrapper .top .order:hover {
  color: #0195ff;
  border-bottom: 2px solid #0195ff;
}
.wrapper .top .order:visited {
  color: #0195ff;
  border-bottom: 2px solid #0195ff;
}
.item .info i {
  margin-right: 5px;
  color: #0195ff;
}
.item .info span {
  margin-right: 14px;
}
.paper .item {
  width: 380px;
  border-radius: 4px;
  padding: 20px 30px;
  border: 1px solid #eee;
  box-shadow: 0 0 4px 2px rgba(217, 222, 234, 0.3);
  transition: all 0.6s ease;
}
.paper .item:hover {
  box-shadow: 0 0 4px 2px rgba(140, 193, 248, 0.45);
  transform: scale(1.03);
}
.paper .item .info {
  font-size: 14px;
  color: #88949b;
}
.paper .item .name {
  font-size: 14px;
  color: #88949b;
}
.paper * {
  margin: 20px 0;
}
.wrapper .paper {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.top .el-icon-search {
  position: absolute;
  right: 10px;
  top: 10px;
}
.top .icon {
  position: relative;
}
.wrapper .top {
  border-bottom: 1px solid #eee;
  margin-bottom: 20px;
}
#myExam .search-li {
  margin-left: auto;
}
.top .search-li {
  margin-left: auto;
}
.top li {
  display: flex;
  align-items: center;
}
.top .search {
  margin-left: auto;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #eee;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
}
.top .search:hover {
  color: #0195ff;
  border-color: #0195ff;
}
.wrapper .top {
  display: flex;
}
.wrapper .top li {
  margin: 20px;
}
#myExam {
  width: 980px;
  margin: 0 auto;
}
#myExam .title {
  margin: 20px;
}
#myExam .wrapper {
  background-color: #fff;
}
.wrapper .top .order {
  cursor: pointer;
}
.wrapper .top .order:hover {
  color: #0195ff;
  border-bottom: 2px solid #0195ff;
}
.wrapper .top .order:visited {
  color: #0195ff;
  border-bottom: 2px solid #0195ff;
}
.item .info i {
  margin-right: 5px;
  color: #0195ff;
}
.item .info span {
  margin-right: 14px;
}
.paper .item {
  border-radius: 4px;
  padding: 20px 30px;
  border: 1px solid #eee;
  box-shadow: 0 0 4px 2px rgba(217, 222, 234, 0.3);
  transition: all 0.6s ease;
}
.paper .item:hover {
  box-shadow: 0 0 4px 2px rgba(140, 193, 248, 0.45);
}
.paper .item .info {
  font-size: 14px;
  color: #88949b;
}
.paper .item .name {
  font-size: 14px;
  color: #88949b;
}
.paper * {
  margin: 20px 0;
}
.wrapper .paper {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.top .el-icon-search {
  position: absolute;
  right: 10px;
  top: 10px;
}
.top .icon {
  position: relative;
}
.wrapper .top {
  border-bottom: 1px solid #eee;
}
#myExam .search-li {
  margin-left: auto;
}
.top .search-li {
  margin-left: auto;
}
.top li {
  display: flex;
  align-items: center;
}
.top .search {
  margin-left: auto;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #eee;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
}
.top .search:hover {
  color: #0195ff;
  border-color: #0195ff;
}
.wrapper .top {
  display: flex;
}
.wrapper .top li {
  margin: 20px;
}
#myExam {
  width: 980px;
  margin: 0 auto;
}
#myExam .title {
  margin: 20px;
}
#myExam .wrapper {
  background-color: #fff;
}
</style>
