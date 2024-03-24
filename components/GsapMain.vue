<script setup lang="ts">
import gsap from 'gsap'

const ctx = ref<any>(null)

onMounted(() => {
  ctx.value = gsap.context(() => {
    const safeToAnimate = window.matchMedia('(prefers-reduced-motion: no-preference)').matches
    if (!safeToAnimate) return;

    // Get the elements that we need
    const pointers = document.querySelectorAll('.pointer')

    // let xPosition: number
    // let yPosition: number

    let storedXPosition: number
    let storedYPosition: number

    // Set up our coordinate mapping with GSAP utils!
    let mapWidth: (arg0: any) => number
    let mapHeight: (value: number) => number

    const setMaps = () => {
      mapWidth = gsap.utils.mapRange(0, innerWidth, -50, 50)
      mapHeight = gsap.utils.mapRange(0, innerHeight, -50, 50)
    }

    window.addEventListener('resize', setMaps)
    setMaps()

    const movePointer = (xPosition: number, yPosition: number) => {
      if (storedXPosition === xPosition && storedYPosition === yPosition) return
      // Now apply the new position to all pointers
      pointers.forEach((pointer: any) => {
        const xSet = gsap.quickSetter(pointer, 'x', '%')
        const ySet = gsap.quickSetter(pointer, 'y', '%')
        xSet(xPosition)
        ySet(yPosition)
      })
      storedXPosition = xPosition
      storedYPosition = yPosition
    }

    gsap.ticker.add(() => movePointer(mapWidth(storedXPosition), mapHeight(storedYPosition)))
    const updateMouseCoords = (event: { clientX: number; clientY: number; }) => {
      storedXPosition = event.clientX
      storedYPosition = event.clientY
    }

    window.addEventListener('mousemove', updateMouseCoords)
  })
})

onMounted(() => ctx.value.revert())
</script>

<template>
  <div>
    <img src="/pizza.png" alt="pointer" class="pointer w-[163px]" />
    <img src="/salad.png" alt="pointer" class="pointer w-[163px]" />
    <img src="/cake.png" alt="pointer" class="pointer w-[255px]" />
    <img src="/bread.png" alt="pointer" class="pointer w-[255px]" />
    <img src="/burger.png" alt="pointer" class="pointer w-[438px]" />
  </div>
</template>

<style>
:root {
  --mouse-x: 0;
  --mouse-y: 0;
}

body {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: "Signika", sans-serif;
  overflow: hidden;
}

.pointer {
  position: relative;
  text-align: center;
}

.pointer img {
  width: 100px;
  display: block;
  margin: 0 auto;
}

.screen-log {
  position: absolute;
  bottom: -3rem;
  width: 300%;
  left: -100%;
  padding-top: 1rem;
  text-align: center;
}
</style>