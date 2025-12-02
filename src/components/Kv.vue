<script setup>
import AOS from 'aos'
import 'aos/dist/aos.css'
import { getWindowWidth, getDivResize } from '../utils/utils'
import { ref, onMounted, onUnmounted } from 'vue'

const windowWidth = ref(0)
const setDivResize = ref({})

function handleResize() {
    windowWidth.value = getWindowWidth()
    setDivResize.value = getDivResize('.kv_area')
}

onMounted(() => {
    AOS.init({
        once: true,
        duration: 1000
    })
    handleResize()
    window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
})
</script>

<template>
    <div :class="['kv_area', { 'pc': windowWidth >= 1024 }, { 'mb': windowWidth < 1024 }]">
        <div class="kv_box" :style="windowWidth >= 1024 ? { width: setDivResize.width + 'px', height: setDivResize.height + 'px' } : {}">
            <div class="kv_text_content">
                <div class="kv_year_tag" data-aos="fade-right" data-aos-delay="400" data-aos-duration="1000">
                    2026年ETF策略論壇
                </div>
                
                <div class="kv_title_wrapper" data-aos="fade-right" data-aos-delay="600" data-aos-duration="1000">
                    <img class="kv_title" src="../assets/image/kv_title.svg" alt="title">
                    <img class="kv_titleb" src="../assets/image/kv_titleb.png" alt="titleb">
                </div>
                
                <img class="kv_title2" data-aos="fade-right" data-aos-delay="800" data-aos-duration="1000" src="../assets/image/kv_title2.svg" alt="title2">
            </div>
            
<img 
  class="kv_stairs" 
  src="../assets/image/kv_stairs.png" 
  alt="stairs visual"
  data-aos="fade-left" 
  data-aos-delay="500" 
  data-aos-duration="1000"
>        </div>
    </div>
</template>

<style scoped>
.kv_area {
    position: relative;
    display: block;
    width: 100%;
    height: calc(100vh - 80px);
    background: url('../assets/image/bg-blue1.jpg') center center no-repeat;
    background-size: cover;
    margin-top: 80px;
}

.kv_area.pc {
    min-height: 580px;
}

.kv_box {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: space-between;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    padding: 0 2%;
    max-width: 100%;
}

.kv_text_content {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 4;
    flex: 0 0 auto;
    bottom: 35%;
}

.kv_year_tag {
    background: linear-gradient(135deg, #439ed6 0%, #3d5f94 100%);
    color: white;
    padding: 8px 24px;
    border-radius: 4px;
    font-size: 18px;
    font-weight: 500;
    letter-spacing: 2px;
    margin-bottom: 2.5rem;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.kv_title_wrapper {
    position: relative;
    width: 38vw;
    max-width: 580px;
    min-width: 400px;
    margin-bottom: 2.5rem;
}

.kv_title {
    display: block;
    width: 100%;
    height: auto;
}

.kv_titleb {
    position: absolute;
    width: 10%;
    left: 39%;
    top: 2%;
    transform: translate(-50%, -50%);
    z-index: 2;
}

.kv_title2 {
    display: block;
    width: 32vw;
    max-width: 480px;
    min-width: 350px;
    height: auto;
}

.kv_stairs {
    position: relative;
    display: block;
    width: 40vw;
    max-width: 620px;
    flex: 0 0 auto;
    z-index: 2;
}

@media screen and (max-width: 1024px) {
    .kv_area {
        margin-top: 55px;
        height: auto;
        min-height: 100vh;
        background: url('../assets/image/bg-blue1.jpg') center center no-repeat;
        background-size: cover;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .kv_area.mb {
        min-height: 100vh;
    }

    .kv_box {
        position: relative;
        top: auto;
        bottom: auto;
        width: 100% !important;
        height: auto !important;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 60px 5%;
    }

    .kv_text_content {
        position: relative;
        bottom: 0;
        align-items: center;
        max-width: 100%;
        padding: 0;
        margin-bottom: 60px;
    }

    .kv_year_tag {
        font-size: 16px;
        padding: 8px 22px;
        letter-spacing: 1.5px;
        margin-bottom: 2rem;
    }

    .kv_title_wrapper {
        width: 70vw;
        max-width: 550px;
        margin-bottom: 2rem;
    }

    .kv_title2 {
        width: 65vw;
        max-width: 520px;
    }

    .kv_stairs {
        position: relative;
        width: 70%;
        max-width: 500px;
        margin: 0 auto;
    }
}

@media screen and (max-width: 768px) {
    .kv_box {
        padding: 40px 5%;
    }

    .kv_text_content {
        margin-bottom: 50px;
    }

    .kv_year_tag {
        font-size: 15px;
        padding: 7px 20px;
        letter-spacing: 1px;
        margin-bottom: 1.8rem;
    }

    .kv_title_wrapper {
        width: 75vw;
        max-width: 450px;
        margin-bottom: 1.8rem;
    }

    .kv_title2 {
        width: 70vw;
        max-width: 430px;
    }

    .kv_stairs {
        width: 75%;
        max-width: 420px;
    }
}

@media screen and (max-width: 480px) {
    .kv_box {
        padding: 30px 5%;
    }

    .kv_text_content {
        margin-bottom: 40px;
    }

    .kv_year_tag {
        font-size: 13px;
        padding: 6px 18px;
        letter-spacing: 0.5px;
        margin-bottom: 1.5rem;
    }

    .kv_title_wrapper {
        width: 80vw;
        max-width: 350px;
        margin-bottom: 1.5rem;
    }

    .kv_title2 {
        width: 78vw;
        max-width: 330px;
    }

    .kv_stairs {
        width: 80%;
        max-width: 360px;
    }
}
</style>