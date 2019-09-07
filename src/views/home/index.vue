<template>
  <div class="wrap">
    <div class="wrap-background">
      <img
        src="https://background.cowtransfer.com/wallpaper/static/1564653832999/%E6%88%91%E7%B3%BB%E5%88%974.jpg?imageView2/0/format/jpg/interlace/1/q/60|imageslim"
      />
    </div>
    <div class="content">
      <div class="content-top">
        <img class="logo" :src="logo" />
        <div class="content-top-right-bar">
          <el-button type="success" size="small" round>升级Pro</el-button>
          <el-avatar
            size="medium"
            src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
          ></el-avatar>
        </div>
      </div>
      <div class="content-main">
        <div class="content-main-card content-main-card_left">
          <div
            class="content-main-card-button-com animated"
            :class="{bounceOutLeft: animateStatus===1}"
          >
            <el-upload
              action
              :show-file-list="false"
              :auto-upload="false"
              :on-change="handleFileChange"
            >
              <span slot="trigger" class="miao-button">添加文件</span>
            </el-upload>
          </div>
        </div>
        <div class="content-main-card content-main-card_right">
          <div
            class="content-main-card-button-com animated"
            :class="{bounceOutRight: animateStatus===1}"
          >
            <span slot="trigger" class="miao-button" @click="downloadHandler">取件码</span>
          </div>
        </div>
        <!-- 上传信息卡 -->
        <div
          v-show="animateStatus===1"
          class="content-main-upload-card animated"
          :class="{bounceInUp: animateStatus===1}"
        >
          <div class="file-area">
            <div class="file-list">
              <div class="file-item" v-for="file in pendingFileList" :key="file.uid">
                <div class="file-item-base">
                  <span class="icon el-icon-picture"></span>
                  <span class="name" :title="file.name">{{file.name}}</span>
                  <span class="size">{{file.size}}</span>
                  <span class="delete el-icon-delete"></span>
                </div>
              </div>
            </div>
          </div>
          <div class="info-area">
            <el-upload
              action
              :show-file-list="false"
              :auto-upload="false"
              :on-change="handleFileChange"
            >
              <span slot="trigger" class="miao-button">添加文件</span>
            </el-upload>
            <el-button @click="uploadFileList">立即上传</el-button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import logo from "@/assets/logo.png";
import axios from "axios";
export default {
  data() {
    return {
      logo,
      animateStatus: 0,
      pendingFileList: []
    };
  },
  methods: {
    handleFileChange(file, fileList) {
      // 调出上传列表
      this.animateStatus = 1;
      const files = {
        uid: file.uid,
        name: file.name,
        size: file.size,
        raw: file.raw
      };
      this.pendingFileList.push(files);
    },
    /*
     * 立即上传
     */
    uploadFileList() {
      let form = new FormData();
      //    this.pendingFileList.forEach(file => {
      //        form.append()
      //     })
      form.append("file", this.pendingFileList[0].raw);
      axios
        .post("/api/upload", form, {
          "Content-Type": "multipart/form-data"
        })
        .then(res => {
          if (res.data.code === 0) {
            this.$notify({
              title: "上传成功",
              message: `你的${this.pendingFileList.length}个文件成功上传`,
              type: "success"
            });
            // TODO
          }
        });
    },
    downloadHandler() {
      axios({
        method: "get",
        url: "/api/download",
        responseType: "blob"
      }).then(res => {
        if (!res) {
          return;
        }
        const filename = res.headers["content-disposition"].match(
          /(?<=(filename=))(.*)/g
        );

        let url = window.URL.createObjectURL(new Blob([res.data]));
        let link = document.createElement("a");
        link.style.display = "none";
        link.href = url;
        link.setAttribute("download", filename);
        document.body.appendChild(link);
        link.click();
      });
    }
  }
};
</script>

<style lang="less" scoped>
@import "./style.less";
</style>
