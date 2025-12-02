<template>
  <div class="sec relatedarticles-section" id="sec6">
    <div class="mid_content" data-aos="fade-up">
      <div class="article_title_wrap">
        <div class="article_title">
          延伸閱讀
        </div>
      </div>

      <div class="article-carousel-container">
        <button class="article-arrow article-arrow-left" @click="swiperPrev">
          <img src="@/assets/image/arrow_left.png" alt="上一張">
        </button>

        <div class="article-carousel-wrapper">
          <swiper
            :modules="modules"
            :slides-per-view="1"
            :space-between="20"
            :loop="true"
            :autoplay="{ delay: 4000, disableOnInteraction: false }"
            :speed="800"
            :breakpoints="{
              680: { slidesPerView: 2, spaceBetween: 20 },
              1024: { slidesPerView: 3, spaceBetween: 20 }
            }"
            @swiper="onSwiper"
            class="article-swiper"
          >
            <swiper-slide v-for="(item, index) in infoData.articles" :key="index">
              <div class="article_item">
                <img :src="getImageUrl(item.img)" 
                     class="cover"
                     :style="(index === 2 || index === 3 || index === 6) ? { objectPosition: '65% center' } : {}">
                <div class="content-wrapper">
                  <h2>{{ item.title }}</h2>
                  <p>{{ item.desc }}</p>
                  <a :href="item.link" target="_blank">閱讀更多</a>
                </div>
              </div>
            </swiper-slide>
          </swiper>
        </div>

        <button class="article-arrow article-arrow-right" @click="swiperNext">
          <img src="@/assets/image/arrow_right.png" alt="下一張">
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Autoplay, Navigation } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/navigation';

defineProps({
  infoData: {
    type: Object,
    default: () => ({})
  }
});

const modules = [Autoplay, Navigation];
const swiperInstance = ref(null);

const getImageUrl = (name) => {
  return new URL(`../assets/image/${name}`, import.meta.url).href
}

const onSwiper = (swiper) => {
  swiperInstance.value = swiper;
};

const swiperPrev = () => {
  swiperInstance.value?.slidePrev();
};

const swiperNext = () => {
  swiperInstance.value?.slideNext();
};
</script>

<style scoped>
.relatedarticles-section {
  background-image: url('@/assets/image/bg-blue1.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  padding: 100px 20px 100px 20px;
}

.star5 {
  position: absolute;
  z-index: 1;
  width: 64px;
  left: 0px;
  top: 0px;
}

.article-carousel-container {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px 0;
}

.article-carousel-wrapper {
  flex: 1;
  overflow: hidden;
}

.article-swiper {
  width: 100%;
  padding: 20px 0;
}

.article-arrow {
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

.article-arrow:hover {
  transform: scale(1.1);
}

.article-arrow img {
  width: 100%;
  height: 100%;
  display: block;
}

.article-arrow-left {
  order: -1;
}

.article-arrow-right {
  order: 1;
}

.article_item {
  width: 100%;
  max-width: 300px;
  height: 400px;
  margin: 0 auto 10px;
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.article_item .cover {
  width: 100%;
  height: 240px;
  object-fit: cover;
  flex-shrink: 0;
  border-radius: 8px 8px 0 0;
}

.article_item .content-wrapper {
  flex: 1;
  background: linear-gradient(135deg, #F4E4C1 0%, #E6D4A8 33%, #C9A961 66%, #8B7355 100%);
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.article_item h2 {
  text-align: left;
  width: 100%;
  font-size: 20px;
  color: #7e603e;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  margin: 0;
  font-weight: 700;
}

.article_item p {
  text-align: left;
  width: 100%;
  font-size: 14px;
  color: #000000;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  box-sizing: border-box;
  margin: 0;
  flex: 1;
  line-height: 1.5;
  max-height: 42px;
}

.article_item a {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  width: 100%;
  max-width: 200px;
  height: 32px;
  color: #ffffff;
  font-weight: 700;
  background-color: #7e603e;
  border-radius: 16px;
  text-decoration: none;
  align-self: center;
  text-align: center;
  padding: 0 10px;
  margin-top: auto;
}

.article_item a:hover {
  -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=50)";
  filter: alpha(opacity=50);
  -moz-opacity: 0.5;
  opacity: 0.5;
  -moz-transition: ease-in-out 0.3s;
  -o-transition: ease-in-out 0.3s;
  -webkit-transition: ease-in-out 0.3s;
  transition: ease-in-out 0.3s;
}

@media screen and (max-width: 1180px) {
  .star5 {
    display: none;
  }
}

@media screen and (max-width: 900px) {
  .article-arrow {
    width: 40px;
    height: 40px;
  }
  
  .article-carousel-container {
    gap: 15px;
  }
}

@media screen and (max-width: 680px) {
  .article_title {
    font-size: 40px;
  }

  .article-carousel-container {
    gap: 10px;
  }

  .article-arrow {
    width: 36px;
    height: 36px;
  }

  .article_item {
    max-width: 280px;
    height: 380px;
  }

  .article_item .cover {
    height: 220px;
  }

  .article_item .content-wrapper {
    padding: 15px;
  }

  .article_item h2 {
    font-size: 18px;
  }

  .article_item p {
    font-size: 13px;
    line-height: 1.5;
    max-height: 39px;
  }

  .article_item a {
    max-width: 180px;
    font-size: 15px;
    height: 38px;
  }
}

@media screen and (max-width: 480px) {
  .article-carousel-container {
    gap: 5px;
  }

  .article-arrow {
    width: 30px;
    height: 30px;
  }

  .article_item {
    max-width: 260px;
    height: 360px;
  }

  .article_item .cover {
    height: 200px;
  }

  .article_item .content-wrapper {
    padding: 12px;
    gap: 8px;
  }

  .article_item h2 {
    font-size: 16px;
    margin: 0;
  }

  .article_item p {
    font-size: 12px;
    line-height: 1.5;
    max-height: 36px;
  }

  .article_item a {
    max-width: 160px;
    font-size: 14px;
    height: 36px;
  }
}
</style>