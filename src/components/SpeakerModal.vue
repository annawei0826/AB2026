<!-- SpeakerModal -->
<template>
  <div id="host_info_box" :class="{ show: isOpen }" @click="close">
    <div class="ct">
      <div class="host_info_item" @click.stop>
        <img src="@/assets/image/button-close.png" class="close_box" @click="close">
<div class="box_name" v-html="speaker?.name"></div>
        <p class="box_intro" id="host_intro" v-html="speaker?.desc"></p>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  isOpen: Boolean,
  speaker: Object
});

const emit = defineEmits(['close']);

const close = () => {
  emit('close');
};
</script>

<style scoped>
.host_info_item .box_name {
  float: left;
  width: 100%;
  font-size: 20px;
  color: #003779;
  text-align: left;
  border-bottom: #2f7a7a 1px solid;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  padding-bottom: 5px;
  margin-bottom: 5px;
}


.host_info_item .box_intro {
  float: left;
  width: 100%;
  font-size: 16px;
  color: #000000;
  text-align: left;
  margin-bottom: 10px;
  max-height: 400px;
  overflow-y: hidden;
}

.host_info_item .box_title {
  float: left;
  font-size: 16px;
  text-align: left;
  font-weight: bold;
  width: 100%;
}
</style>
