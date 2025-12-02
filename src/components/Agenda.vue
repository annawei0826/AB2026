<template>
  <div class="sec agenda-section" id="sec3">
    <div class="mid_content" data-aos="fade-up">
      <div class="article_title_wrap">
        <div class="article_title">
          {{ infoData.title }}
        </div>
      </div>

      <div class="agenda-cards-wrapper">
        <div class="agenda-cards">
          <div class="agenda-card" v-for="(topic, index) in localTopics" :key="index">
            <div class="card-number">{{ index + 1 }}</div>
            <h3 class="card-title" v-html="topic.title"></h3>
            <p class="card-desc">{{ topic.desc }}</p>
          </div>
        </div>
      </div>

      <div class="centerBox">
        <div class="agenda_btns">
          <button 
            v-for="(city, index) in cities" 
            :key="index"
            class="btn agenda_btn" 
            :class="{ active: activeTab === index }"
            @click="slideTo(index)"
          >
            <img src="@/assets/image/icon-map.png" alt="地點圖示" class="map-icon">
            <span class="city-name">{{ city.name }}</span>
          </button>
        </div>
      </div>

      <div class="agenda-carousel-container">
        <button class="agenda-arrow agenda-arrow-left" @click="swiperPrev">
          <img src="@/assets/image/arrow_left.png" alt="上一張">
        </button>

        <div class="agenda-carousel-wrapper">
          <div class="current-city-title">
            <h3>{{ cities[activeTab]?.name || '' }}</h3>
          </div>

          <swiper
            :modules="modules"
            :slides-per-view="1"
            :space-between="20"
            :loop="true"
            :autoplay="{ delay: 4000, disableOnInteraction: false }"
            :speed="1200"
            :effect="'slide'"
            @swiper="onSwiper"
            @slideChange="onSlideChange"
            class="agenda-swiper"
          >
            <swiper-slide v-for="(city, index) in infoData.cities" :key="index">
              <div class="centerBox">
                <div class="agenda_slider_info">
                    <div class="agenda_info_line1">
                      <span class="agenda_info_date">{{ cityEventInfo[index].date }}</span>
                      <div class="agenda_info_time_group">
                        <span class="agenda_info_time">{{ cityEventInfo[index].time }}</span>
                        <span class="agenda_info_remark" v-if="cityEventInfo[index].remark">{{ cityEventInfo[index].remark }}</span>
                      </div>
                    </div>
                    
                    <div class="agenda_info_line2">
                      <span class="agenda_info_venue">{{ cityEventInfo[index].venue }}</span>
                      <span class="agenda_info_room">{{ cityEventInfo[index].room }}</span>
                    </div>
                </div>
              </div>
              <div class="agenda_content">
                <div class="agenda">
                  <div class="agenda_tr" v-for="(row, rIndex) in city.schedule" :key="rIndex">
                    <div class="agenda-td">{{ row.time }}</div>
                    <div class="agenda-td" :class="{ orange: row.isOrange }">
                      <div v-html="row.content"></div>
                      <span v-if="row.speaker" v-html="row.speaker"></span>
                    </div>
                  </div>
                </div>
              </div>
            </swiper-slide>
          </swiper>
        </div>

        <button class="agenda-arrow agenda-arrow-right" @click="swiperNext">
          <img src="@/assets/image/arrow_right.png" alt="下一張">
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, computed } from 'vue';
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
const localTopics = ref([]);

const cities = ref([
  { name: '台中場', key: 'taichung' },
  { name: '台北場', key: 'taipei' },
  { name: '高雄場', key: 'kaohsiung' }
]);

const cityEventInfo = computed(() => {
    if (!props.infoData.cities || props.infoData.cities.length < 3) {
        return [
            { date: 'N/A', time: 'N/A', venue: 'N/A', room: 'N/A' },
            { date: 'N/A', time: 'N/A', venue: 'N/A', room: 'N/A' },
            { date: 'N/A', time: 'N/A', venue: 'N/A', room: 'N/A' }
        ];
    }

    const rawData = props.infoData.cities.map(c => c.eventInfo);

    const kaohsiungInfo = {
      ...rawData[2],
      time: "(二) 18:00 - 21:15"
    };

    return [
        rawData[0],
        rawData[1],
        kaohsiungInfo
    ];
});


watch(() => props.infoData.topics, (newTopics) => {
  if (newTopics) {
    localTopics.value = JSON.parse(JSON.stringify(newTopics));
  }
}, { immediate: true });

const getImageUrl = (name) => {
  return new URL(`../assets/image/${name}`, import.meta.url).href
}

const onSwiper = (swiper) => {
  swiperInstance.value = swiper;
};

const onSlideChange = (swiper) => {
  activeTab.value = swiper.realIndex;
};

const slideTo = (index) => {
  swiperInstance.value?.slideToLoop(index);
};

const swiperPrev = () => {
  swiperInstance.value?.slidePrev();
};

const swiperNext = () => {
  swiperInstance.value?.slideNext();
};
</script>

<style scoped>
.agenda-section {
  background-image: url('@/assets/image/bg-blue2.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
}

.agenda-section .mid_content {
  max-width: 100%;
  padding: 0;
}

.agenda-section .agenda-cards-wrapper {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 40px;
  box-sizing: border-box;
}

.agenda-section .agenda-cards-wrapper .agenda-cards {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  width: 100%;
  max-width: none;
  margin: 0 0 60px 0;
  align-items: stretch;
}

.agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: none;
  min-width: 0;
  min-height: 100%;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 4px;
  overflow: visible;
  padding: 32px 28px;
  text-align: left;
  transition: all 0.3s ease;
  cursor: pointer;
  box-sizing: border-box;
}

.agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card:nth-child(3),
.agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card:nth-child(4) {
  grid-column: span 1;
}

.agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card:hover {
  background: #f4e692;
}

.card-number {
  width: 50px;
  height: 50px;
  background: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  font-weight: 700;
  color: #b28922;
  margin-bottom: 20px;
  flex-shrink: 0;
}

.card-title {
  font-size: 18px;
  font-weight: 700;
  color: white;
  line-height: 1.5;
  margin: 0 0 16px 0;
  transition: color 0.3s ease;
}

.agenda-card:hover .card-title {
  color: #84603f;
}

.card-desc {
  font-size: 14px;
  color: white;
  line-height: 1.7;
  margin: 0;
  transition: color 0.3s ease;
}

.agenda-card:hover .card-desc {
  color: #000;
}

.centerBox {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.agenda_btns {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
  margin-bottom: 0px;
  flex-wrap: wrap;
}

.agenda_btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 14px 28px;
  background: transparent;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
  opacity: 0.4;
}

.agenda_btn:hover {
  opacity: 0.7;
  transform: translateY(-2px);
}

.agenda_btn.active {
  opacity: 1;
}

.map-icon {
  width: 40px;
  height: 40px;
  flex-shrink: 0;
  transition: all 0.3s ease;
  display: block;
}

.city-name {
  font-size: 20px;
  font-weight: 700;
  color: #f4e697;
  white-space: nowrap;
  transition: all 0.3s ease;
  line-height: 1;
}

.current-city-title {
  text-align: center;
  margin-bottom: 20px;
  padding: 16px 0;
}

.current-city-title h3 {
  font-size: 36px;
  font-weight: 700;
  color: #f4e697;
  margin: 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.agenda-section .agenda_tr .agenda-td {
  color: #000;
}

.agenda-section .agenda_tr .agenda-td.orange {
  color: #000;
}

.agenda-section .agenda_tr .agenda-td span {
  font-weight: 700;
  color: #b28922;
}

.agenda-carousel-container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px 0;
}

.agenda-carousel-wrapper {
  flex: 1;
  overflow: hidden;
}

.agenda-swiper {
  width: 100%;
}

.agenda-arrow {
  flex-shrink: 0;
  width: 48px;
  height: 48px;
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  transition: transform 0.3s ease;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.agenda-arrow:hover {
  transform: scale(1.1);
}

.agenda-arrow img {
  width: 100%;
  height: 100%;
  display: block;
}

.agenda-arrow-left {
  order: -1;
}

.agenda-arrow-right {
  order: 1;
}

.agenda_slider_info {
    padding: 20px 0;
    text-align: center;
    background-color: transparent;
    min-height: 80px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.agenda_info_line1 {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom:20px; 
    gap: 15px;
}

.agenda_info_time_group {
    display: flex;
    align-items: center;
    gap: 0px;
}

.agenda_info_line2 {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
}

.agenda_info_date {
    color: #f4e697;
    font-size: 36px;
    font-weight: 700;
    white-space: nowrap;
    line-height: 1;
}

.agenda_info_time {
    color: #ffffff;
    font-size: 20px; 
    font-weight: 400;
    white-space: nowrap;
    line-height: 1;
}

.agenda_info_remark {
    color: #ffffff;
    font-size: 14px;
    font-weight: 400;
    white-space: nowrap;
    line-height: 1;
}

.agenda_info_venue {
    color: #f4e697;
    font-size: 24px;
    font-weight: 700;
    white-space: nowrap;
    line-height: 1;
}

.agenda_info_room {
    color: #ffffff;
    font-size: 20px;
    font-weight: 400;
    white-space: nowrap;
    line-height: 1;
}

@media screen and (max-width: 1024px) {
    .agenda_info_date { font-size: 32px; }
    .agenda_info_venue { font-size: 22px; }
    .agenda_info_time, .agenda_info_room { font-size: 18px; }
    .agenda_info_remark { font-size: 13px; }

    .agenda-cards-wrapper {
        max-width: 900px;
        padding: 0 30px;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards {
        gap: 20px !important;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card {
        padding: 28px 24px !important;
    }

    .card-number {
        width: 45px;
        height: 45px;
        font-size: 22px;
        margin-bottom: 16px;
    }

    .card-title {
        font-size: 17px;
        margin-bottom: 14px;
    }

    .card-desc {
        font-size: 13px;
    }

    .current-city-title h3 {
        font-size: 32px;
    }
}

@media screen and (max-width: 900px) {
    .agenda-section .agenda-cards-wrapper .agenda-cards {
        grid-template-columns: 1fr;
        gap: 16px;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card:nth-child(3),
    .agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card:nth-child(4) {
        grid-column: span 1;
    }

    .agenda-arrow {
        width: 40px;
        height: 40px;
    }
    
    .agenda-carousel-container {
        gap: 15px;
    }

    .agenda_btns {
        gap: 15px;
    }

    .agenda_btn {
        padding: 12px 24px;
    }

    .map-icon {
        width: 22px;
        height: 22px;
    }

    .city-name {
        font-size: 17px;
    }

    .current-city-title h3 {
        font-size: 28px;
    }

    .agenda_info_date { font-size: 28px; }
    .agenda_info_venue { font-size: 20px; }
    .agenda_info_time, .agenda_info_room { font-size: 16px; }
    .agenda_info_remark { font-size: 12px; }
}

@media screen and (max-width: 768px) {
    .agenda-section .agenda-cards-wrapper {
        max-width: 600px;
        padding: 0 20px;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards {
        grid-template-columns: 1fr;
        gap: 16px;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card {
        padding: 24px 20px;
    }

    .card-number {
        width: 42px;
        height: 42px;
        font-size: 20px;
        margin-bottom: 14px;
    }

    .card-title {
        font-size: 16px;
        margin-bottom: 12px;
    }

    .card-desc {
        font-size: 13px;
        line-height: 1.6;
    }

    .current-city-title h3 {
        font-size: 24px;
    }

    .agenda_info_date { font-size: 24px; }
    .agenda_info_venue { font-size: 18px; }
    .agenda_info_time, .agenda_info_room { font-size: 14px; }
    .agenda_info_line1, .agenda_info_line2 {
        gap: 10px;
    }
}

@media screen and (max-width: 680px) {
    .agenda-carousel-container {
        gap: 10px;
    }

    .agenda-arrow {
        width: 36px;
        height: 36px;
    }

    .agenda_btns {
        gap: 12px;
    }

    .agenda_btn {
        padding: 10px 20px;
    }

    .map-icon {
        width: 20px;
        height: 20px;
    }

    .city-name {
        font-size: 16px;
    }

    .current-city-title h3 {
        font-size: 22px;
    }
}

@media screen and (max-width: 480px) {
    .agenda-section .agenda-cards-wrapper {
        padding: 0 20px;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards {
        gap: 14px;
    }

    .agenda-section .agenda-cards-wrapper .agenda-cards .agenda-card {
        padding: 20px 18px;
    }

    .card-number {
        width: 38px;
        height: 38px;
        font-size: 18px;
        margin-bottom: 12px;
    }

    .card-title {
        font-size: 15px;
        margin-bottom: 10px;
    }

    .card-desc {
        font-size: 12px;
    }

    .agenda-carousel-container {
        gap: 5px;
    }

    .agenda-arrow {
        width: 30px;
        height: 30px;
    }

    .agenda_btns {
        gap: 10px;
        flex-direction: column;
        width: 100%;
        max-width: 280px;
        margin: 0 auto;
    }

    .agenda_btn {
        width: 100%;
        justify-content: center;
        padding: 10px 16px;
    }

    .current-city-title {
        margin-bottom: 15px;
        padding: 12px 0;
    }

    .current-city-title h3 {
        font-size: 18px;
    }

    .agenda_info_date { font-size: 22px; }
    .agenda_info_venue { font-size: 16px; }
    .agenda_info_time, .agenda_info_room { font-size: 13px; }
    .agenda_info_line1, .agenda_info_line2 {
        gap: 8px;
    }
}

.agenda_tr:last-child .agenda-td {
  border-bottom: none;
}
</style>