<script setup lang="ts">
import {useFullscreen} from '@vueuse/core'
import {onBeforeUnmount, onMounted, ref} from 'vue';
import ColorPicker from "./components/ColorPicker.vue";

const hue = ref(0);
const {toggle} = useFullscreen(document.documentElement);

const updateBodyColor = () => {
  document.body.style.backgroundColor = `hsl(${hue.value}, 100%, 50%)`;
  localStorage.setItem('backgroundColorHue', String(hue.value));
};

//переход в полноэкранный режим
const handleKeyDown = (event: KeyboardEvent) => {
  if (event.ctrlKey && event.key === 'Enter') {
    toggle();
  } else if (event.key === 'ArrowRight') {
    hue.value = (hue.value + 10) % 360;
    updateBodyColor();
  } else if (event.key === 'ArrowLeft') {
    hue.value = (hue.value - 10 + 360) % 360;
    updateBodyColor();
  }
};

onMounted(() => {
  const savedHue = localStorage.getItem('backgroundColorHue');
  if (savedHue) {
    hue.value = Number(savedHue);
    updateBodyColor();
  } else {
    document.body.style.backgroundColor = 'black';
  }
  window.addEventListener('keydown', handleKeyDown);
});

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeyDown);
});
</script>

<template>
  <div>
    <ColorPicker/>
  </div>
</template>

