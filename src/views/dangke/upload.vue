<template>
  <div class="dangke-upload-container">
    <uploader ref="uploader" class="uploader-example"
      :options="options"
      :autoStart="autoStart"
      :file-status-text="statusText"
      @file-added="getFileKeyAndUploadId"
      @file-complete="multipartComplete" >
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
  import axios from 'axios'
  import md5 from 'js-md5'

  export default {
    name: 'upload',
    data () {
      return {
        options: {
          target: '/api/cos/upload/multipart',
          testChunks: false,
          singleFile: true,
          chunkSize: 2*1024*1024
        },
        autoStart: false,
        attrs: {
          // accept: 'video/*'
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
      // complete () {
      //   console.log('complete', arguments)
      // },
      async getFileKeyAndUploadId (file) {
        // console.log(file.uniqueIdentifier)
        let filename = file.name
        const filetype = filename.split('.')[filename.split('.').length - 1]
        filename = md5(filename) + '.' + filetype
        file.name = filename
        const result = await axios.get('/api/cos/upload/multipart/key?identifier=' +
          encodeURIComponent(file.uniqueIdentifier) + '&filename=' + filename)
        .then(res => {
          // console.log(res)
          if(res.data.code == 0) {
            return true
          }
          return false
        })
        if(result) {
          file.resume()
        }
        return false
      },
      async multipartComplete (file) {
        console.log(file)
        const result = axios.get('/api/cos/upload/complete?identifier=' + encodeURIComponent(file.uniqueIdentifier))
        .then(res => {
          console.log(res)
          if(res.data.code == 0) {
            // window.uploader.upload()
          } else {
            return false
          }
        })
        if(!result) {
          return false
        }
      }
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
