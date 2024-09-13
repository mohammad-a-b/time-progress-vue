<template>
  <div class="eye" ref="eyeRef">
    <div class="pupil" :style="pupilStyle"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const eyeRef = ref(0);
const pupilStyle = ref({ top: '50%', left: '50%' });

function updatePupilPosition(event) {
  const eye = eyeRef.value.getBoundingClientRect();
  const eyeCenterX = eye.left + eye.width / 2;
  const eyeCenterY = eye.top + eye.height / 2;

  const angle = Math.atan2(event.clientY - eyeCenterY, event.clientX - eyeCenterX);
  const distance = Math.min(eye.width / 6, Math.hypot(event.clientX - eyeCenterX, event.clientY - eyeCenterY));

  pupilStyle.value = {
    top: `${50 + distance * Math.sin(angle)}%`,
    left: `${50 + distance * Math.cos(angle)}%`,
  };
}

onMounted(() => document.addEventListener('mousemove', updatePupilPosition));
onUnmounted(() => document.removeEventListener('mousemove', updatePupilPosition));
</script>

<style scoped>
.eye {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: white;
  border: 2px solid black;
  position: relative;
  overflow: hidden;
}

.pupil {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: black;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: transform 1s ease;
}
</style>
