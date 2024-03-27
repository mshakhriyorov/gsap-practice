<template>
  <div ref="containerRef" class="py-12 px-30" @mousemove="onMouseMove">
    <div class="grid grid-cols-3 min-w-[1200px]">
      <h2 class="flex flex-col items-start text-[#FF5100] font-semibold text-[50px] min-w-[530px]">TAOMLA
        <span class="capitalize text-[30px] text-[#170A04]">making your restaurant's digital presence stand out</span>
      </h2>

      <img src="/pizza.png" alt="pointer" class="item w-[163px] col-span-2 place-self-center">
      <img src="/cake.png" alt="pointer" class="item w-[255px] self-end" />
      <img src="/bread.png" alt="pointer" class="item w-[255px] self-start " />
      <img src="/burger.png" alt="pointer" class="item w-[438px]" />
      <img src="/salad.png" alt="pointer" class="item w-[163px] col-span-2 place-self-center" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { gsap } from 'gsap'

const containerRef = ref<HTMLDivElement | null>(null)
const items = ref<NodeListOf<Element>>()
const rect = ref<any>(null)
const arraySetters = ref<any[]>([])

const randomMin = gsap.utils.random(-5, -30, 1, true)
const randomPlus = gsap.utils.random(5, 30, 1, true)


const setImagesRange = () => {
  items.value?.forEach(item => {
    const setter = {
      x: gsap.quickSetter(item, 'x', 'px'),
      y: gsap.quickSetter(item, 'y', 'px'),
      minus: gsap.utils.mapRange(0, rect.value.width, randomMin(), randomPlus()),
      plus: gsap.utils.mapRange(0, rect.value.width, randomMin(), randomPlus())
    }
    arraySetters.value.push(setter)
  })
}

const onMouseMove = (e: MouseEvent) => {
  arraySetters.value.forEach((item, index) => {
    const obj = arraySetters.value[index]
    obj.x(obj.minus(e.clientX - rect.value.left))
    obj.y(obj.plus(e.clientY - rect.value.top))
  })
}

onMounted(() => {
  if (containerRef.value) {
    rect.value = containerRef.value.getBoundingClientRect()
  }
  items.value = document.querySelectorAll('.item')
  setImagesRange()
})


</script>

<style lang="scss">
body {
  width: 100%;
  height: 100vh;
}
</style>