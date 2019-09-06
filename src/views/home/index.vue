<template>
    <div class="wrap">
        <div class="wrap-background">
            <img src="https://background.cowtransfer.com/wallpaper/static/1564653832999/%E6%88%91%E7%B3%BB%E5%88%974.jpg?imageView2/0/format/jpg/interlace/1/q/60|imageslim"/>
        </div>
        <div class="content">
            <div class="content-top">
                <img class="logo" :src="logo" />
                <div class="content-top-right-bar">
                    <el-button type="success" size="small" round>升级Pro</el-button>
                    <el-avatar size="medium" src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"></el-avatar>
                </div>
            </div>
            <div class="content-main">
                <div class="content-main-card content-main-card_left">
                    <div class="content-main-card-button-com animated" :class="{bounceOutLeft: animateStatus===1}">
                        <el-upload
                            action=""
                            :show-file-list="false"
                            :auto-upload="false"
                            :on-change="handleFileChange">
                            <span slot="trigger" class="miao-button">添加文件</span>
                        </el-upload>
                    </div>
                </div>
                <div class="content-main-card content-main-card_right">
                    <div class="content-main-card-button-com animated" :class="{bounceOutRight: animateStatus===1}">
                        <span slot="trigger" class="miao-button">取件码</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import logo from '@/assets/logo.png'
export default {
    data() {
        return {
            logo,
            animateStatus: 0
        }
    },
    methods: {
        handleFileChange(file, fileList) {
            // 调出上传列表
            this.animateStatus = 1
        }
    }
}
</script>

<style lang="less" scoped>
@import './style.less';
</style>