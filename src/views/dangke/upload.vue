<template>
  <div class="dangke-upload-container">
    <uploader ref="uploader" :options="options" class="uploader-example"  @complete="complete" :file-status-text="statusText" >
      <uploader-unsupport></uploader-unsupport>
      <uploader-drop>
        <!-- <p>Drop files here to upload or</p> -->
        <uploader-btn :attrs="attrs">选择上传视频文件</uploader-btn>
        <!-- <uploader-btn :attrs="attrs">select images</uploader-btn>
        <uploader-btn :directory="true">select folder</uploader-btn> -->
      </uploader-drop>
      <uploader-list></uploader-list>
    </uploader>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex'
  export default {
    name: 'upload',
    data () {
      return {
        options: {
          target: process.env.BASE_API + '/api/cos/upload/big/upload?key=video/dangke',
          testChunks: false,
          // singleFile: true,
        },
        attrs: {
          accept: 'video/*'
        },
        statusText: {
          success: '上传成功',
          error: '上传失败',
          uploading: '上传中',
          paused: '暂停中',
          waiting: '等待中'
        }
      }
    },
    mounted () {
      this.$nextTick(() => {
        window.uploader = this.$refs.uploader.uploader
      })
    },
    methods: {
      complete () {
        console.log('complete', arguments)
      },
    },
  }
</script>

<style rel="stylesheet/less" lang="less">
  .dangke-upload-container {
    padding: 1%;
  }
  .uploader-example {
    width: 880px;
    padding: 15px;
    margin: 40px auto 0;
    font-size: 12px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .4);
  }
  .uploader-example .uploader-btn {
    margin-right: 4px;
  }
  .uploader-example .uploader-list {
    max-height: 440px;
    overflow: auto;
    overflow-x: hidden;
    overflow-y: auto;
  }
</style>
