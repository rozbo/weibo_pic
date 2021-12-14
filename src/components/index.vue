<template>
  <el-row :gutter="20">
    <el-card class="box-card">
      <div class="demo-basic--circle">
        <div class="block">
          <el-avatar :size="150" :src="avatar"></el-avatar>
        </div>
      </div>
      <h1>微博图片去水印解析</h1>
      <div class="typo">
        <p><strong>温馨提示 </strong>粘贴图片地址到下面，再点击解析按钮，稍等后再右键保存即可。</p>
        <p><small>2021年12月25日，为了给老婆去除头像的水印，反正也睡不着，直接搞个工具吧，省的以后再有这种需求烦我。</small></p>
      </div>
      <hr>
      <div class="main" v-loading="loading">
        <div class="grid-content">
          <el-input placeholder="请粘贴图片链接" v-model="input" id="url" lass="input-with-select">
            <el-button slot="append" @click="onSubmit">解析</el-button>
          </el-input>
        </div>

        <div class="waterfall" v-if="iseen">
          <div class="item">
            <el-image
                :src="images_url" @load="onImageLoad">
            </el-image>
          </div>
        </div>
      </div>
    </el-card>
  </el-row>
</template>
<script>
export default {
  data() {
    return {
      info: {},
      input: '',
      select: '',
      music: false,
      iseen: false,
      loading: false,
      avatar: "https://lz.sinaimg.cn/oslarge/a15cd863ly1gwkhur90u0j20u00t8wft.jpg",
      images_url: {}
    }
  },
  methods: {
    onImageLoad() {
      this.loading = false
      // this.iseen = true
    },
    onSubmit() {
      this.images_url = ""
      this.iseen = false
      this.loading = true
      const url = /[\s\S]+?\/([\w]+\.jpg)/.exec(this.input)
      if (url?.length !== 2) {
        this.$notify.error({
          title: '解析失败',
          message: '图片网址有误'
        });
        this.iseen = false
        this.loading = false
      } else {
        this.images_url = "https://lz.sinaimg.cn/oslarge/" + url[1] + "?rnd=" + new Date().getTime();
        this.$notify.success({
          title: '解析成功',
          message: '在下方长按，保存到相册即可'
        });
        this.iseen = true
        // this.loading = false
      }
    },
  }
}
</script>

<style>
::selection {
  background: rgba(0, 149, 255, .1);
}

body:before {
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: .3;
  z-index: -1;
  content: "";
  position: fixed;
  background-image: linear-gradient(135deg, #CE9FFC 10%, #7367F0 100%);
}

.grid-content {
  margin-top: 1em;
  border-radius: 4px;
  min-height: 50px;
}

.el-select .el-input {
  width: 80px;
}

.input-with-select .el-input-group__prepend {
  background-color: #fff;

}

.box-card {
  margin-top: 4em !important;
  margin-bottom: 4em !important;
  opacity: .8;
}

@media screen and (max-width: 700px) {
  .box-card {
    margin-top: 1em !important;
    margin-bottom: 1em !important;
  }
}

.download h3 {
  margin-top: 2em;
}

.download button {
  margin-right: 0.5em;
  margin-left: 0.5em;
}

.waterfall {
  display: inline-block;
  box-sizing: border-box;
  text-align: center;
  color: #fff;
  background: #e7b5d3;
}

.item {
  padding: 5px;
  break-inside: avoid;
}

.item img {
  width: 100%;
}

.typo {
  text-align: left;
}

.typo a {
  color: #2c3e50;
  text-decoration: none;
}

hr {
  height: 10px;
  margin-bottom: .8em;
  border: none;
  border-bottom: 1px solid rgba(0, 0, 0, .12);
}
</style>
