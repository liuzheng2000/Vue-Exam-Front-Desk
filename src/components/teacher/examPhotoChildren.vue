<!-- 页面修改完成 -->
// 考试图片页面
<template>
  <section class="add">
    <div id="myExam">
      <div class="wrapper">
      <ul class="top">
        <li class="order">人员列表</li>
        <li class="search-li">
        </li>
        <li>
        </li>
      </ul>
      <ul class="paper" v-loading="loading">
        <li
          class="item"
          v-for="(item, index) in ReturnExamPhotoVo"
          :key="index"
        >
          <h4>{{ item.institute }} {{ item.major }} {{ item.studentName }} </h4>
          <span class="name"><img :src="item.imgUrlOss" >
          <span>时间: {{ item.createTime }}</span>
          </span>
          <div class="info">
            <span>学生学号: {{ item.studentID }}</span>
            <span>电话: {{ item.tel }}</span>
            <!-- <span>跳出次数: {{item.count}}</span> -->
          </div>
        </li>
      </ul>
         <el-button type="primary" @click="getBack()" >返回</el-button>
    </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      examId: this.$route.query.examCode,
      IsExamOrPractice: true,
      loading: false,
      key: null, //搜索关键字
      allExam: null, //所有考试信息
      pagination: {
        //分页后的考试信息
        current: 1, //当前页
        total: null, //记录条数
        size: 6, //每页条数
      },
      ReturnExamPhotoVo: {
        studentID: null,
        imgUrlOss: null,
        studentName: null,
        major:null,
        institute:null,
        tel:null,
        createTime: null,
        count:0
      },
    };
  },
  created() {
    this.selectPhotoByExamIDAndTeacher();
  },

  methods: {
    getBack() {
    this.$router.push({ path: "/examPhoto" });
    },
    selectPhotoByExamIDAndTeacher() {
      this.$axios({
        headers: { Authorization: this.$cookies.get("token") }, //设置的请求头
        url: "/api/examPhoto/byExamPhotoVo",
        method: "post",
        data: {
          examId: this.$route.query.examCode,
        },
      }).then((res) => {
          this.ReturnExamPhotoVo=res.data.data
      }).catch((error) =>{
          console.log("获取远程图片失败");
      })
    },
  },
};
</script>

<style lang="scss" scoped>
.add {
  padding: 0px 40px;
  width: 400px;
}
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
</style>
