<template>
  <div class="app-container">
    <el-form label-width="120px">
      <el-form-item label="信息描述">
        <el-tag type="info">excel模版说明</el-tag>
        <el-tag>
          <i class="el-icon-download" />
          <!-- 下面链接文件为课程分类excel模板在阿里云oss上的地址 -->
          <a
            :href="'https://zsstudy-file.oss-cn-hangzhou.aliyuncs.com/%E8%AF%BE%E7%A8%8B%E5%88%86%E7%B1%BBexcel%E6%A8%A1%E6%9D%BF/01.xlsx'"
          >点击下载模版</a>
        </el-tag>
      </el-form-item>

      <el-form-item label="选择Excel">
        <el-upload
          ref="upload"
          :auto-upload="false"
          :on-success="fileUploadSuccess"
          :on-error="fileUploadError"
          :disabled="importBtnDisabled"
          :limit="1"
          :action="BASE_API+'/eduservice/subject/addSubject'"
          name="file"
          accept="application/vnd.ms-excel"
        >
          <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
          <el-button
            :loading="loading"
            style="margin-left: 10px;"
            size="small"
            type="success"
            @click="submitUpload"
          >上传到服务器</el-button>
        </el-upload>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
import teacherApi from "@/api/edu/teacher";
import ImageCropper from "@/components/ImageCropper";
import PanThumb from "@/components/PanThumb";
export default {
  data() {
    return {
      BASE_API: process.env.BASE_API, // 接口API地址
      OSS_PATH: process.env.OSS_PATH, // 阿里云OSS地址
      importBtnDisabled: false, // 按钮是否禁用,
      loading: false
    };
  },
  created() {},
  methods: {
    //点击按钮上传文件到接口里面
    submitUpload() {
      //表示表单的提交，没有用ajax
      this.fileUploadBtnText = "正在上传";
      this.importBtnDisabled = true;
      this.loading = true;
      this.$refs.upload.submit();
    },
    //上传重成功
    fileUploadSuccess(response) {
      if (response.success === true) {
        this.fileUploadBtnText = "添加成功";
        this.loading = false;
        this.$message({
          type: "success",
          message: response.message
        });
        //跳转课程分类列表
        //路由
        this.$router.push({path:'/edu/subject/list'})
      }
    },
    //上传失败
    fileUploadError(response) {
      this.fileUploadBtnText = "添加失败";
      this.loading = false;
      this.$message({
        type: "error",
        message: "添加失败"
      });
    }
  }
};
</script>
