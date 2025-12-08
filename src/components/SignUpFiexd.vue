<template>
  <img 
    src="@/assets/image/link.png" 
    class="scroll_to_form_btn" 
    @click="scrollToSection"
    alt="立即報名"
  >
</template>

<script setup>
const easeInOutCubic = (t) => {
  return t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1;
};

const scrollToSection = () => {
  const targetElement = document.querySelector('#sec4');
  
  if (!targetElement) {
    console.error('找不到 #sec4 元素');
    return;
  }

  const header = document.querySelector('#HEADER');
  const headerHeight = header ? header.offsetHeight : 80;
  
  const targetRect = targetElement.getBoundingClientRect();
  const startPosition = window.pageYOffset;
  const targetPosition = targetRect.top + startPosition - headerHeight;
  const distance = targetPosition - startPosition;
  const duration = 1500;
  let startTime = null;

  const animation = (currentTime) => {
    if (startTime === null) startTime = currentTime;
    const timeElapsed = currentTime - startTime;
    const progress = Math.min(timeElapsed / duration, 1);
    const ease = easeInOutCubic(progress);
    
    window.scrollTo(0, startPosition + distance * ease);
    
    if (timeElapsed < duration) {
      requestAnimationFrame(animation);
    }
  };

  requestAnimationFrame(animation);
};
</script>

<style scoped>
.scroll_to_form_btn {
  position: fixed;
  width: 50px;
  right: 35px;
  bottom: 30%;
  cursor: pointer;
  z-index: 10;
  transition: opacity 0.3s ease;
}

.scroll_to_form_btn:hover {
  opacity: 0.5;
}

@media screen and (max-width: 768px) {
  .scroll_to_form_btn {
    width: 40px;
    right: 15px;
  }
}

@media screen and (max-width: 480px) {
  .scroll_to_form_btn {
    width: 32px;
    right: 10px;
  }
}
</style>