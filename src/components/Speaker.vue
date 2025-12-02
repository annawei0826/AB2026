<!-- Speaker.vue -->
<template>
  <div class="sec speaker-section" id="sec2">
    <div class="mid_content" data-aos="fade-up">
      <div class="article_title_wrap">
        <div class="article_title">
          {{ infoData.title || '講者陣容' }}
        </div>
      </div>

      <div class="speaker-carousel-container">
        <button class="carousel-arrow carousel-arrow-left" @click="swiperPrev">
          <img src="@/assets/image/arrow_left.png" alt="上一張">
        </button>

        <div class="carousel-wrapper">
          <swiper
            :modules="modules"
            :slides-per-view="1"
            :space-between="20"
            :loop="true"
            :autoplay="{ delay: 4000, disableOnInteraction: false }"
            :speed="800"
            :breakpoints="{
              600: { slidesPerView: 1, spaceBetween: 20 },
              768: { slidesPerView: 2, spaceBetween: 20 },
              1024: { slidesPerView: 3, spaceBetween: 20 }
            }"
            @swiper="onSwiper"
            class="speaker-swiper"
          >
            <swiper-slide v-for="(speaker, index) in speakers" :key="index">
              <div class="speaker-card-wrapper">
                <div class="card-base" :class="`card-${index}`">
                  
                  <div class="photo-container">
                    <img 
                      :src="getImageUrl(speaker.img)" 
                      :alt="speaker.name"
                      class="speaker-photo"
                      :style="getPhotoStyle(speaker)"
                    >
                  </div>
                  
                  <div class="info-section">
                    <h3 class="speaker-name" v-html="speaker.name"></h3>
                    
                    <div v-if="speaker.englishName" class="speaker-english-name">
                      {{ speaker.englishName }}
                    </div>
                    
                    <p class="speaker-title" v-html="speaker.title"></p>
                    
                    <button 
                      v-if="speaker.desc && speaker.desc.trim() !== ''"
                      class="detail-btn"
                      @click="openModal(speaker)"
                    >
                      個人簡介
                    </button>
                  </div>
                  
                  <div class="gold-border"></div>
                </div>
              </div>
            </swiper-slide>
          </swiper>
        </div>

        <button class="carousel-arrow carousel-arrow-right" @click="swiperNext">
          <img src="@/assets/image/arrow_right.png" alt="下一張">
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Navigation } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/navigation';

const props = defineProps({
  speakers: {
    type: Array,
    default: () => []
  },
  infoData: {
    type: Object,
    default: () => ({})
  }
});

const modules = [Autoplay, Navigation];
const swiperInstance = ref(null);
const windowWidth = ref(window.innerWidth);

const emit = defineEmits(['open-modal']);

const getImageUrl = (name) => {
  return new URL(`../assets/image/${name}`, import.meta.url).href
}

const getScaleRatio = computed(() => {
  if (windowWidth.value <= 480) return 0.714; 
  if (windowWidth.value <= 680) return 0.8;   
  if (windowWidth.value <= 900) return 0.914; 
  return 1; 
});

const getPhotoStyle = (speaker) => {
  const settings = speaker.imgSettings || {};
  const bottomValue = parseInt(settings.bottom || '0');
  const scaledBottom = Math.round(bottomValue * getScaleRatio.value);
  
  return {
    width: settings.width || '100%',
    height: settings.height || '100%',
    objectFit: settings.objectFit || 'cover',
    objectPosition: settings.objectPosition || 'center top',
    bottom: `${scaledBottom}px`
  };
};

const onSwiper = (swiper) => {
  swiperInstance.value = swiper;
};

const swiperPrev = () => {
  swiperInstance.value?.slidePrev();
};

const swiperNext = () => {
  swiperInstance.value?.slideNext();
};

const openModal = (speaker) => {
  emit('open-modal', speaker);
};

const handleResize = () => {
  windowWidth.value = window.innerWidth;
};

onMounted(() => {
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
});
</script>

<style scoped>
.speaker-section {
  background-image: url('@/assets/image/bg-blue2.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
}

.speaker-carousel-container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 20px;
}

.carousel-wrapper {
  flex: 1;
  overflow: hidden;
}

.speaker-swiper {
  width: 100%;
  padding: 20px 0;
}

.carousel-arrow {
  flex-shrink: 0;
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  transition: transform 0.3s ease;
  z-index: 10;
}

.carousel-arrow:hover {
  transform: scale(1.1);
}

.carousel-arrow img {
  width: 100%;
  height: 100%;
  display: block;
}

.carousel-arrow-left {
  order: -1;
}

.carousel-arrow-right {
  order: 1;
}

.speaker-card-wrapper {
  width: 100%;
  max-width: 280px;
  margin: 0 auto;
}

.card-base {
  position: relative;
  width: 100%;
  aspect-ratio: 250 / 440;
  background-image: url('@/assets/image/bg-gold.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.photo-container {
  position: absolute;
  bottom: 120px;
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 20px);
  height: 350px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  z-index: 1;
}

.speaker-photo {
  display: block;
  position: absolute;
}

.info-section {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 170px;
  background-image: url('@/assets/image/bg-gold.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 20px 20px;
  box-sizing: border-box;
  z-index: 2;
}

.speaker-name {
  font-size: 24px;
  font-weight: 700;
  color: #7f560e;
  text-align: center;
  line-height: 1.3;
  margin: 0 0 4px 0;
  padding: 0;
  flex-shrink: 0;
}

.speaker-name:has(br) {
  font-size: 18px;
  line-height: 1.2;
}

.speaker-english-name {
  font-size: 13px;
  color: #333;
  text-align: center;
  line-height: 1.2;
  margin: 0 0 4px 0;
  padding: 0;
  flex-shrink: 0;
}

.speaker-title {
  font-size: 15px;
  color: #333;
  text-align: center;
  line-height: 1.4;
  margin: 0;
  padding: 0;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.detail-btn {
  margin: 0;
  padding: 2px 24px;
  background: #7f560e;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.3s ease;
  flex-shrink: 0;
  margin-top: auto;
}

.detail-btn:hover {
  background: #5f3f0a;
}

.gold-border {
  position: absolute;
  top: 10px;
  left: 10px;
  right: 10px;
  bottom: 10px;
  border: 5px solid;
  border-color: #b98b23;
  pointer-events: none;
  z-index: 3;
}

@media screen and (max-width: 900px) {
  .carousel-arrow {
    width: 40px;
    height: 40px;
  }
  
  .card-base {
    aspect-ratio: 250 / 430;
  }

  .photo-container {
    height: 320px;
  }

  .info-section {
    height: 160px;
  }

  .speaker-name {
    font-size: 22px;
  }
}

@media screen and (max-width: 680px) {
  .speaker-carousel-container {
    gap: 10px;
  }

  .carousel-arrow {
    width: 36px;
    height: 36px;
  }

  .card-base {
    aspect-ratio: 250 / 420;
  }

  .photo-container {
    height: 280px;
    width: calc(100% - 16px);
  }

  .info-section {
    height: 155px;
    padding: 15px;
  }

  .speaker-name {
    font-size: 20px;
  }

  .speaker-name:has(br) {
    font-size: 16px;
  }

  .speaker-english-name {
    font-size: 11px;
  }

  .speaker-title {
    font-size: 14px;
  }

  .detail-btn {
    font-size: 13px;
    padding: 2px 20px;
  }

  .gold-border {
    top: 8px;
    left: 8px;
    right: 8px;
    bottom: 8px;
    border-width: 4px;
  }
}

@media screen and (max-width: 480px) {
  .speaker-carousel-container {
    gap: 5px;
  }

  .carousel-arrow {
    width: 30px;
    height: 30px;
  }

  .card-base {
    aspect-ratio: 250 / 400;
  }

  .photo-container {
    height: 250px;
    width: calc(100% - 12px);
  }

  .info-section {
    height: 145px;
  }

  .speaker-name {
    font-size: 18px;
  }

  .speaker-name:has(br) {
    font-size: 14px;
  }

  .speaker-title {
    font-size: 13px;
  }

  .detail-btn {
    padding:2px 18px;
  }

  .gold-border {
    top: 6px;
    left: 6px;
    right: 6px;
    bottom: 6px;
    border-width: 3px;
  }
}
</style>