<template>
  <div class="section">
    <div class="fancy-grid">
      <div class="inner">
        <div class="content">
          <h1>About us</h1>
          <h2>Waarbij de inwoner centraal staat</h2>
          <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi iaculis nisl quis commodo facilisis. Morbi
            scelerisque gravida libero, at dapibus nisi mollis sed. Etiam mollis semper mauris vel congue.</p>
          <div class="cta-block">
            <a href="#" class="btn">
              <span class="bg"></span>
              <span class="text">Button text</span>
            </a>
          </div>
        </div>

        <div class="grid">

          <div class="item has-image">
            <div class="image">
              <img
                src="https://images.unsplash.com/photo-1607281671197-399dd9d01af5?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTAzMjQ5OTF8&ixlib=rb-4.0.3&q=80&w=400">
            </div>
          </div>

          <div class="item has-image">
            <div class="image">
              <img
                src="https://images.unsplash.com/photo-1591608971376-46e64aa7fd19?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=M3wzMjM4NDZ8MHwxfHJhbmRvbXx8fHx8fHx8fDE3MTAzMjQ5OTF8&ixlib=rb-4.0.3&q=80&w=400">
            </div>
          </div>

          <div class="item">
            <a href="" class="alloy-card blurb">
            <span class="inner">
              <h5>blub</h5>
              <p>Some random text</p>
            </span>
            </a>
          </div>

          <div class="item">
            <a href="" class="alloy-card blurb">
            <span class="inner">
              <h5>blub</h5>
              <p>Some random text</p>
            </span>
            </a>
          </div>

          <div class="item">
            <a href="" class="alloy-card blurb">
            <span class="inner">
              <h5>blub</h5>
              <p>Some random text</p>
            </span>
            </a>
          </div>

        </div>
      </div>
    </div>
  </div>
  <section style="height:500vh">
    <h1>Extra space just for demo, remove this</h1>
  </section>
</template>

<script setup lang="ts">
import { gsap } from 'gsap'

onMounted(() => {

  const item = document.querySelector('.fancy-grid')
  const effect = document.querySelectorAll('.item')
  const rect = item?.getBoundingClientRect()

  const randomMin = gsap.utils.random(-5, -30, 1, true)
  const randomPlus = gsap.utils.random(5, 30, 1, true)
  const width = rect?.width


  const arraySetters: any[] = []

  effect.forEach(item => {
    const setter = {
      x: gsap.quickSetter(item, 'x', 'px'),
      y: gsap.quickSetter(item, 'y', 'px'),
      minus: width && gsap.utils.mapRange(0, width, randomMin(), randomPlus()),
      plus: width && gsap.utils.mapRange(0, width, randomMin(), randomPlus())
    }
    arraySetters.push(setter)
  })

  document.querySelector('body')?.addEventListener('mousemove', (e) => {
    arraySetters.forEach((item, index) => {
      const obj = arraySetters[index]
      obj.x(obj.minus(e.clientX - rect!.left))
      obj.y(obj.plus(e.clientY - rect!.top))
    })
  })
})
</script>

<style lang="scss">
$brand-one: #0091ad;
$brand-two: #106374;
$brand-three: #5cc844;
$brand-light: #ffffff;
$brand-dark: #222;
$brand-grey: #e9ecef;

$box-shadow: 0px 0px 75px rgba(0, 0, 0, 0.08);
$border-radius: 30px;

body {
  background-color: $brand-one;
  // display: flex;
  // justify-content: center;
  // align-items: center;
  // min-height: 100vh;
}

.section {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

h1, h2, h3, h4, h5, h6, p, a, span, strong, label, blockquote, ul, li, ol, mark {
  margin-top: 0;
  margin-bottom: 0;
}

.alloy-card {
  display: block;
  color: $brand-dark;
  display: flex;
  flex-direction: column;
  height: 100%;

  > .inner {
    background-color: $brand-light;
    padding: 20px;
    box-shadow: $box-shadow;
    flex-grow: 1;
  }

  .alloy-image {
    order: -1;
    min-height: 180px; // todo standaard hoogte bedenken
    max-height: 180px;
  }

  .btn-group {
    text-align: right;
  }

  &:hover {
    text-decoration: none;
  }
}

.image {
  width: 100%;
  height: 100%;

  img {
    object-fit: cover;
    height: 100%;
    width: 100%;
  }
}

.fancy-grid {
  padding: 10px;
  max-width: 100%;
  width: 1200px;
  margin-left: auto;
  margin-right: auto;

  > .inner {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    gap: 50px;

    .content {
      flex-basis: 370px;
    }
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-column-gap: 30px;
    grid-row-gap: 30px;

    > .item {
      align-self: end;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: flex-end;


      &.has-image {
        align-self: start;
        height: calc(100% - 40px);
      }

      > * {
        border-radius: $border-radius;
        overflow: hidden;
        box-shadow: $box-shadow;
      }

      .blurb {
        width: 100%;
        border-radius: $border-radius /2;
        height: auto;
        text-decoration: none;

        h5 {
          font-size: 20px;
          color: $brand-one;
        }
      }

      &:nth-child(1) {
        grid-area: 1 / 4 / 6 / 7;
      }

      &:nth-child(2) {
        grid-area: 3 / 1 / 6 / 4;
      }

      &:nth-child(3) {
        grid-area: 2 / 2 / 3 / 4;
      }

      &:nth-child(4) {
        grid-area: 4 / 3 / 6 / 5;
      }

      &:nth-child(5) {
        grid-area: 3 / 6 / 5 / 8;
      }
    }
  }
}

.cta-block {
  margin-top: 30px;

}

.btn {
  display: inline-block;
  position: relative;
  font-weight: 700;
  text-decoration: none;
  background-color: #5cc844;
  padding: 10px 45px;
  font-size: 18px;
  border-radius: 4em;
  color: #fff;
  overflow: hidden;
  box-shadow: 0 0 5px 0 rgba(#000, 0.2);

  .text {
    position: relative;
    z-index: 100;
  }

  .bg {
    position: absolute;
    z-index: 10;
    top: 0;
    left: 0;
    width: 120%;
    height: 100%;
    background-color: #fff;
    clip-path: polygon(0% 0%, 85% 0, 100% 50%, 85% 100%, 0 100%);

    transform: translatex(-100%);
    transition: transform;
    transition-duration: 300ms;
    transition-timing-function: ease;
  }

  &:hover {
    color: #5cc844;

    .bg {
      transform: translatex(0);
    }
  }
}

</style>