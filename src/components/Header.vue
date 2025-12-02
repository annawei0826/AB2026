<template>
  <div>
    <div id="HEADER" :class="{ reveal: isReveal }">
      <img 
        src="@/assets/image/kv_logo.png" 
        class="logo" 
        :class="{ 'logo-hidden': isNavOpen }"
        alt="Logo"
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
#HEADER {
  position: fixed;
  width: 100%;
  top: 0px;
  left: 0px;
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, #000000 0%, #142161 50%, #000000 100%);
  transition: background-color 0.3s ease;
}

#HEADER.reveal {
  background: linear-gradient(90deg, #000000 0%, #142161 50%, #000000 100%);
  box-shadow: rgba(0, 0, 0, 0.1) 0px 1px 2px 0px;
}

.logo {
  height: 50px;
  width: auto;
  margin: 15px 60px;
  transition: opacity 0.3s ease, visibility 0.3s ease;
  position: relative;
  z-index: 13;
}

.logo.logo-hidden {
  opacity: 0;
  visibility: hidden;
}

#NAV {
  position: static;
  display: flex;
  align-items: center;
  height: 100%;
  box-sizing: border-box;
  margin-right: 60px;
  background: transparent;
  background-color: transparent;
  visibility: visible;
  opacity: 1;
}

nav {
  display: flex;
  align-items: center;
  height: 100%;
}

nav .scroll_btn {
  display: flex;
  align-items: center;
  outline: none;
  cursor: pointer;
  box-sizing: border-box;
  padding: 0 20px;
  height: 100%;
  font-size: 20px;
  color: #fff;
  white-space: nowrap;
}

nav .scroll_btn:hover {
  opacity: 1;
  transition: ease-in-out 0.3s;
  color: #f2e391;
  filter: none;
  -moz-opacity: 1;
}

.NAV_btn_wrap {
  display: none;
  position: fixed;
  width: 60px;
  top: 30px;
  right: 20px;
  z-index: 14;
}

#nav-icon3 {
  width: 30px;
  height: 45px;
  position: relative;
  margin: 0px auto;
  transform: rotate(0deg);
  transition: .5s ease-in-out;
  cursor: pointer;
}

#nav-icon3 span {
  display: block;
  position: absolute;
  height: 2px;
  width: 100%;
  border-radius: 2px;
  opacity: 1;
  left: 0;
  transform: rotate(0deg);
  transition: .25s ease-in-out;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.56);
}

#nav-icon3.light span {
  background: #fff;
}

#nav-icon3.open.light span {
  background: #000;
}

#nav-icon3 span:nth-child(1) { top: 0px; }
#nav-icon3 span:nth-child(2), #nav-icon3 span:nth-child(3) { top: 10px; }
#nav-icon3 span:nth-child(4) { top: 20px; }

#nav-icon3.open span:nth-child(1) {
  top: 18px;
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
  top: 18px;
  width: 0%;
  left: 50%;
}

@media screen and (max-width: 1440px) {
  #HEADER {
    justify-content: space-between;
    padding: 10px 0;
  }

  .logo {
    height: 40px;
    margin-left: 20px;
    margin-right: 0;
    margin-top: 10px;
    margin-bottom: 10px;
  }

  .NAV_btn_wrap {
    display: block;
  }

  #NAV {
    position: fixed;
    z-index: 12;
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    text-align: center;
    background: #FDF4CC;
    left: 0px;
    top: 0px;
    margin-right: 0;
    display: block;
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    transition: none;
  }
  
  #NAV.reveal {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
    transition: opacity 0.3s ease, visibility 0.3s ease;
  }

  nav {
    display: inline-block;
    width: 100%;
    height: auto;
    text-align: center;
    margin-top: 80px;
  }

  nav .scroll_btn {
    display: block;
    width: 100%;
    padding-left: 20px;
    padding-right: 20px;
    line-height: 60px;
    height: auto;
    text-align: center;
    font-size: 18px;
    color: #000;
  }
}

@media screen and (max-width: 768px) {
  .logo {
    height: 35px;
    margin-left: 15px;
    margin-top: 8px;
    margin-bottom: 8px;
  }
  
  .NAV_btn_wrap {
    top: 20px;
    right: 15px;
  }
}

@media screen and (max-width: 480px) {
  .logo {
    height: 30px;
    margin-left: 10px;
    margin-top: 5px;
    margin-bottom: 5px;
  }
  
  .NAV_btn_wrap {
    top: 15px;
    right: 10px;
  }
}
</style>