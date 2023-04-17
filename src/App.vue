<template>
  <div class="box">
    <div class="header">
      <button class="btn" @click="chooseImg">选择图片</button>
      <input
        ref="fileInputRef"
        @change="(e) => fileChange(e as HTMLInputEvent)"
        type="file"
        accept="image/*"
        style="display: none"
      />
      <button
        class="btn"
        v-show="hasImg"
        @click="downloadImg"
        style="margin-left: 20px; display: none"
      >
        下载黑白图片
      </button>
    </div>
    <div class="container">
      <img ref="imageRef" @load="imgLoad" :src="imgUrl" v-if="hasImg" />
      <div class="image-area" v-else>暂无图片</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import ImagesHandler from "./utils/ImagesHandler";

interface HTMLInputEvent extends Event {
  target: HTMLInputElement & EventTarget;
}

const fileInputRef = ref<HTMLInputElement | null>(null);
const imgUrl = ref("");
const hasImg = ref(false);
let imgName = "";

const chooseImg = () => {
  fileInputRef.value!.click();
};

const fileChange = (e: HTMLInputEvent) => {
  const file = e.target.files![0];
  if (file) {
    imgName = file.name;
    imgUrl.value = URL.createObjectURL(file);
    hasImg.value = true;
  } else {
    imgName = "";
    imgUrl.value = "";
    hasImg.value = false;
  }
};

const imgLoad = () => {
  hasImg.value = true;
};

const downloadImg = () => {
  const imagesHandler = new ImagesHandler(imgUrl.value);
  imagesHandler.blackWhite().then(() => imagesHandler.downloadImg(imgName));
};
</script>

<style scoped>
.box {
  width: 100vw;
  height: 100vh;
}
.btn {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  line-height: 1;
  height: 32px;
  white-space: nowrap;
  cursor: pointer;
  color: #606266;
  text-align: center;
  box-sizing: border-box;
  outline: none;
  transition: 0.1s;
  user-select: none;
  vertical-align: middle;
  -webkit-appearance: none;
  border: 1px solid #dcdfe6;
  padding: 8px 15px;
  font-size: 14px;
  border-radius: 4px;
  background-color: #fff;
}

.btn:hover {
  background-color: #f4f4f5;
  color: #909399;
}

.header {
  display: flex;
  align-items: center;
  height: 15%;
  padding: 0 10px;
}

.container {
  width: 100%;
  height: 85%;
  padding: 0 10px;
  box-sizing: border-box;
}

.image-area {
  height: 95%;
  background-color: #f5f7fa;
  color: #909399;
  display: flex;
  justify-content: center;
  align-items: center;
}

img {
  width: calc(100% - 20px);
  height: 95%;
}
</style>
