<script setup>
import AOS from 'aos'
import 'aos/dist/aos.css'
import { getWindowWidth, getDivResize } from '../utils/utils'
import { ref, onMounted, onUnmounted } from 'vue'

const windowWidth = ref(0)
const setDivResize = ref({})
const isReady = ref(false)

function handleResize() {
    windowWidth.value = getWindowWidth()
    setDivResize.value = getDivResize('.kv_area')
}

onMounted(() => {
    handleResize()
    
    setTimeout(() => {
        isReady.value = true
        AOS.init({
            once: true,
            duration: 1000,
            offset: 0,
            anchorPlacement: 'top-center'
        })
    }, 100)
    
    window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
})
</script>

<template>
    <div :class="['kv_area', { 'pc': windowWidth >= 1024 }, { 'mb': windowWidth < 1024 }]">
        <div class="kv_box" :style="windowWidth >= 1024 ? { width: setDivResize.width + 'px', height: setDivResize.height + 'px' } : {}">
            <div class="kv_content_wrapper">
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
                
                <div class="kv_image_content">
                    <img 
                      class="kv_stairs" 
                      src="../assets/image/kv_stairs.png" 
                      alt="stairs visual"
                      data-aos="fade-left" 
                      data-aos-delay="500" 
                      data-aos-duration="1000"
                    >
                </div>
            </div>
        </div>
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
    overflow: hidden;
}


.kv_area.pc {
    min-height: 700px;
}

.kv_box {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    padding: 5% 3%;
}

.kv_content_wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    max-width: 100%;
    gap: 5%;
}

.kv_text_content {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    flex: 0 1 auto;
        margin-bottom: 25%;

}

.kv_image_content {
    display: flex;
    align-items: center;
    justify-content: center;
    flex: 0 1 auto;
}

.kv_year_tag {
    background: linear-gradient(135deg, #439ed6 0%, #3d5f94 100%);
    color: white;
    padding: 8px 24px;
    border-radius: 4px;
    font-size: 14px;
    font-weight: 500;
    letter-spacing: 1.5px;
    margin-bottom: 40px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    white-space: nowrap;
    flex-shrink: 0;
}

.kv_title_wrapper {
    position: relative;
    width:650px;        
    margin-bottom: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
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
    width:480px;       
    height: auto;
    flex-shrink: 0;
}

.kv_stairs {
    display: block;
    width: 600px;
    height: auto;
    flex-shrink: 0;
}

@media screen and (max-width: 1367px) {
    .kv_title_wrapper {
        width: 650px;
    }
    
    .kv_title2 {
        width: 480px;
    }
    
    .kv_stairs {
        width: 480px;
        margin-top: -15%;
    }
}

@media screen and (max-width: 1280px) {
    .kv_year_tag {
        font-size: 13px;
        padding: 7px 20px;
        margin-bottom: 35px;
    }
    
    .kv_title_wrapper {
        width: 500px;
        margin-bottom: 35px;
    }
    
    .kv_title2 {
        width: 400px;
    }
    
    .kv_stairs {
        width: 450px;
        margin-top:0;
    }
}

@media screen and (max-width: 1024px) {
    .kv_area {
        margin-top: 55px;
        height: auto;
        min-height: calc(100vh - 55px);
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .kv_box {
        position: static;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: auto;
        padding: 60px 5%;
        min-height: calc(100vh - 55px);
    }

    .kv_content_wrapper {
        flex-direction: column;
        gap: 50px;
        align-items: center;
        justify-content: center;
    }

    .kv_text_content {
        align-items: center;
        text-align: center;
        max-width: 90%;
        margin-bottom: 0;
    }

    .kv_image_content {
        max-width: 65%;
    }

    .kv_year_tag {
        font-size: 15px;
        padding: 7px 20px;
        margin-bottom: 30px;
        white-space: normal;
    }

    .kv_title_wrapper {
        width: 75vw;
        max-width: 500px;
        margin-bottom: 30px;
    }

    .kv_title2 {
        width: 70vw;
        max-width: 450px;
    }

    .kv_stairs {
        width: 100%;
        max-width: 420px;
        margin-top:0;
    }
}
@media screen and (min-width: 1025px) {
    .kv_area {
        background-attachment: fixed;
    }
}
@media screen and (max-width: 768px) {
    .kv_box {
        padding: 50px 5%;
    }

    .kv_content_wrapper {
        gap: 40px;
    }

    .kv_image_content {
        max-width: 70%;
    }

    .kv_year_tag {
        font-size: 14px;
        padding: 6px 18px;
        margin-bottom: 25px;
    }

    .kv_title_wrapper {
        width: 78vw;
        max-width: 420px;
        margin-bottom: 25px;
    }

    .kv_title2 {
        width: 73vw;
        max-width: 400px;
    }

    .kv_stairs {
        max-width: 380px;
        margin-top:0;
    }
}

@media screen and (max-width: 480px) {
    .kv_box {
        padding: 0 5% 40px 5%;
    }

    .kv_content_wrapper {
        gap: 35px;
    }

    .kv_image_content {
        max-width: 75%;
    }

    .kv_year_tag {
        font-size: 13px;
        padding: 6px 16px;
        margin-bottom: 20px;
    }

    .kv_title_wrapper {
        width: 82vw;
        max-width: 340px;
        margin-bottom: 20px;
    }

    .kv_title2 {
        width: 78vw;
        max-width: 320px;
    }

    .kv_stairs {
        max-width: 330px;
        margin-top:0;
    }
}

@media screen and (max-width: 360px) {
    .kv_year_tag {
        font-size: 12px;
        padding: 5px 14px;
    }

    .kv_title_wrapper {
        width: 85vw;
        max-width: 300px;
    }

    .kv_title2 {
        width: 82vw;
        max-width: 280px;
    }

    .kv_stairs {
        max-width: 300px;
        margin-top:0;
    }
}
</style>