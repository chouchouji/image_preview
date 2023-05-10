<script setup lang="ts">
import { ref } from 'vue'

import ImagePreview from '@/common/ImagePreview.vue'
import { getImageUrl } from '@/utils'

const IMG_WIDTH = document.documentElement.clientWidth * 0.6
const IMAGE_LIST = Array.from(Array(3), (_item, index: number) => {
  return getImageUrl(`../assets/cats/${index + 1}.jpg`)
})

const curIndex = ref(0)
const showImagePreview = ref(false)

const selectImage = (index: number) => {
  curIndex.value = index
  showImagePreview.value = true
}

function close(activeIndex: number) {
  const element = document.querySelector('.images') as HTMLElement
  element.scrollLeft = activeIndex * IMG_WIDTH
}
</script>

<template>
  <div class="container">
    <div class="images">
      <div class="image_box" v-for="(imgUrl, index) in IMAGE_LIST">
        <img class="image" :src="imgUrl" alt="image" @click="selectImage(index)" />
      </div>
    </div>
  </div>
  <ImagePreview
    v-model:show-image="showImagePreview"
    :image-list="IMAGE_LIST"
    :start-index="curIndex"
    @close="close"
  ></ImagePreview>
</template>

<style scoped>
.container {
  width: 100%;
  height: 100vh;
  overflow-y: hidden;
  display: flex;
  align-items: center;
}

.images {
  width: 100%;
  height: 60vh;
  overflow-x: scroll;
  overflow-y: hidden;
  display: flex;
  flex-direction: row;
}

.image_box {
  margin: 0 10px;
  min-width: 60%;
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border: 1px solid #efefef;
}
</style>
