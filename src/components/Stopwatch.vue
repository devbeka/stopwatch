<template>
  <div class="stopwatch">
    <div v-for="(timer, index) in timers" :key="index">
      <div class="timer" :class="{ active: timer.isActive }">
        <p>{{ formatTime(timer.time) }}</p>
        <hr />
        <div class="buttons">
          <button v-if="!timer.running" @click="startTimer(index)">
            <img :src="start" alt="start" />
          </button>
          <button v-else @click="pauseTimer(index)">
            <img :src="pause" alt="pause" />
          </button>
          <button @click="resetTimer(index)">
            <img :src="reset" alt="reset" />
          </button>
        </div>
      </div>
    </div>
    <button class="add" @click="addTimer">
      <img :src="plus" alt="add-timer" />
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import start from '@/assets/svg/start.svg'
import pause from '@/assets/svg/pause.svg'
import reset from '@/assets/svg/reset.svg'
import plus from '@/assets/svg/plus.svg'

const timers = ref([{ time: 0, running: false, paused: false, isActive: true }])

const startTimer = (index) => {
  timers.value[index].isActive = false
  timers.value[index].running = true
  timers.value[index].paused = false
  const timer = setInterval(() => {
    timers.value[index].time++
  }, 1000)
  timers.value[index].timer = timer
}

const pauseTimer = (index) => {
  timers.value[index].isActive = true
  timers.value[index].running = false
  timers.value[index].paused = true
  clearInterval(timers.value[index].timer)
}

const resetTimer = (index) => {
  timers.value[index].isActive = true
  timers.value[index].time = 0
  timers.value[index].running = false
  timers.value[index].paused = false
  clearInterval(timers.value[index].timer)
}

const addTimer = () => {
  timers.value.push({ time: 0, running: false, paused: false, isActive: true })
}

const formatTime = (time) => {
  const hours = Math.floor(time / 3600)
  const minutes = Math.floor((time % 3600) / 60)
  const seconds = time % 60

  if (hours > 0) {
    return `${hours}:${minutes.toString().padStart(2, '0')}:${seconds
      .toString()
      .padStart(2, '0')}`
  } else if (minutes > 0) {
    return `${minutes}:${seconds.toString().padStart(2, '0')}`
  } else {
    return seconds.toString().padStart(1, '0')
  }
}

const computedTimers = computed(() => {
  return timers.value.map((timer) => ({
    ...timer,
    displayTime: formatTime(timer.time),
  }))
})
</script>

<style scoped>
.active {
  opacity: 0.5;
}
.stopwatch {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-column-gap: 50px;
  grid-row-gap: 45px;
}
p {
  padding: 22px 0 20px 0;
  font-weight: 400;
  font-size: 22px;
  line-height: 21px;
}
.timer {
  text-align: center;
  color: white;
  width: 225px;
  height: 120px;
  background: #696969;
}
.buttons {
  padding-top: 18px;
  width: 80px;
  justify-content: space-between;
  text-align: center;
  display: flex;
  margin: auto;
}
button {
  padding: 0;
  background: transparent;
  border: none;
  cursor: pointer;
}
.add {
  width: 225px;
  height: 120px;
  background: #696969;
  display: flex;
  justify-content: center;
  align-items: center;
}

@media (max-width: 1125px) {
  .stopwatch {
    grid-template-columns: repeat(3, 1fr);
  }
}
@media (max-width: 870px) {
  .stopwatch {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 595px) {
  .stopwatch {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>
