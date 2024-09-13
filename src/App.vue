<script setup>
import { ref, onMounted } from "vue";
import eye from "./components/eye.vue";

const timeLeft = ref({
  secondsToNextMinute: 0,
  minutesToNextHour: 0,
  hoursToNextDay: 0,
});

onMounted(() => {
  const updateTime = () => {
    const now = new Date();
    const seconds = now.getSeconds();
    const minutes = now.getMinutes();
    const hours = now.getHours();

    timeLeft.value = {
      secondsToNextMinute: 60 - seconds,
      minutesToNextHour: 60 - minutes,
      hoursToNextDay: 24 - hours,
    };
  };

  updateTime();
  setInterval(updateTime, 1000);
});
</script>

<template>
  <main class="container">
    <div class="title">Progress</div>
    <div class="timer-container">
      <div class="timer-item">
        <div class="timer-text">
          🕑 next minute
          <span>{{ timeLeft.secondsToNextMinute }} seconds left</span>
        </div>
        <div class="progress-bar">
          <div
            class="progress-bar-fill"
            :style="{
              width: ((60 - timeLeft.secondsToNextMinute) * 100) / 60 + '%',
            }"
          ></div>
        </div>
      </div>
      <div class="timer-item">
        <div class="timer-text">
          🕑 next hour
          <span> {{ timeLeft.minutesToNextHour }} minutes left</span>
        </div>
        <div class="progress-bar">
          <div
            class="progress-bar-fill"
            :style="{
              width: ((60 - timeLeft.minutesToNextHour) * 100) / 60 + '%',
            }"
          ></div>
        </div>
      </div>
      <div class="timer-item">
        <div class="timer-text">
          🌅 next day <span>{{ timeLeft.hoursToNextDay }} hour left</span>
        </div>
        <div class="progress-bar">
          <div
            class="progress-bar-fill"
            :style="{
              width: ((24 - timeLeft.hoursToNextDay) * 100) / 24 + '%',
            }"
          ></div>
        </div>
      </div>
    </div>
    <div class="eye">
      <eye />
      <eye />
    </div>
  </main>
</template>

<style scoped>
.title {
  font-size: 32px;
  padding: 30px 20px;
  margin-bottom: 10px;
  font-weight: 700;
  text-align: center;
  line-height: 1.6em;
  color: #333;
  background: #fff;
}
.container {
  padding: 4px;
  margin: auto;
  max-width: 1000px;
}
.timer-container {
  background: #fff;
  padding: 20px;
}

.timer-item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.timer-text {
  width: 100%;
  display: flex;
  font-size: 26px;
  font-weight: 600;
  justify-content: space-between;
  align-items: center;
  margin: 25px 0;
}
.timer-text span {
  font-size: 20px;
  color: #333;
}
.progress-bar {
  width: 100%;
  height: 45px;
  border-radius: 5px;
  background-color: #efefef;
}

.progress-bar-fill {
  height: 100%;
  background-color: #2ecc71;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  transition: width 0.25s ease;
}
.eye {
  display: flex;
  gap: 30px;
  justify-content: center;
  margin-top: 10px;
}
@media (width<768px) {
  .timer-text {
    font-size: 20px;
    margin: 10px 0;
  }
  .timer-text span {
    font-size: 15px;
  }
}
</style>
