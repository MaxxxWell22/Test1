<template>
  <div class="dropdown-wrapper">
    <div :class="dropdownClass" @click="isDropDownVisible = !isDropDownVisible">
      <span class="placeholder" v-if="!selectedOption">{{ props.placeholder }}</span>
      <span v-else style="color: #29277d; padding-left: 5px;">{{ selectedOption }}</span>
      <div v-if="selectedOption" class="placeholder1">
        <span>{{ props.placeholder }}</span>
      </div>
      <svg :class="arrowClass" width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M3 6L8 11L13 6" stroke="#BBC3FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </div>
    <transition name="slide-fade">
      <div class="options-wrapper" v-if="isDropDownVisible">
        <div class="option" v-for="(option) in props.options" :key="option.id" @click="toggleOptionSelect(option)">
          {{ option.title }}
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { defineProps, ref, computed, defineEmits } from 'vue';

const props = defineProps({
  options: {
    type: Array,
    required: true
  },
  modelValue: {
    default: null
  },
  placeholder: {
    type: String,
    required: true,
    default: 'Выберите название животного'
  }
});

const emit = defineEmits(['update:modelValue']);

const selectedOption = ref(null);
const isDropDownVisible = ref(false);

const dropdownClass = computed(() => {
  return selectedOption.value !== null ? 'dropdown-selected-option' : 'dropdown-selected-optionNull';
});

const arrowClass = computed(() => {
  return isDropDownVisible.value ? 'arrow' : 'arrowNull';
});

const toggleOptionSelect = (option) => {
  selectedOption.value = option.title;
  emit('update:modelValue', option.title);
  isDropDownVisible.value = false;
};
</script>

<style lang="scss" scoped>
.dropdown-wrapper {
  width: 300px;

  .dropdown-selected-option {
    margin-top: 10px;
    width: 300px;
    border: 1px solid rgba(41, 39, 125, 0.40);
    display: flex;
    border-radius: 6px;
    height: 40px;
    align-items: center;
    position: relative;
    z-index: 1;

    svg {
      &:hover {
        cursor: pointer;
      }
    }

    .placeholder1 {
      position: absolute;
      top: -14px;
      left: 3px;
      color: rgba(41, 39, 125, 0.40);
      z-index: 9999;
      background-color: white;
    }

    .arrow {
      position: absolute;
      top: 30%;
      right: 10px;
      transform: rotate(180deg);
    }

    .arrowNull {
      position: absolute;
      top: 30%;
      right: 10px;
    }
  }

  .dropdown-selected-optionNull {
    margin-top: 10px;
    border: 1px solid #F968bf;
    width: 300px;
    display: flex;
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
    height: 40px;
    align-items: center;
    position: relative;

    .placeholder {
      position: absolute;
      top: 50%;
      left: 10px;
      transform: translateY(-50%);
      color: rgba(41, 39, 125, 0.40);
    }

    .arrow {
      position: absolute;
      top: 30%;
      right: 10px;
      transform: rotate(180deg);
    }

    .arrowNull {
      position: absolute;
      top: 30%;
      right: 10px;
    }
  }

  .options-wrapper {
    width: 100%;
    border-bottom: 1px solid #F968bf;
    border-left: 1px solid #F968bf;
    border-right: 1px solid #F968bf;
    border-bottom-left-radius: 6px;
    border-bottom-right-radius: 6px;

    .option {
      padding: 7px 10px;
      color: #29277d;

      &:hover {
        cursor: pointer;
        background-color: #DADEFE;
      }
    }
  }

  .slide-fade-enter-active {
    transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
  }

  .slide-fade-leave-active {
    transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
  }

  .slide-fade-enter-from,
  .slide-fade-leave-to {
    opacity: 0;
  }
}
</style>