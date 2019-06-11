<template>
  <div ref="wrap">
    <!-- 热门话题 -->
    <div class="hot-topic">
      <div class="hot-topic-title flex">
        <text class="f28 fw5 c0">{{i18n.HotTopic}}</text>
        <text class="f24 c153 fw4 pl20 pt10 pb10" @click="hotAllEvent">{{i18n.All}}</text>
      </div>
      <div class="hot-topic-content" v-if='isShowLoad'>
        <div v-if='hotTopicArr.length!=0' class="hot-topic-item flex-jc" v-for="(item, index) in hotTopicArr"
          :key='index' @click='hotTopicEvent(item.topicId,item.title)'>
          <div class="flex">
            <text class="c85 fw4 f24 topci-left lines1">#{{item.title}}#</text>
            <text class="c0 fw4 f28 lines1 topic-text">个性需求接受or拒绝，揭秘产品组个性需求接受or拒绝，揭秘产品组</text>
          </div>
        </div>
        <div class="no-content flex-ac flex-jc" v-if='hotTopicArr.length==0'>
          <div class="flex-dr">
            <bui-image src="/image/sleep.png" width="42px" height="39px"></bui-image>
            <text class="f26 c51 fw4 pl15 center-height">{{isError?i18n.NoneData:i18n.ErrorLoadData}}</text>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  const link = weex.requireModule("LinkModule");
  const animation = weex.requireModule('animation')
  const dom = weex.requireModule('dom');
  const linkapi = require('linkapi');
  export default {
    data() {
      return {
        hotTopicArr: [],
        isShowLoad: false,
        isError: true,
        channel: new BroadcastChannel('WidgetsMessage'),
        i18n: ''
      }
    },
    methods: {
      hotAllEvent() {
        link.launchLinkService(['[OpenBuiltIn] \n key=BlogTopicMain'], (res) => {}, (err) => {});
      },
      hotTopicEvent(id, title) {
        link.launchLinkService(['[OpenBuiltIn] \n key=BlogTopicList \n topicId=' + id + ' \n topicTitle=' + title], (
            res) => {},
          (err) => {});
      },
      getHotTopci() {
        link.getServerConfigs([], (params) => {
          let timestamp = (new Date()).getTime();
          let objData = {
            cursor: timestamp,
            limit: 5
          }
          linkapi.fetch('GET', {
            url: params.blogUri + '/v1/topic/list/newest',
            data: objData
          }).then((res) => {
            this.isError = true
            this.isShowLoad = true
            this.broadcastWidgetHeight()
            if (res.code == 200) {
              this.hotTopicArr = res.data
            } else {

            }
          }, (err) => {
            this.error()
          })
        }, () => {
          this.error()
        });
      },
      error() {
        this.isError = false
        this.isShowLoad = true
        this.broadcastWidgetHeight()
      },
      broadcastWidgetHeight() {
        let _params = this.$getPageParams();
        setTimeout(() => {
          dom.getComponentRect(this.$refs.wrap, (ret) => {
            this.channel.postMessage({
              widgetHeight: ret.size.height,
              id: _params.id
            });
            channel.close();
          });
        }, 100)
      }
    },
    created() {
      linkapi.getLanguage((res) => {
        this.i18n = this.$window[res]
      })
    },
    mounted() {
      this.channel.onmessage = (event) => {
        if (event.data.action === 'RefreshData') {
          this.getHotTopci()
        }
      }
      this.getHotTopci()
    }
  }
</script>

<style lang="css" src="../css/common.css"></style>
<style>
  .hot-topic {
    background-color: #fff;
  }

  .hot-topic-title {
    height: 40px;
    margin: 18px 23px 20px 25px;
  }

  .hot-topic-item {
    height: 88px;
    padding: 0 32px 0 24px;
  }

  .topci-left {
    width: 160px;
  }

  .topic-text {
    width: 511px;
    margin-left: 6px;
  }

  .no-content {
    height: 166px;
    width: 750px
  }

  .center-height {
    line-height: 40px;
  }
</style>