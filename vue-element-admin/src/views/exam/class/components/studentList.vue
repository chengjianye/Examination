<template>
  <div>
    <el-table
        :data="studentData"
        style="width: 100%">
        <el-table-column
            prop="student_name"
            label="姓名">
        </el-table-column>
        <el-table-column
            prop="student_id"
            label="学号">
        </el-table-column>
        <el-table-column
            prop="grade_name"
            label="班级">
        </el-table-column>
        <el-table-column
            prop="room_text"
            label="教室">
        </el-table-column>
        <el-table-column
            prop="student_pwd"
            label="密码">
        </el-table-column>
        <el-table-column
            fixed="right"
            label="操作"
            width="100" style="colo:#000">
        <template slot-scope="scope">
            <el-button type="text" size="small" style="color:#606266" @click="curDelete(scope.$index,scope.row,studentData)">删除</el-button>
        </template>
        </el-table-column>
        </el-table>
        <div class="block">
            <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-sizes="pageSizes"
            :page-size="pageNum"
            layout=" prev, pager, next,sizes,jumper"
            :total="len"
            >
        </el-pagination>
        </div>
  </div>
</template>

<script>

import {mapState,mapMutations,mapActions} from 'vuex';
export default {
  name: 'studentList',
  data(){
     return {
        data:[],
        currentPages:1,
        pageSizes:[5, 10, 20, 50,100],
        pageSize:20,
        pageTotal:553
     }
  },
  computed:{
     ...mapState({
       studentData:state => state.student.studentData,
       student1Data:state => state.student.student1Data,
       pageNum:state => state.student.pageNum,
       currentPage:state => state.student.currentPage,
       len:state => state.student.len
    })
   },
    methods:{
       ...mapMutations({
        updatePage:'student/updatePage',
        pageData:'student/pageData',
        pageDatas:'student/pageDatas'
       }),
       ...mapActions({
        curUpDateStudent:'student/curUpDateStudent',
        getCurAllRoom:'classManage/getCurAllRoom',
        curDeleteStudent:'student/curDeleteStudent',
        curUpDateClass:'classManage/curUpDateClass'
       }),
    getRoom(e){
       this.form.room = e ;
    },
    getClass(e){
       this.form.class = e ;
    },
    async curDelete(ind,row,rows){
      await this.curDeleteStudent({
        student_id:row.student_id
      })
      rows.splice(ind, 1);
      this.getPage()
      
    },
    
    async getPage(){
        this.updatePage({
          pageSize:this.pageSize,
          currentPage:this.currentPages
        })
        
        let res = await this.curUpDateStudent()
        if(this.student1Data.length > 0){
          this.data = this.student1Data.slice((this.currentPages-1) * this.pageSize,this.currentPages * this.pageSize)         
          this.pageData(this.data)
        }else{
          this.data = res.slice((this.currentPages-1) * this.pageSize,this.currentPages * this.pageSize)
          this.pageData(this.data)
        }
    },
    handleSizeChange(val) {
      this.pageSize = val
      this.getPage()
    },
    handleCurrentChange(val) {
        this.currentPages = val
        this.getPage()
    }
   }
}
</script>

<style lang="scss">
    .el-pagination{
       text-align: right;
       margin-top: 10px;
    }
</style>
