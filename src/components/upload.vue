<template>
  <div class="upload">
    <div class="text" ref="choosePhoto">
      <span>将文件拖拽此处,或</span>
      <span @click="inputFile()" class="cilPhoto"><i class="el-icon-upload"></i>点击上传图片</span>
      <input type="file" multiple class="uploadInput" style="display: none" id="file" />
    </div>
    <div class="filePhoto" v-for="(item, index) in fileData" :key="index">
      <div class="showFile">
        <div><i class="el-icon-document"></i>{{ item.fileText }}</div>
        <div>
          <span @click="look(index)">查看</span><el-button type="text" @click="open(index)">删除</el-button>
        </div>
      </div>
      <div class="showPhoto" v-show="index === currentIndex">
        <img :src="fileArray[index]" class="sizePhoto">
        <i class="el-icon-close position" @click="hidden(index)"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "upload",
  data() {
    return {
      imgArr: [],
      fileData: [],
      fileurl: [],
      currentIndex: 0,
      fileArray: []
    };
  },
  mounted() {
    this.$refs.choosePhoto.ondragenter = e => {
      e.preventDefault(); //阻止拖入时的浏览器默认行为
      this.$refs.choosePhoto.border = "2px dashed red";
    };
    this.$refs.choosePhoto.ondragover = e => {
      e.preventDefault(); //阻止拖来拖去的浏览器默认行为
    };
    this.$refs.choosePhoto.ondragleave = e => {
      e.preventDefault(); //阻止离开时的浏览器默认行为
    };
    this.$refs.choosePhoto.ondrop = e => {
      this.choose(e);
    };
  },
  methods: {
    //修改input上传文件原有样式
    inputFile() {
      let file = document.getElementById("file");
      file.click();
    },
    choose(e) {
      e.stopPropagation();
      e.preventDefault(); //必填字段
      let fileData = e.dataTransfer.files;
      console.log(fileData);
      this.uploadFile(fileData);
    },
    uploadFile: function(file) {
      //渲染上传文件
      for (let i = 0; i !== file.length; i++) {
        let fileJson = {
          Url: "",
          fileText: ""
        };
        let video_type =
          file[i].type === "video/mp4" || file[i].type === "video/ogg";
        if (file[i].type.indexOf("image") === 0) {
          //如果是图片
          // console.log(file[i])
          this.fileurl[i] = window.URL.createObjectURL(file[i]); //创建一个url连接,供src属性引用
          this.fileArray.push(this.fileurl[i]); //把url链接压入到fileArray数组中，当用户需要查看时，再调用fileArray
          // console.log(this.fileArray);
        } else if (video_type) {
          alert("不支持此类型文件");
        }
        fileJson.fileText = file[i].name; //图片名字
        this.fileData.push(fileJson); //把图片压入到fileData数组里
      }
      // console.log(this.fileData)
    },
    //删除图片
    open(index) {
      this.$confirm("确定要删除吗", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.fileData.splice(index, 1);
          this.$message({
            type: "success",
            message: "删除成功!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    //查看图片
    look(index) {
      // console.log("index" + index)
      this.currentIndex = index;
    },
    //点击隐藏图片
    hidden(index) {
      console.log(index)
      this.currentIndex = 100 ;
    }
  }
};
</script>

<style scoped>
.upload .text {
  height: 30vh;
  width: 70vw;
  border: 1px solid rgba(0, 0, 0, 0.5);
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.upload .text .cilPhoto {
  color: cadetblue;
  cursor: pointer;
}

.upload .showFile {
  margin: 5px auto;
  width: 68vw;
  height: 40px;
  line-height: 40px;
  display: flex;
  justify-content: space-between;
  border: 1px solid rgba(0,0,0,0.3);
  padding: 0 1vw;
  border-radius: 5px;
}

.upload .filePhoto .showPhoto {
  position: relative;
}

.upload .filePhoto .showPhoto .sizePhoto {
  height: 30vh;
  width: 60vw;
}

.position {
  position: absolute;
  top: 1%;
  right: 19.5%;
  height: 20px;
  width: 20px;
  line-height: 20px;
  border-radius: 50%;
  background-color: red;
}
</style>
