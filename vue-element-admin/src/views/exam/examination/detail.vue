<template>
  <div class="detail">
    <div class="head">试卷详情</div>
    <div class="bigBox">
      <div class="left">
        <div class="style_exam__2fYLs">
          <div class="style_questionitem">
            <div
              class="style_questionitem__1TQyr"
              v-for="(item,index) in DetailExamState"
              :key="index"
            >
              <vue-markdown class="text-input">
                {{index+1}}、{{item.title}}
                {{item.questions_type_text}}
                {{item.questions_answer}}
                {{item.questions_stem}}
              </vue-markdown>
            </div>
          </div>
        </div>
      </div>
      <div class="right"></div>
    </div>
  </div>
</template>
<script>
import { mapState, mapMutations, mapActions } from "vuex";
import VueMarkdown from "vue-markdown";

export default {
  data() {
    return {
      detailData: []
    };
  },
  components: {
    VueMarkdown
  },
  computed: {
    ...mapState({
      DetailExamState: state => {
        return state.examination.DetailExamData;
      }
    })
  },
  methods: {
    ...mapMutations({
      DetailExamSave: "examination/getDetailExam"
    }),
    ...mapActions({
      DetailExam: "examination/DetailExam"
    })
  },
  mouted() {
    //存入本地
    this.detailData = JSON.parse(window.localStorage.getItem("examID"));
  }
};
</script>

<style lang="scss" scoped>
.left /deep/ code {
  white-space: normal;
}

.left /deep/ ul li{
  list-style: none;
}
.detail {
  width: 100%;
  background: #eee;
}

.head {
  height: 80px;
  padding: 25px;
  box-sizing: border-box;
  font-size: 26px;
}

.bigBox {
  flex: auto;
  min-height: 0;
  display: flex;
  margin-left: 35px;

  .left {
    background: rgb(255, 255, 255);
    width: 68%;
    padding: 24px;
    margin: 0px 10px 20px 0px;
    border-radius: 10px;

    .style_exam__2fYLs {
      min-height: 980px;
      margin: auto;
      text-align: center;
    }
  }
  .right {
    background: rgb(255, 255, 255);
    padding: 24px;
    margin: 0px 0px 20px;
    border-radius: 10px;
    width: 30%;
    flex-shrink: 0;
  }
}
</style>
