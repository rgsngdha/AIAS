<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="120">
      <el-form-item label="音频文件链接">
        <el-input v-model="form.url" />
      </el-form-item>
      <el-row>
        <el-col :span="9">
          <el-form-item>
            <el-button
              v-loading.fullscreen.lock="fullscreenLoading"
              type="primary"
              size="small"
              element-loading-text="拼命加载中"
              @click="onSubmit"
            >语音识别</el-button>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="">
            <json-viewer
              :value="form.result1"
              :expand-depth="3"
              copyable
              width="400px"
            />
          </el-form-item>
        </el-col>
      </el-row>

      <el-form-item>
        <el-divider />
      </el-form-item>
      <el-row>
        <el-col :span="9">
          <el-form-item label="本地音频文件">
            <el-upload
              ref="upload"
              name="audioFile"
              class="upload"
              :action="upload()"
              :on-preview="handlePreview"
              :on-change="handleChange"
              :on-remove="handleRemove"
              :on-success="handleSuccess"
              :before-upload="beforeUpload"
              ::limit="1"
              :show-file-list="false"
              :auto-upload="false"
            >
              <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
              <el-button
                v-loading.fullscreen.lock="fullscreenLoading"
                style="margin-left: 10px;"
                type="success"
                size="small"
                element-loading-text="拼命加载中"
                @click="submitUpload"
              >上传</el-button>
              <div slot="tip" class="el-upload__tip">音频格式: wav, mp3, flac, mp4 等</div>
            </el-upload>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="">
            <json-viewer
              :value="form.result2"
              :expand-depth="3"
              copyable
              width="500px"
            />
          </el-form-item>
        </el-col>
      </el-row>

    </el-form>
  </div>
</template>

<script>
import { zhAsrForAudioUrl } from '@/api/inference'
import JsonViewer from 'vue-json-viewer'

export default {
  name: 'InferenceDetail',
  components: {
    JsonViewer
  },
  data() {
    return {
      fullscreenLoading: false,
      form: {
        url: 'https://aias-home.oss-cn-beijing.aliyuncs.com/products/audio/test.wav',
        result1: '',
        result2: ''
      }
    }
  },
  methods: {
    upload() {
      // return window.g.Base_URL + '/api/asr/zhAsrForAudioFile'
      return `${process.env.VUE_APP_BASE_API}/api/asr/zhAsrForAudioFile`
    },
    submitUpload() {
      this.fullscreenLoading = true
      this.$refs.upload.submit()
    },
    handleRemove(file, fileList) {
      console.log(file, fileList)
    },
    handleChange(file) {
      console.log(file)
    },
    handlePreview(file) {
      console.log(file)
    },
    handleSuccess(file) {
      this.form.result2 = file.data.result
      this.fullscreenLoading = false
    },
    beforeUpload(file) {
      const pass = file.type === 'audio/mp3' || 'image/mpeg'
      if (!pass) {
        this.$message.error('音频文件应为 wav,mp3,flac等!')
      }
      return pass
    },
    onSubmit() {
      this.fullscreenLoading = true
      zhAsrForAudioUrl(this.form).then(response => {
        this.fullscreenLoading = false
        this.form.result1 = response.data.result
      })
    }
  }
}
</script>

<style scoped>
  .el-input {
    width: 600px;
  }

  .input-with-select .el-input-group__prepend {
    background-color: #fff;
  }

  .line {
    text-align: center;
  }
</style>
