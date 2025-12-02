<!-- Intro -->
<template>
    <div class="intro-section-wrapper" id="sec1">
        <div class="intro-bg-image">
            <img src="@/assets/image/bg-blue1.jpg" alt="背景圖片">
        </div>
        
        <div class="intro-content-container">
            <div class="intro-inner-content" data-aos="fade-up">
                
                <div class="intro-header">
                    <div class="intro-star">
                        <img src="@/assets/image/Intro_star.png" alt="裝飾星星">
                    </div>
                    <div class="intro-title-image">
                        <img src="@/assets/image/Intro_title.png" alt="論壇介紹">
                    </div>
                </div>

                <div class="intro-text-block intro-text-block-1">
                    <p>在全民瘋股、資金狂追指數型ETF的年代，</p>
                    <p>主動式「非投等債」ETF站上市場聚光燈，成為下一波收益追尋者的焦點武器。</p>
                </div>
                
                <div class="intro-text-block intro-text-block-2">
                    <h3 class="intro-emphasis-title">
                        國際債市權威、頂尖投信團隊與實戰型理財講者
                    </h3>
                    
                    <p>從全球視角全面剖析，非投等債如何躍升為關鍵收益來源；</p>
                    <p>聯準會降息下，主動式策略如何穿越震盪、穩定提升收益。</p>
                    <p>本論壇將帶您搶先卡位下一階段「主動投資」的收益投資風口。</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

defineProps({
    infoData: {
        type: Object,
        default: () => ({})
    }
})
</script>

<style scoped>
.intro-section-wrapper,
.intro-section-wrapper * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.intro-section-wrapper {
    position: relative;
    width: 100%;
    background-color: #0d1a44;
    overflow: hidden;
    padding-top: 200px;
    padding-bottom: 220px;
}

.intro-bg-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 0;
}

.intro-bg-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center bottom;
    display: block;
}

.intro-content-container {
    position: relative;
    z-index: 2;
    width: 100%;
    max-width: 1200px;
    margin-left: auto;
    margin-right: auto;
    padding-left: 20px;
    padding-right: 20px;
}

.intro-inner-content {
    width: 100%;
    max-width: 900px;
    margin-left: auto;
    margin-right: auto;
}

.intro-header {
    width: 100%;
    text-align: center;
    margin-bottom: 40px;
}

.intro-star {
    width: 150px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 35px;
}

.intro-star img {
    width: 100%;
    height: auto;
    display: block;
}

.intro-title-image {
    width: 100%;
    max-width: 400px;
    margin-left: auto;
    margin-right: auto;
}

.intro-title-image img {
    width: 100%;
    height: auto;
    display: block;
}

.intro-text-block {
    width: 100%;
    text-align: center;
}

.intro-text-block-1 {
    margin-bottom: 50px;
}

.intro-text-block p {
    font-size: 18px;
    color: #ffffff;
    line-height: 1.8;
    margin-bottom: 8px;
    text-align: center;
}

.intro-emphasis-title {
    font-size: 32px;
    color: #ffffff;
    font-weight: 700;
    line-height: 1.5;
    margin-bottom: 10px;
    text-align: center;
    transform: skewX(-10deg);
    display: inline-block;
}

@media screen and (max-width: 1024px) {
    .intro-section-wrapper {
        padding-top: 80px;
        padding-bottom: 80px;
    }
    
    .intro-star {
        width: 130px;
        margin-bottom: 12px;
    }
    
    .intro-title-image {
        max-width: 350px;
    }
    
    .intro-text-block p {
        font-size: 17px;
    }
    
    .intro-emphasis-title {
        font-size: 28px;
        transform: skewX(-8deg);
    }
}

@media screen and (max-width: 768px) {
    .intro-section-wrapper {
        padding-top: 60px;
        padding-bottom: 60px;
    }
    
    .intro-header {
        margin-bottom: 35px;
    }
    
    .intro-star {
        width: 110px;
        margin-bottom: 15px;
    }
    
    .intro-title-image {
        max-width: 300px;
    }
    
    .intro-text-block-1 {
        margin-bottom: 40px;
    }
    
    .intro-text-block p {
        font-size: 16px;
        line-height: 1.7;
    }
    
    .intro-emphasis-title {
        font-size: 24px;
        transform: skewX(-5deg);
        max-width: 90%;
        margin-left: auto;
        margin-right: auto;
    }
}

@media screen and (max-width: 480px) {
    .intro-section-wrapper {
        padding-top: 40px;
        padding-bottom: 40px;
    }
    
    .intro-content-container {
        padding-left: 15px;
        padding-right: 15px;
    }
    
    .intro-header {
        margin-bottom: 30px;
    }
    
    .intro-star {
        width: 90px;
        margin-bottom: 18px;
    }
    
    .intro-title-image {
        max-width: 250px;
    }
    
    .intro-text-block-1 {
        margin-bottom: 35px;
    }
    
    .intro-text-block p {
        font-size: 14px;
        line-height: 1.7;
        margin-bottom: 6px;
    }
    
    .intro-emphasis-title {
        font-size: 20px;
        transform: none;
        font-weight: 600;
        margin-bottom: 20px;
    }
}

@media screen and (max-width: 375px) {
    .intro-section-wrapper {
        padding-top: 35px;
        padding-bottom: 35px;
    }
    
    .intro-star {
        width: 80px;
        margin-bottom: 20px;
    }
    
    .intro-title-image {
        max-width: 220px;
    }
    
    .intro-text-block p {
        font-size: 13px;
    }
    
    .intro-emphasis-title {
        font-size: 18px;
    }
}
</style>