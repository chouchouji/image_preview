<script setup lang="ts">
import { ref, toRef, defineEmits, watch } from 'vue'

import { Pagination } from 'swiper'
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/swiper-bundle.css'

const modules = [Pagination]

const props = defineProps({
  showImage: Boolean,
  imageList: {
    type: Array<string>,
    required: true
  },
  startIndex: {
    type: Number
  }
})

const activeIndex = ref(0)
const showImage = toRef(props, 'showImage')
const emits = defineEmits(['update:showImage', 'close'])

// 预览开启禁止滚动，关闭允许滚动
watch(showImage, (cur: boolean, _pre: boolean) => {
  if (cur) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = 'scroll'
  }
})

const close = () => {
  emits('update:showImage', false)
  emits('close', activeIndex.value)
}

const slideChange = (swiper: any) => {
  activeIndex.value = swiper?.activeIndex
}
</script>

<template>
  <div class="swiper" v-if="showImage" @click="close">
    <swiper
      :spaceBetween="10"
      :pagination="{
        el: '.custom_pagination',
        type: 'fraction',
        clickable: false
      }"
      :initialSlide="startIndex"
      :modules="modules"
      @slideChange="slideChange"
    >
      <!-- 页码 -->
      <div class="custom_pagination"></div>
      <swiper-slide v-for="imageUrl in imageList">
        <img :src="imageUrl" alt="image" />
      </swiper-slide>
    </swiper>
  </div>
</template>

<style scoped>
.swiper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: 2000;
  background-color: black;
}
.swiper-slide {
  display: flex;
  justify-content: center;
}
.swiper-slide img {
  max-height: 70vh;
  object-fit: contain;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
.custom_pagination {
  position: absolute;
  width: 60px;
  height: 20px;
  top: 10px;
  left: 50%;
  text-align: center;
  line-height: 20px;
  transform: translateX(-50%);
  color: white;
}
</style>
