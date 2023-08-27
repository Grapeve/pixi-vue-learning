<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'
import * as PIXI from 'pixi.js'

// 创建应用
const app = new PIXI.Application({
  width: window.innerWidth / 2,
  height: window.innerHeight / 2,
  backgroundColor: 0x1099bb,
  resolution: window.devicePixelRatio || 1,
  antialias: true
})

const initRender = async () => {
  const container = new PIXI.Container()

  const mushroomTexture = PIXI.Texture.from('/textures/mushroom.png')
  const mushroomSprite = new PIXI.Sprite(mushroomTexture)
  mushroomSprite.position.set(app.screen.width / 2, app.screen.height / 6)
  mushroomSprite.anchor.set(0.5)
  container.addChild(mushroomSprite)
  mushroomSprite.eventMode = 'dynamic'
  mushroomSprite.on('click', () => {
    eventText.text = 'click'
  })
  mushroomSprite.on('pointermove', () => {
    eventText.text = 'pointermove'
  })
  mushroomSprite.on('pointerleave', () => {
    eventText.text = 'pointerleave'
  })

  const eventText = new PIXI.Text('none', {
    fill: '#1e9f',
    fontSize: 26
  })
  eventText.position.set(app.screen.width / 2, app.screen.height / 3.1)
  eventText.anchor.set(0.5)
  container.addChild(eventText)

  const adventurerTextureResource = await PIXI.Assets.load('/textures/adventurer-spritesheet.json')
  const adventurerTextures = [
    adventurerTextureResource.textures['adventurer-idle-00.png'],
    adventurerTextureResource.textures['adventurer-idle-01.png'],
    adventurerTextureResource.textures['adventurer-idle-02.png']
  ]
  const adventurerSprite = new PIXI.AnimatedSprite(adventurerTextures)
  adventurerSprite.position.set(app.screen.width / 2, app.screen.height / 2)
  adventurerSprite.anchor.set(0.5)
  adventurerSprite.scale.set(2)
  adventurerSprite.play()
  adventurerSprite.animationSpeed = 0.1
  container.addChild(adventurerSprite)

  app.stage.addChild(container)
}

onMounted(() => {
  // 将应用画布添加到DOM中
  const pixiElement = document.getElementById('pixi')
  pixiElement?.appendChild(app.view)
  initRender()
})
onUnmounted(() => {
  console.log('About page Unmounted')
  PIXI.Assets.reset()
  PIXI.utils.clearTextureCache()
})
</script>

<template>
  <div id="pixi"></div>
</template>

<style scoped></style>
