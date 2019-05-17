<template>
    <div style="flex:1">
        <hotTopic></hotTopic>
    </div>
</template>

<!--引入bui-weex样式文件-->
<style lang="scss" src="bui-weex/src/css/buiweex.scss"></style>

<script>
    import hotTopic from "./hotTopic.vue";
    module.exports = {
        data () {
            return {}
        },
        components: {
          hotTopic
        }
    }
</script>