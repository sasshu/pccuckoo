<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { Ref } from 'vue'

const hourNeedle: Ref<HTMLElement | null> = ref(null)
const minuteNeedle: Ref<HTMLElement | null> = ref(null)
const secondNeedle: Ref<HTMLElement | null> = ref(null)
const isNoteVisible: Ref<boolean> = ref(false)

const tick = (now: Date): void => {
  const hour = now.getHours()
  const minute = now.getMinutes()
  const second = now.getSeconds()
  const hourDeg = hour * 30 + minute * 0.5 + second * 0.00833333333
  const minuteDeg = minute * 6 + second * 0.1
  const secondDeg = second * 6
  hourNeedle.value!.style.transform = `rotate(${hourDeg}deg)`
  minuteNeedle.value!.style.transform = `rotate(${minuteDeg}deg)`
  secondNeedle.value!.style.transform = `rotate(${secondDeg}deg)`
}

const isChangedHour = (now: Date): boolean => {
  return now.getSeconds() === 0 && now.getMinutes() === 0
}

const init = () => {
  // window.api.playCuckoo(12)
  // tick()
  isNoteVisible.value = true
  setInterval(() => {
    const now = new Date(Date.now() + (new Date().getTimezoneOffset() + 9 * 60) * 60 * 1000)
    tick(now)
    if (isChangedHour(now)) {
      isNoteVisible.value = true
      window.api.playCuckoo(now.getHours())
      setTimeout(() => {
        isNoteVisible.value = false
      }, 3000)
    }
  }, 100)
}

onMounted(() => {
  init()
})
</script>

<template>
  <div class="container">
    <div class="clock">
      <div ref="hourNeedle" class="hour"></div>
      <div ref="minuteNeedle" class="minute"></div>
      <div ref="secondNeedle" class="second"></div>
    </div>
    <div v-show="isNoteVisible" class="hato"></div>
  </div>
</template>

<style scoped lang="scss">
.container {
  position: absolute;
  display: flex;
  justify-content: start;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  width: 200px;
  height: 100px;
  // background-color: blanchedalmond;
  border-radius: 10px;
}

.clock {
  position: relative;
  width: 50%;
  height: 100%;
  background: url('./assets/clock.png') no-repeat;
  background-size: 100%;
}

.hato {
  position: relative;
  width: 50%;
  height: 100%;
  background: url('./assets/hato.gif') no-repeat;
  background-size: 100%;
}

.hour,
.minute,
.second {
  position: absolute;
  background: transparent;
  transform-origin: bottom;
}

.hour {
  background-color: #333333;
  border-radius: 10000px;
  width: 3%;
  height: 25%;
  top: 25%;
  left: 48.5%;
}

.minute {
  background-color: #333333;
  border-radius: 10000px;
  width: 2%;
  height: 30%;
  top: 20%;
  left: 49%;
}

.second {
  background-color: #777777;
  border-radius: 10000px;
  width: 1.2%;
  height: 35%;
  top: 15%;
  left: 49.4%;
}
</style>
