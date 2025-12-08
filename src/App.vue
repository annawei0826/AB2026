<template>
  <div>
    <!-- Preloader -->
    <!-- <div id="preloader" v-if="isLoading">
      <div id="status">&nbsp;</div>
    </div> -->

    <div id="fixedBackground">
      <img :src="bgBlue2" alt="background">
    </div>

    <Header :menu="infoData.menu" :isNavOpen="isNavOpen" @toggle-nav="toggleNav" @scroll-to="scrollTo" />
    
    <section id="KV" class="relative w-full overflow-hidden">
      <Kv />
    </section>
    <section id="intro" class="relative w-full overflow-hidden">
      <Intro :infoData="infoData.content[0]" />
    </section>
    <section id="speaker" class="relative w-full overflow-hidden">
      <Speaker :speakers="speakers.data" :infoData="infoData.content[1]" @open-modal="openModal" />
    </section>
    <section id="agenda" class="relative w-full overflow-hidden">
      <Agenda :infoData="infoData.content[2]" />
    </section>
    <section id="signUp" class="relative w-full overflow-hidden">
      <SignUp :infoData="infoData.content[3]" />
    </section>
    <section id="event" class="relative w-full overflow-hidden">
      <Event :infoData="infoData.content[4]" />
    </section>
    <section id="related" class="relative w-full overflow-hidden">
      <RelatedArticles :infoData="infoData.content[5]" />
    </section>
    <Footer :footerData="infoData.footer" />

    <!-- 當選單打開時隱藏這兩個按鈕 -->
    <SignUpFiexd v-show="!isNavOpen" />
    <Gototop v-show="!isNavOpen" />

    <SpeakerModal :isOpen="isModalOpen" :speaker="currentSpeaker" @close="closeModal" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Kv from './components/Kv.vue';
import Intro from './components/Intro.vue';
import Speaker from './components/Speaker.vue';
import Agenda from './components/Agenda.vue';
import SignUp from './components/SignUp.vue';
import Event from './components/Event.vue';
import RelatedArticles from './components/RelatedArticles.vue';
import Gototop from './components/Gototop.vue';
import SignUpFiexd from './components/SignUpFiexd.vue';
import SpeakerModal from './components/SpeakerModal.vue';

import infoData from './assets/json/info.json';
import speakers from './assets/json/speaker.json';
import bgBlue2 from '@/assets/image/bg-blue2.jpg';

import AOS from 'aos';
import 'aos/dist/aos.css';

const isLoading = ref(true);
const isNavOpen = ref(false);
const isModalOpen = ref(false);
const currentSpeaker = ref(null);

const toggleNav = () => {
  isNavOpen.value = !isNavOpen.value;
};

const scrollTo = (selector) => {
  const element = document.querySelector(selector);
  if (element) {
    // 關閉選單
    isNavOpen.value = false;
    
    // 獲取 Header 的高度
    const header = document.querySelector('#HEADER');
    const headerHeight = header ? header.offsetHeight : 80;
    
    // 計算目標位置，減去 Header 高度
    const elementTop = element.getBoundingClientRect().top;
    const targetPosition = elementTop + window.pageYOffset - headerHeight;
    const startPosition = window.pageYOffset;
    const distance = targetPosition - startPosition;
    
    // 如果距離很小，直接跳轉
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
      
      // easeInOutQuad - 更柔和的緩動
      const ease = progress < 0.5 
        ? 2 * progress * progress 
        : 1 - Math.pow(-2 * progress + 2, 2) / 2;
      
      const currentPosition = startPosition + distance * ease;
      window.scrollTo(0, currentPosition);
      
      if (progress < 1) {
        requestAnimationFrame(animation);
      } else {
        // 確保最後精確到達目標位置
        window.scrollTo(0, targetPosition);
      }
    }

    requestAnimationFrame(animation);
  }
};

const openModal = (speaker) => {
  currentSpeaker.value = speaker;
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

onMounted(() => {
  AOS.init({
    offset: 50,
    duration: 1000,
    once: false,
    disable: false
  });

  window.addEventListener('load', () => {
    setTimeout(() => {
      isLoading.value = false;
      document.body.style.overflow = 'visible';
    }, 350);
  });
  
  if (document.readyState === 'complete') {
    setTimeout(() => {
      isLoading.value = false;
      document.body.style.overflow = 'visible';
    }, 350);
  }
});
</script>

<style scoped>

section {
  margin: 0 ;
  padding: 0 ;
  display: block;
}

#KV{
  padding: 0 !important;
  margin: 0 ;
  background: transparent ;
}

#intro{
    padding: 0 !important;
    margin: 0 ;
    background: transparent ;
}

#speaker{
  padding: 0 ;
  margin: 0 ;
  background: transparent ;
}

#agenda{
  padding: 0 ;
  margin: 0 ;
  background: transparent ;
}

#signUp{
  padding: 0 ;
  margin: 0 ;
  background-color: transparent ;
}

#event{
  padding: 0 ;
  margin: 0 ;
  background: transparent ;
}

#related {
  padding: 0 ;
  margin: 0 ;
  background: transparent ;
}

@media screen and (max-width: 1024px) {
    #intro, #speaker, #agenda, #signUp, #event {
        padding: 0 ;
        margin: 0 ;
    }
}
</style>

<style>
html {
  scroll-behavior: auto;
  overflow-x: hidden;
  width: 100%;
}

body {
  margin: 0;
  padding: 0;
  background: transparent;
  overflow-x: hidden;
}

.relative {
  position: relative;
}

.w-full {
  width: 100%;
}

.overflow-hidden {
  overflow: hidden;
}
</style>