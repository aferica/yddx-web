<template>
  <div class="dashboard-container">
    <div class="title">欢迎使用移动党校管理控制台</div>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex'
  export default {
    name: 'dashboard',
    computed: {
        name(){
            return this.$route.path
        }
    }
  }
</script>

<style rel="stylesheet/less" lang="less">
.dashboard-container{
  text-align: center;
  padding: 1%;
}
.title {
  padding-top: 10%;
  font-size: 24px;
  font-weight: bolder;
}
</style>
