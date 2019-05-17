<template>
  <div class="main">
    <!-- 热门话题 -->
    <div class="hot-topic bra">
      <div class="hot-topic-title flex">
        <text class="f28 fw5 c0">热门话题</text>
        <text class="f24 c153 fw4 pl20 pt10 pb10" @click="hotAllEvent">全部</text>
      </div>
      <div class="hot-topic-content">
        <div class="hot-topic-item flex-jc" v-for="(item, index) in hotTopicArr" :key='index' @click='hotTopicEvent'>
          <div class="flex lines1">
            <text class="c85 fw4 f24">#复联4首映#</text>
            <text class="c0 fw4 f28 lines1 topic-text">个性需求接受or拒绝，揭秘产品组个性需求接受or拒绝，揭秘产品组</text>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        hotTopicArr:['','','','','','']
      }
    },
    methods: {
      // 更多
      hotAllEvent() {
        this.$alert()
      },
      hotTopicEvent(){
        this.$alert()
      }
    }
  }
</script>

<style lang="css" src="../css/common.css"></style>
<style>
  .main {
    flex: 1;
    padding-top: 100px;
    background-color: #666;
  }

  .hot-topic {
    background-color: #fff;
  }

  .hot-topic-title {
    height: 40px;
    margin: 18px 23px 20px 25px;
  }

  .hot-topic-item {
    height: 88px;
    padding: 0 29px 0 24px;
  }

  .topic-text {
    width: 511px;
    margin-left: 20px;
  }
</style>