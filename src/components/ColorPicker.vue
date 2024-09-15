<script setup lang="ts">
import {onBeforeUnmount, onMounted, ref} from 'vue';

const hue = ref(0);
const showPicker = ref(false);

const updateColor = () => {
  document.body.style.backgroundColor = `hsl(${hue.value}, 100%, 50%)`;
  localStorage.setItem('backgroundColorHue', String(hue.value));
};

const togglePicker = () => {
  showPicker.value = !showPicker.value;
};

const closePicker = (event: MouseEvent) => {
  const target = event.target as HTMLElement;
  if (!target.closest('.color-picker')) {
    showPicker.value = false;
  }
};

onMounted(() => {
  const savedHue = localStorage.getItem('backgroundColorHue');
  if (savedHue) {
    hue.value = Number(savedHue);
    updateColor();
  } else {
    document.body.style.backgroundColor = 'black';
  }
  document.addEventListener('click', closePicker);
});

onBeforeUnmount(() => {
  document.removeEventListener('click', closePicker);
});
</script>

<template>
  <div class="color-picker" @click="togglePicker">
    <div class="toggle-btn">🎨</div>
    <div v-if="showPicker" class="picker">
      <input type="range" min="0" max="360" v-model="hue" @input="updateColor"/>
    </div>
  </div>
</template>

<style scoped>
.color-picker {
  position: fixed;
  bottom: 20px;
  left: 20px;
  z-index: 1000;
  display: flex;
  flex-direction: row;
  align-items: center;
}

.toggle-btn {
  font-size: 36px;
  margin-right: 12px;
}

.picker {
  margin-top: 10px;
  width: 200px;
  transition: opacity 0.3s ease;
}
</style>
