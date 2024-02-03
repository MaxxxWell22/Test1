<template>
  <div class="wrapper" @click="$emit('click')">
    <span style="color: #FFF;">{{ props.titles }}</span>
    <div class="d-flex flex-column ml-5" v-if="props.titles === props.activeTitle">
      <img src="../assets/arrowTwoToggle.svg" alt="svg" />
      <img src="../assets/arrowToggle.svg" alt="svg" />
    </div>
    <div v-else class="d-flex flex-column ml-5">
      <img src="../assets/arrowTwo.svg" alt="svg" />
      <img src="../assets/arrow.svg" alt="svg" />
    </div>
  </div>
</template>
<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  titles: {
    type: String,
  },
  toggleTheme: {
    type: Boolean
  },
  activeTitle: {
    type: String
  }
})
</script>

<style lang="scss" scoped>
.wrapper {
  display: flex;
  align-items: center;
}
</style>