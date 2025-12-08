<template>
  <div class="sec event-section" id="sec5">
    <div class="mid_content" data-aos="fade-up">
      <div class="article_title_wrap" ref="titleContainer">
        <div class="article_title">
          {{ infoData.title }}
        </div>
      </div>

      <div class="centerBox">
        <div class="event_btns">
          <button 
            v-for="(city, index) in cities" 
            :key="index"
            class="btn event_btn" 
            :class="{ active: activeTab === index }"
            @click="handleCityClick(index)"
          >
            <img src="@/assets/image/icon-map.png" alt="地點圖示" class="map-icon">
            <span class="city-name">{{ city.name }}</span>
          </button>
        </div>
      </div>

      <div class="event-carousel-container">
        <button class="event-arrow event-arrow-left" @click="swiperPrev">
          <img src="@/assets/image/arrow_left.png" alt="上一張">
        </button>

        <div class="event-carousel-wrapper">
          <swiper
            :modules="modules"
            :slides-per-view="1"
            :space-between="20"
            :loop="true"
            :autoplay="{ delay:8000, disableOnInteraction: false }"
            :speed="1200"
            :effect="'slide'"
            @swiper="onSwiper"
            @slideChange="onSlideChange"
            class="event-swiper"
          >
            <swiper-slide v-for="(venue, index) in sortedVenues" :key="index">
              <div class="venue-content">

                <div class="traffic_body">
                  <div class="map_wrap">
                    <div class="map-responsive">
                      <iframe
                        :src="venue.mapUrl"
                        width="100%" 
                        height="450" 
                        style="border:0; border-radius: 10px; display: block;"
                        allowfullscreen="" 
                        loading="lazy"
                        referrerpolicy="no-referrer-when-downgrade"
                      ></iframe>
                    </div>
                  </div>

                  <div class="centerBox">
                    <div class="traffic_item_wrap">
                      <div 
                        v-for="(transport, tIndex) in venue.transportation" 
                        :key="tIndex"
                        class="traffic_item"
                      >
                        <div class="traffic_icon">
                          <img :src="getImageUrl(transport.icon)" />
                        </div>
                        <div class="traffic_des">
                          <p class="traffic_title">
                            {{ transport.type }}
                            <span v-if="transport.time" class="traffic_time">{{ transport.time }}</span>
                          </p>
                          <p class="traffic_txt" v-html="transport.description"></p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </swiper-slide>
          </swiper>
        </div>

        <button class="event-arrow event-arrow-right" @click="swiperNext">
          <img src="@/assets/image/arrow_right.png" alt="下一張">
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Navigation } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/navigation';

const props = defineProps({
  infoData: {
    type: Object,
    default: () => ({})
  }
});

const modules = [Autoplay, Navigation];
const swiperInstance = ref(null);
const activeTab = ref(0);
const titleContainer = ref(null);

const cities = ref([
  { name: '台中場', key: 'taichung' },
  { name: '台北場', key: 'taipei' },
  { name: '高雄場', key: 'kaohsiung' }
]);

const getImageUrl = (name) => {
  return new URL(`../assets/image/${name}`, import.meta.url).href;
};

const sortedVenues = computed(() => {
  if (!props.infoData.venues) return [];
  const venues = props.infoData.venues;
  
  const taichung = venues.find(v => v.name.includes('台中')) || venues[1];
  const taipei = venues.find(v => v.name.includes('台北')) || venues[0];
  const kaohsiung = venues.find(v => v.name.includes('高雄')) || venues[2];
  
  return [taichung, taipei, kaohsiung].filter(Boolean);
});

const onSwiper = (swiper) => {
  swiperInstance.value = swiper;
};

const onSlideChange = (swiper) => {
  activeTab.value = swiper.realIndex;
};

const slideTo = (index) => {
  swiperInstance.value?.slideToLoop(index);
};

const handleCityClick = (index) => {
  slideTo(index);
  
  if (titleContainer.value) {
    const header = document.querySelector('#HEADER');
    const headerHeight = header ? header.offsetHeight : 80;
    
    const elementTop = titleContainer.value.getBoundingClientRect().top;
    const targetPosition = elementTop + window.pageYOffset - headerHeight;
    const startPosition = window.pageYOffset;
    const distance = targetPosition - startPosition;
    
    if (Math.abs(distance) < 10) {
      window.scrollTo(0, targetPosition);
      return;
    }
    
    const duration = 1000;
    let startTime = null;

    function animation(currentTime) {
      if (startTime === null) startTime = currentTime;
      const elapsed = currentTime - startTime;
      const progress = Math.min(elapsed / duration, 1);
      
      const ease = progress < 0.5
        ? 4 * progress * progress * progress
        : 1 - Math.pow(-2 * progress + 2, 3) / 2;
      
      const currentPosition = startPosition + distance * ease;
      window.scrollTo(0, currentPosition);
      
      if (progress < 1) {
        requestAnimationFrame(animation);
      } else {
        window.scrollTo(0, targetPosition);
      }
    }

    requestAnimationFrame(animation);
  }
};

const swiperPrev = () => {
  swiperInstance.value?.slidePrev();
};

const swiperNext = () => {
  swiperInstance.value?.slideNext();
};
</script>

<style scoped>
.event-section {
  background-image: url('@/assets/image/bg-blue2.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  display: flex;
  align-items: center;
  min-height: 100vh;
  margin-bottom: 5%;
}

.event-section .mid_content {
  width: 100%;
}

.event_btns {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 24px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.event_btn {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px 32px;
  background: transparent;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  opacity: 0.4;
}

.event_btn:hover {
  opacity: 0.7;
  transform: translateY(-2px);
}

.event_btn.active {
  opacity: 1;
}

.map-icon {
  width: 30px;
  height: 30px;
  flex-shrink: 0;
  transition: all 0.3s ease;
}

.city-name {
  font-size: 20px;
  font-weight: 700;
  color: #f4e697;
  white-space: nowrap;
  transition: all 0.3s ease;
}

.event-carousel-container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px 40px;
  box-sizing: border-box;
}

.event-carousel-wrapper {
  flex: 1;
  overflow: hidden;
}

.event-swiper {
  width: 100%;
}

.event-arrow {
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

.event-arrow:hover {
  transform: scale(1.1);
}

.event-arrow img {
  width: 100%;
  height: 100%;
  display: block;
}

.event-arrow-left {
  order: -1;
}

.event-arrow-right {
  order: 1;
}

.venue-content {
  width: 100%;
}

.traffic_body {
  width: 100%;
}

.map_wrap {
  width: 100%;
  position: relative;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 30px;
}

.map-responsive {
  border-radius: 10px;
  overflow: hidden;
  position: relative;
  width: 100%;
}

.map-responsive iframe {
  border-radius: 10px;
  display: block;
  width: 100%;
}

/* 交通資訊區域 */
.traffic_item_wrap {
  width: 100%;
  max-width: 715px;
  text-align: center;
  box-sizing: border-box;
  margin-top: 50px;
}

.traffic_item {
  display: table;
  width: 100%;
  margin-bottom: 20px;
}

.traffic_item:last-child {
  margin-bottom: 0;
}

.traffic_icon {
  display: table-cell;
  width: 30px;
  vertical-align: top;
}

.traffic_icon img {
  width: 100%;
}

.traffic_des {
  display: table-cell;
  vertical-align: top;
  box-sizing: border-box;
  padding-left: 15px;
  text-align: left;
}

.traffic_title {
  color: #f2e391;
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 10px;
  display: flex;
  align-items: baseline;
  gap: 8px;
}

.traffic_time {
  color: #fff;
  font-size: 16px;
  font-weight: normal;
}

.traffic_txt {
  color: #fff;
  font-size: 16px;
  line-height: 1.6;
}

.traffic_txt span {
  font-weight: bold;
}

@media screen and (max-width: 1024px) {
  .event-carousel-container {
    max-width: 900px;
  }
}

@media screen and (max-width: 900px) {
  .event_btns {
    gap: 18px;
    margin-bottom: 16px;
  }

  .event_btn {
    padding: 14px 28px;
  }

  .map-icon {
    width: 28px;
    height: 28px;
  }

  .city-name {
    font-size: 19px;
  }

  .event-arrow {
    width: 40px;
    height: 40px;
  }
  
  .event-carousel-container {
    gap: 15px;
  }

  .map-responsive iframe {
    height: 400px;
  }
}

@media screen and (max-width: 768px) {
  .event-section {
    min-height: auto;
    padding: 80px 0;
  }

  .event-carousel-container {
    max-width: 100%;
    padding: 15px 20px;
  }

  .map_wrap {
    margin-bottom: 10px;
  }

  .map-responsive iframe {
    height: 350px;
  }

  .traffic_title {
    font-size: 18px;
  }

  .traffic_time {
    font-size: 15px;
  }

  .traffic_txt {
    font-size: 15px;
  }
}

@media screen and (max-width: 680px) {
  .event_btns {
    gap: 12px;
  }

  .event_btn {
    padding: 10px 20px;
  }

  .map-icon {
    width: 26px;
    height: 26px;
  }

  .city-name {
    font-size: 16px;
  }

  .event-carousel-container {
    gap: 10px;
  }

  .event-arrow {
    width: 36px;
    height: 36px;
  }

  .map-responsive iframe {
    height: 300px;
  }

  .traffic_title {
    font-size: 17px;
    flex-wrap: wrap;
  }

  .traffic_time {
    font-size: 14px;
  }

  .traffic_txt {
    font-size: 14px;
  }
}

@media screen and (max-width: 480px) {
  .event-section {
    padding: 60px 0 60px 0;
  }

  .event_btns {
    gap: 8px;
    flex-direction: row;
    width: 100%;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 16px;
  }

  .event_btn {
    padding: 8px 16px;
    gap: 6px;
  }

  .map-icon {
    width: 18px;
    height: 18px;
  }

  .city-name {
    font-size: 14px;
  }

  .event-carousel-container {
    gap: 5px;
    padding: 15px 10px;
  }

  .event-arrow {
    width: 30px;
    height: 30px;
  }



  .map-responsive iframe {
    height: 280px;
  }

  .traffic_title {
    font-size: 16px;
    flex-direction: column;
    align-items: flex-start;
    gap: 4px;
  }

  .traffic_time {
    font-size: 14px;
  }

  .traffic_txt {
    font-size: 14px;
    line-height: 1.5;
  }
}

@media screen and (max-width: 375px) {

  .event_btns {
    gap: 6px;
  }

  .event_btn {
    padding: 7px 14px;
    gap: 5px;
  }

  .map-icon {
    width: 16px;
    height: 16px;
  }

  .city-name {
    font-size: 13px;
  }

  .map-responsive iframe {
    height: 250px;
  }
}
</style>