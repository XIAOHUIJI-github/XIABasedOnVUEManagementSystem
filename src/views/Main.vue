<template>
  <el-container style="height: 100%">
    <el-aside width="auto"><common-aside></common-aside></el-aside>
    <el-container>
      <el-header>
        <common-header></common-header>
      </el-header>
      <common-tag></common-tag>
      <el-main><router-view /></el-main>
    </el-container>
  </el-container>
</template>

<script>
import CommonAside from '../components/CommonAside.vue';
import CommonHeader from '../components/CommonHeader.vue';
import CommonTag from "../components/CommonTag.vue";
export default {
  name: 'Main',
  components: {CommonAside,CommonHeader,CommonTag},
};
</script>
<style lang = "scss" scoped>

.el-header{
  background: #333;
}
.el-main{
  padding-top: 0px;
}
</style>
