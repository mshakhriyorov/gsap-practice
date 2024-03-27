<template>
  <div ref="containerRef" class="py-12 px-30 relative" @mousemove="onMouseMove">
    <div class="grid grid-cols-3 min-w-[1200px]">
      <h2 class="flex flex-col items-start text-[#FF5100] font-semibold text-[50px] min-w-[530px]">TAOMLA
        <span class="capitalize text-[30px] text-[#170A04]">making your restaurant's digital presence stand out</span>
      </h2>

      <div class="item col-span-2 place-self-center">
        <img src="/pizza.png" alt="pointer" class="w-[163px]">
      </div>
      <div class="item self-end">
        <img src="/cake.png" alt="pointer" class="w-[255px]">
      </div>
      <div class="item self-start">
        <img src="/bread.png" alt="pointer" class="w-[255px]">
      </div>
      <div class="item">
        <img src="/burger.png" alt="pointer" class="w-[438px]">
      </div>
      <div class="item col-span-2 place-self-center">
        <img src="/salad.png" alt="pointer" class="w-[163px]">
      </div>
    </div>
    <div class="circle right"/>
    <div class="circle left"/>
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

.circle {
  @apply rounded-full w-8 h-8 [background:#FF5100] absolute;

  &.left {
    @apply left-40  bottom-20;
  }


  &.right {
    @apply right-40  bottom-4;
  }
}

.item {
  @apply relative;

  &::before, &::after {
    @apply content-[''] [background:#FF5100] absolute rounded-full;
  }

  &:nth-of-type(1)::before {
    @apply w-[30px] h-[30px] right-[-50px] -top-2.5;
  }

  &:nth-of-type(1)::after {
    @apply w-5 h-5 -left-2.5 -bottom-2.5;
  }


  &:nth-of-type(2)::before {
    @apply w-[30px] h-[30px] left-[-50px] -top-2.5;
  }

  &:nth-of-type(2)::after {
    @apply w-5 h-5 -right-10 bottom-2.5;
  }

  &:nth-of-type(3)::before {
    @apply w-[30px] h-[30px] left-0 -top-2.5;
  }

  &:nth-of-type(3)::after {
    @apply w-[50px] h-[50px] right-[50px] bottom-2.5;
  }

  &:nth-of-type(4)::before {
    @apply w-2.5 h-2.5 -left-2.5 -bottom-2.5;
  }

  &:nth-of-type(4)::after {
    @apply w-10 h-10 -right-2.5 -bottom-2.5;
  }
}
</style>