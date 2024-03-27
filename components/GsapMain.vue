<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'

const container = ref<HTMLDivElement>()
const images = ref<HTMLImageElement[]>([])
const mouse = ref({ x: 0, y: 0, moved: false })
let rect = { top: 0, left: 0, width: 0, height: 0 }

// Define an array of corresponding movements for the targets.
const movements = [-10, -3, -7, -13, -1]
const positions = [
  { x: 400, y: 20 },
  { x: 250, y: 200 },
  { x: 30, y: -10 },
  { x: 180, y: -150 },
  { x: 180, y: -150 }
]

const onMousemove = (e: MouseEvent) => {
  mouse.value.moved = true
  mouse.value.x = e.clientX - rect.left
  mouse.value.y = e.clientY - rect.top
}

const updateRect = () => {
  if (container.value) {
    const domRect = container.value.getBoundingClientRect()
    rect = { top: domRect.top, left: domRect.left, width: domRect.width, height: domRect.height }
  }
}

const parallaxIt = (el: HTMLImageElement, movement: number) => {
  const rect = el.getBoundingClientRect()
  gsap.to(el,
    {
      duration: 0.5,
      x: (mouse.value.x - rect.width / 2) / rect.width * movement,
      y: (mouse.value.y - rect.height / 2) / rect.height * movement
    })
}


const frameHandler = () => {
  if (mouse.value.moved) {
    images.value.forEach((image, index) => {
      parallaxIt(image, movements[index])
    })
  }
  // Reset the moved property of the mouse value for the next event.
  mouse.value.moved = false
}


const setImagesPosition = () => {
  images.value.forEach((pointer, i) => {
    const pos = positions[i % positions.length] // it will loop through positions if there are more images
    pointer.style.transform = `translate(${pos.x}%, ${pos.y}%)`
  })
}


onBeforeMount(() => {
  // Create an array of targets (pointers). The targets are string identifiers like ".pointer1", ".pointer2", etc.
  const targets = Array.from({ length: 5 }, (_, i) => `.pointer${i + 1}`)
  images.value = targets.map(target => (document.querySelector(target) as HTMLImageElement))

  setImagesPosition()
})

onMounted(() => {
  gsap.ticker.add(frameHandler)
  window.addEventListener('resize', updateRect)
  window.addEventListener('scroll', updateRect)
  updateRect()
})

onBeforeUnmount(() => {
  gsap.ticker.remove(frameHandler)
  window.removeEventListener('resize', updateRect)
  window.removeEventListener('scroll', updateRect)
})
</script>

<template>
  <div ref="container" class="w-full" @mousemove="onMousemove">
    <img src="/pizza.png" alt="pointer" class="pointer1 w-[163px]" />
    <img src="/salad.png" alt="pointer" class="pointer2 w-[163px]" />
    <img src="/cake.png" alt="pointer" class="pointer3 w-[255px]" />
    <img src="/bread.png" alt="pointer" class="pointer4 w-[255px]" />
    <img src="/burger.png" alt="pointer" class="pointer5 w-[438px]" />
  </div>
</template>

<style>

body {
  min-height: 100vh;
  font-family: "Signika", sans-serif;
}

.pointer img {
  width: 100px;
  display: block;
  margin: 0 auto;
}
</style>