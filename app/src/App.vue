<script setup lang="ts">
import { ref, computed } from 'vue';

const workDuration = 45; 
const restDuration = 15; 

const timeLeft = ref(workDuration);
const isWorking = ref(true);
const isRunning = ref(false);

let timerInterval: ReturnType<typeof setInterval> | null = null;

const formattedTime = computed(() => {
  const minutes = Math.floor(timeLeft.value / 60);
  const seconds = timeLeft.value % 60;
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
});

const toggleTimer = () => {
  if (isRunning.value) {
    clearInterval(timerInterval!);
    isRunning.value = false;
  } else {
    isRunning.value = true;
    timerInterval = setInterval(() => {
      if (timeLeft.value > 0) {
        timeLeft.value--;
      } else {
        isWorking.value = !isWorking.value;
        timeLeft.value = isWorking.value ? workDuration : restDuration;
      }
    }, 1000);
  }
};

const resetTimer = () => {
  if (timerInterval) clearInterval(timerInterval);
  isRunning.value = false;
  isWorking.value = true;
  timeLeft.value = workDuration;
};
</script>

<template>
  <div class="timer-container">
    <h1 :class="isWorking ? 'text-work' : 'text-rest'">
      {{ isWorking ? '🔥 Час працювати!' : '🧘 Час відпочити' }}
    </h1>
    <div class="time-display">{{ formattedTime }}</div>
    <div class="controls">
      <button @click="toggleTimer" :class="isRunning ? 'btn-pause' : 'btn-start'">
        {{ isRunning ? 'Пауза' : 'Старт' }}
      </button>
      <button @click="resetTimer" class="btn-reset">Скинути</button>
    </div>
  </div>
</template>

<style scoped>

.timer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-family: Arial, sans-serif;
  background-color: #1e1e2f;
  color: white;
}

.time-display {
  font-size: 6rem;
  font-weight: bold;
  margin: 20px 0;
  font-variant-numeric: tabular-nums;
}

.text-work { color: #ff6b6b; }

.text-rest { color: #4ecdc4; }

button {
  margin: 0 10px;
  padding: 12px 24px;
  font-size: 1.2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: opacity 0.2s;
}

button:hover { opacity: 0.8; }
.btn-start { background-color: #4ecdc4; color: #1e1e2f; }
.btn-pause { background-color: #ffe66d; color: #1e1e2f; }
.btn-reset { background-color: #555; color: white; }
</style>
