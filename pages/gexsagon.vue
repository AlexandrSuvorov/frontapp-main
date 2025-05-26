<template>
  <div class="relative w-screen h-screen bg-gray-100 overflow-hidden">
    <!-- Хлебные крошки -->
    <UBreadcrumb
      :items="[
        { label: 'Home', to: '/' },
        { label: 'Profile', to: '/gexsagon' }
      ]"
      class=""
    />

    <!-- Слайдер на весь экран -->
    <div class="w-full h-full flex items-center justify-center">
      <div
        class="hexagons flex transition-transform duration-500 ease-in-out"
        :style="hexagonsStyle"
      >
        <div
          v-for="(n, index) in hexCount"
          :key="n"
          class="hexagon flex items-center justify-center font-bold text-white text-3xl select-none flex-shrink-0"
          :style="getHexagonStyle(index)"
        >
          {{ n + 1 }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const currentIndex = ref(0)
const hexCount = 5
const baseHexWidth = 100
const baseGap = 30

// Высота правильного гексагона с шириной 100px
const baseHexHeight = Math.sqrt(3) / 2 * baseHexWidth

// Увеличение размера для центрального гексагона
const scaleCenter = 1.3

// Угол поворота слайдера в градусах
const rotationAngle = 315
const rad = (rotationAngle * Math.PI) / 180

// Расчёт шага сдвига — используем базовый размер + gap
const step = baseHexWidth + baseGap

const translateX = computed(() => currentIndex.value * step * Math.cos(rad))
const translateY = computed(() => currentIndex.value * step * Math.sin(rad))

const hexagonsStyle = computed(() => ({
  transform: `translate(${translateX.value}px, ${translateY.value}px) rotate(${rotationAngle}deg)`,
  transformOrigin: 'center center',
}))

// Функция для получения стиля каждого гексагона с учётом центрального
function getHexagonStyle(index: number) {
  const isCenter = index === 2 // 3-й элемент (индексация с 0)
  const width = isCenter ? baseHexWidth * scaleCenter : baseHexWidth
  const height = isCenter ? baseHexHeight * scaleCenter : baseHexHeight
  const marginRight = isCenter ? baseGap * scaleCenter : baseGap

  return {
    width: `${width}px`,
    height: `${height}px`,
    clipPath:
      'polygon(50% 0%, 93% 25%, 93% 75%, 50% 100%, 7% 75%, 7% 25%)',
    backgroundColor: '#4caf50',
    marginRight: `${marginRight}px`,
    userSelect: 'none',
  }
}

let intervalId: number | null = null

onMounted(() => {
  intervalId = window.setInterval(() => {
    currentIndex.value = (currentIndex.value + 1) % hexCount
  }, 3000)
})

onUnmounted(() => {
  if (intervalId !== null) clearInterval(intervalId)
})
</script>
