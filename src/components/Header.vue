<template>
  <div>
    <div id="HEADER" :class="{ reveal: isReveal }">
      <img 
        src="@/assets/image/kv_logo.png" 
        class="logo" 
        :class="{ 'logo-hidden': isNavOpen }"
        alt="Logo"
        @click="scrollToTop"
      >
      
      <div id="NAV" :class="{ reveal: isNavOpen }">
        <nav>
          <div 
            v-for="(item, index) in menu" 
            :key="index"
            class="scroll_btn" 
            @click="scrollTo(item.link)"
          >
            {{ item.name }}
          </div>
        </nav>
      </div>
      
      <div class="NAV_btn_wrap" @click="toggleNav">
        <div id="nav-icon3" class="light" :class="{ open: isNavOpen }">
          <span></span>
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

defineProps({
  menu: {
    type: Array,
    default: () => []
  },
  isNavOpen: Boolean
});

const emit = defineEmits(['toggle-nav', 'scroll-to']);

const isReveal = ref(false);

const toggleNav = () => {
  emit('toggle-nav');
};

const scrollTo = (selector) => {
  emit('scroll-to', selector);
};

const easeInOutCubic = (t) => {
  return t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1;
};

const scrollToTop = () => {
  const startPosition = window.pageYOffset;
  const distance = startPosition;
  const duration = 1500;
  let startTime = null;

  const animation = (currentTime) => {
    if (startTime === null) startTime = currentTime;
    const timeElapsed = currentTime - startTime;
    const progress = Math.min(timeElapsed / duration, 1);
    const ease = easeInOutCubic(progress);
    
    window.scrollTo(0, startPosition - distance * ease);
    
    if (timeElapsed < duration) {
      requestAnimationFrame(animation);
    }
  };

  requestAnimationFrame(animation);
};

const handleScroll = () => {
  if (window.scrollY > 500) {
    isReveal.value = true;
  } else {
    isReveal.value = false;
  }
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<style scoped>
/* ==================== HEADER 主要樣式 ==================== */
#HEADER {
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, #000000 0%, #142161 50%, #000000 100%);
  transition: background-color 0.3s ease;
  padding: 0 20px;
  box-sizing: border-box;
  min-height: 80px;
}

#HEADER.reveal {
  background: linear-gradient(90deg, #000000 0%, #142161 50%, #000000 100%);
  box-shadow: rgba(0, 0, 0, 0.1) 0px 1px 2px 0px;
}

/* ==================== LOGO 樣式 ==================== */
.logo {
  height: 50px;
  width: auto;
  position: relative;
  z-index: 13;
  cursor: pointer;
}

.logo.logo-hidden {
  opacity: 0;
  visibility: hidden;
}

/* ==================== 導覽列樣式 ==================== */
#NAV {
  display: flex;
  align-items: center;
  height: 100%;
  background: transparent;
  justify-content: flex-end;
}

nav {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 18px;
}

nav .scroll_btn {
  display: flex;
  align-items: center;
  padding: 0 20px;
  height: 60px;
  font-size: 20px;
  color: #fff;
  cursor: pointer;
  transition: ease-in-out 0.3s;
  white-space: nowrap;
}

nav .scroll_btn:hover {
  color: #f2e391;
}

/* ==================== 漢堡按鈕樣式 ==================== */
.NAV_btn_wrap {
  display: none;
}

#nav-icon3 {
  width: 30px;
  height: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#nav-icon3 span {
  display: block;
  position: absolute;
  height: 2px;
  width: 100%;
  background: #fff;
  border-radius: 2px;
  opacity: 1;
  left: 0;
  transform: rotate(0deg);
  transition: 0.25s ease-in-out;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.56);
}

#nav-icon3.light span {
  background: #fff;
}

#nav-icon3.open.light span {
  background: #000;
}

#nav-icon3 span:nth-child(1) {
  top: 4px;
}

#nav-icon3 span:nth-child(2),
#nav-icon3 span:nth-child(3) {
  top: 14px;
}

#nav-icon3 span:nth-child(4) {
  top: 24px;
}

#nav-icon3.open span:nth-child(1) {
  top: 14px;
  width: 0%;
  left: 50%;
}

#nav-icon3.open span:nth-child(2) {
  transform: rotate(45deg);
}

#nav-icon3.open span:nth-child(3) {
  transform: rotate(-45deg);
}

#nav-icon3.open span:nth-child(4) {
  top: 14px;
  width: 0%;
  left: 50%;
}

/* ==================== 響應式設計 - 1440px ==================== */
@media screen and (max-width: 1440px) {
  #HEADER {
    padding: 0 20px;
    min-height: 60px;
  }

  .logo {
    height: 40px;
  }

  .NAV_btn_wrap {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    cursor: pointer;
    z-index: 14;
  }

  #NAV {
    position: fixed;
    z-index: 12;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    background: #FDF4CC;
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    display: none;
    align-items: center;
    justify-content: center;
    transition: opacity 0.3s ease;
    overflow: hidden;
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  #NAV::-webkit-scrollbar {
    display: none;
  }

  #NAV.reveal {
    display: flex;
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
  }

  nav {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    max-width: 500px;
    min-height: 100%;
    padding: 0px 0 200px 0;
    box-sizing: border-box;
  }

  nav .scroll_btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    padding: 0 20px;
    height: 60px;
    font-size: 18px;
    color: #000;
    text-align: center;
    flex-shrink: 0;
  }
}

/* ==================== 響應式設計 - 768px ==================== */
@media screen and (max-width: 768px) {
  #HEADER {
    padding: 0 15px;
    min-height: 55px;
  }

  .logo {
    height: 35px;
  }
  
  .NAV_btn_wrap {
    width: 35px;
    height: 35px;
  }

  #nav-icon3 {
    width: 28px;
    height: 28px;
  }

  #nav-icon3 span:nth-child(1) {
    top: 3px;
  }

  #nav-icon3 span:nth-child(2),
  #nav-icon3 span:nth-child(3) {
    top: 13px;
  }

  #nav-icon3 span:nth-child(4) {
    top: 23px;
  }

  #nav-icon3.open span:nth-child(1),
  #nav-icon3.open span:nth-child(4) {
    top: 13px;
  }
}

/* ==================== 響應式設計 - 480px ==================== */
@media screen and (max-width: 480px) {
  #HEADER {
    padding: 0 10px;
    min-height: 50px;
  }

  .logo {
    height: 30px;
  }
  
  .NAV_btn_wrap {
    width: 30px;
    height: 30px;
  }

  #nav-icon3 {
    width: 25px;
    height: 25px;
  }

  #nav-icon3 span:nth-child(1) {
    top: 2px;
  }

  #nav-icon3 span:nth-child(2),
  #nav-icon3 span:nth-child(3) {
    top: 11px;
  }

  #nav-icon3 span:nth-child(4) {
    top: 20px;
  }

  #nav-icon3.open span:nth-child(1),
  #nav-icon3.open span:nth-child(4) {
    top: 11px;
  }

  nav .scroll_btn {
    font-size: 16px;
    height: 50px;
  }
}
</style>