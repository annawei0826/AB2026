<template>
  <div id="host_info_box" :class="{ show: isOpen }" @click="close">
    <div class="ct">
      <div class="host_info_item" @click.stop>
        <img src="@/assets/image/button-close.png" class="close_box" @click="close">
        
        <div class="modal_content" v-if="speaker" ref="modalContent" tabindex="0">
          <div class="photo_section">
            <div class="photo_wrapper">
              <img :src="getImageUrl(speaker.img)" :alt="speaker.name" class="speaker_photo">
            </div>
          </div>
          
          <div class="info_section">
            <h2 class="speaker_name" v-html="speaker.name"></h2>
            <p class="speaker_title" v-html="speaker.title"></p>
            <div class="speaker_desc" v-html="speaker.desc"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, nextTick } from 'vue';

const props = defineProps({
  isOpen: Boolean,
  speaker: Object
});

const emit = defineEmits(['close']);

const close = () => {
  emit('close');
};

const modalContent = ref(null);

watch(() => props.isOpen, (open) => {
  if (open) {
    nextTick(() => {
      try { modalContent.value?.focus(); } catch (e) {}
    });
  }
});

const getImageUrl = (name) => {
  if (!name) return '';
  try {
    return new URL(`../assets/image/${name}`, import.meta.url).href;
  } catch (e) {
    console.error('Image load error:', e);
    return '';
  }
};
</script>

<style scoped>
#host_info_box {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 20, 50, 0.9);
  display: none;
  z-index: 9999;
  box-sizing: border-box;
}

#host_info_box.show {
  display: flex;
  align-items: center;
  justify-content: center;
}

#host_info_box .ct {
  position: relative;
  width: 100%;
  max-width: 850px;
  margin: 0 40px;
}

.host_info_item {
  position: relative;
  width: 100%;
}

.close_box {
  position: absolute;
  top: -60px;
  right: 0;
  width: 40px;
  height: 40px;
  cursor: pointer;
  z-index: 10;
  transition: opacity 0.2s;
  filter: brightness(0) saturate(100%) invert(85%) sepia(16%) saturate(1015%) hue-rotate(359deg) brightness(102%) contrast(93%);
}

.close_box:hover {
  opacity: 0.7;
}

/* 彈窗容器 - 限制高度 */
/* 彈窗容器 - 限制高度，四邊 padding 一致 */
.modal_content {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 30px;
  width: 100%;
  height: 450px;
  max-height: 70vh;
  background: #f5f5f0;
  border-radius: 16px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.4);
  padding: 40px;
  box-sizing: border-box;
  overflow: hidden;
  align-items: center;  /* 改成 center：讓左右兩邊都垂直置中 */
}

/* 照片區域 - 固定不滾動 */
.photo_section {
  display: flex;
  align-items: center;
  justify-content: center;
}

.photo_wrapper {
  width: 240px;
  height: 310px;
  background: url('@/assets/image/bg-gold.jpg') center center no-repeat;
  background-size: cover;
  overflow: hidden;
  position: relative;
  flex-shrink: 0;
}

.speaker_photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
  display: block;
}

/* 資訊區域 - 可滾動 */
.info_section {
  max-height: 100%;  /* 改成 max-height，讓它可以自適應內容高度 */
  overflow-y: auto;
  overflow-x: hidden;
  padding-right: 10px;
  margin-right: -10px;
}

.speaker_name {
  font-size: 32px;
  font-weight: bold;
  color: #7f560e;
  margin: 0 0 8px 0;
  line-height: 1.3;
}

.speaker_title {
  font-size: 15px;
  color: #333;
  margin: 0 0 20px 0;
  line-height: 1.6;
}

.speaker_desc {
  font-size: 15px;
  color: #333;
  line-height: 1.8;
  text-align: justify;
}

/* 滾動條樣式 */
.info_section::-webkit-scrollbar {
  width: 6px;
}

.info_section::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.05);
  border-radius: 3px;
}

.info_section::-webkit-scrollbar-thumb {
  background: rgba(127, 86, 14, 0.3);
  border-radius: 3px;
}

.info_section::-webkit-scrollbar-thumb:hover {
  background: rgba(127, 86, 14, 0.5);
}

/* 平板 */
@media screen and (max-width: 900px) {
  #host_info_box .ct {
    max-width: 750px;
    margin: 0 20px;
  }
  
  .modal_content {
    height: 480px;
    max-height: 60vh;
  }
}

/* 手機橫式/小平板 */
@media screen and (max-width: 768px) {
  #host_info_box {
    padding: 15px;
    overflow-y: auto;
  }
  
  #host_info_box .ct {
    max-width: 90%;
    margin: 70px auto 30px;
  }
  
  .modal_content {
    display: flex;
    flex-direction: column;
    height: auto;
    max-height: calc(100vh - 120px);
    padding: 40px 30px;
    gap: 25px;
    overflow-y: auto;
  }
  
  .photo_section {
    width: 200px;
    margin: 0 auto;
  }
  
  .photo_wrapper {
    width: 200px;
    height: 258px;
  }
  
  .info_section {
    width: 100%;
    overflow-y: visible;
    padding-right: 0;
  }
  
  .speaker_name {
    font-size: 28px;
    text-align: center;
  }
  
  .speaker_title {
    font-size: 14px;
    text-align: center;
  }
  
  .speaker_desc {
    font-size: 14px;
  }
  
  .close_box {
    top: -45px;
    width: 35px;
    height: 35px;
  }
}

/* 手機直式 */
@media screen and (max-width: 480px) {
  #host_info_box {
    padding: 10px;
  }
  
  #host_info_box .ct {
    max-width: 100%;
    margin: 60px auto 20px;
  }
  
  .modal_content {
    border-radius: 12px;
    padding: 30px 20px;
    max-height: calc(100vh - 100px);
    gap: 20px;
  }
  
  .photo_section {
    width: 180px;
  }
  
  .photo_wrapper {
    width: 180px;
    height: 232px;
  }
  
  .speaker_name {
    font-size: 24px;
    margin-bottom: 6px;
  }
  
  .speaker_title {
    font-size: 13px;
    margin-bottom: 18px;
  }
  
  .speaker_desc {
    font-size: 13px;
    line-height: 1.7;
  }
  
  .close_box {
    top: -40px;
    right: 5px;
    width: 32px;
    height: 32px;
  }
}

/* 極小手機 */
@media screen and (max-width: 360px) {
  .modal_content {
    padding: 25px 15px;
  }
  
  .photo_section {
    width: 160px;
  }
  
  .photo_wrapper {
    width: 160px;
    height: 206px;
  }
  
  .speaker_name {
    font-size: 22px;
  }
  
  .speaker_title {
    font-size: 12px;
  }
  
  .speaker_desc {
    font-size: 12px;
  }
}
</style>