<!-- eslint-disable no-unused-vars -->
<script setup lang="ts">
import TheWelcome from '../components/TheWelcome.vue'
import { Rectangle, Assets, utils } from 'pixi.js'
import { Application, onTick, Loader } from 'vue3-pixi'
import type { GraphicsInst, ApplicationInst, TextInst } from 'vue3-pixi'
import { ref, reactive, onMounted, onUnmounted, onUpdated } from 'vue'
// import { OutlineFilter } from 'pixi-filters'

// application
const devicePixelRatio = ref(window.devicePixelRatio)
const app = ref<ApplicationInst>()

// sprite hitarea
const hitArea = new Rectangle(0, 0, 128, 128)

const blurNumber = ref(0.5)

// event
const onClick = () => {
  console.log('onClick!')
  blurNumber.value += 0.5
}

const drawArc = (e: GraphicsInst) => {
  e.beginFill('#4f455c')
  e.arc(0, 0, 100, 0, Math.PI / 2, false)
  e.endFill()
}

const textRef = ref<TextInst>()
const eventName = ref('none')
const evtHandler = (name: string, evt: any) => {
  eventName.value = name
  // console.log(textRef.value)
  // console.log('name: ', name, 'evt: ', evt)
}

const onResolved = (sheet: any) => {
  // console.log('sheet: ', sheet)
}

const position = reactive({ x: 160, y: 70 })
// 应用程序的更新循环
let count = 0
onTick((delta) => {
  count += delta * 0.05
  position.x = 160 + Math.cos(count) * 30
  position.y = 70 + Math.sin(count) * 30
})

onMounted(() => {
  console.log(app.value?.app.screen)
})
onUnmounted(() => {
  console.log('Home page Unmounted')
  // Assets.reset()
  // utils.clearTextureCache()
})
onUpdated(() => {
  console.log('Updated')
})
</script>

<template>
  <TheWelcome />
  <!-- :background-color="0x1e90ff" -->
  <Application
    :width="640"
    :height="480"
    :antialias="true"
    :resolution="devicePixelRatio || 1"
    ref="app"
  >
    <Container>
      <Sprite
        texture="/textures/mujian.png"
        :position="position"
        :scale="0.5"
        :anchor="0.5"
        :on-click="onClick"
      >
        <BlurFilter :blur="blurNumber"></BlurFilter>
      </Sprite>
      <text
        :style="{ fill: 'white', fontSize: 36, fontFamily: 'Arial' }"
        :position="[510, 80]"
        :anchor="0.5"
        @click="onClick"
        >Hello World</text
      >
      <Graphics :position="[0, 0]" @render="drawArc"></Graphics>
      <Sprite
        texture="/textures/mushroom.png"
        :position="[320, 60]"
        :anchor="0.5"
        @pointermove="(evt) => evtHandler('pointermove', evt)"
        @pointerleave="(evt) => evtHandler('pointerleave', evt)"
        @click="(evt) => evtHandler('click', evt)"
      ></Sprite>
      <text
        ref="textRef"
        :position="[320, 110]"
        :anchor="0.5"
        :style="{ fill: '#1e9f', fontSize: 28 }"
        >{{ eventName }}</text
      >
    </Container>
    <Container :position="[0, 160]">
      <Loader
        :resources="{ spritesheet: '/textures/adventurer-spritesheet.json' }"
        @resolved="onResolved($event.spritesheet)"
        @progress="(progress) => console.log(progress)"
      >
        <AnimatedSprite
          :textures="['adventurer-idle-00.png', 'adventurer-idle-01.png', 'adventurer-idle-02.png']"
          playing
          :animation-speed="0.1"
          :scale="2"
        ></AnimatedSprite>
      </Loader>
    </Container>
  </Application>
</template>
