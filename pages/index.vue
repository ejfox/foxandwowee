<template>
  <div>
    <div class="fixed top-2 right-2 z-20">
      <UButton color="red" icon="i-bxl-youtube" @click="goToYoutube">Watch on YouTube</UButton>
    </div>
    <TresCanvas window-size shadows power-preference="high-performance" :output-color-space="SRGBColorSpace" class="z-10">
      <OrbitControls />
      <TresPerspectiveCamera ref="camera" :position="[2, 0, 9]" :fov="32" />
      <TresDirectionalLight ref="light" :position="[5, 5, 5]" cast-shadow />
      <TresDirectionalLight ref="light2" :position="[-5, 5, 5]" cast-shadow :color="'red'" />
      <TresDirectionalLight ref="light3" :position="[5, -5, 5]" cast-shadow :color="'blue'" />

      <Suspense>
        <TresMesh ref="textFox" :position="[0, -0.1, 0]" :rotation="[0, 0, 0]" :scale="[1, 1, 1]" cast-shadow
          @click="goToYoutube">
          <TresTextGeometry :args="['FOX', { font, ...fontOptions }]" center />
          <TresMeshPhongMaterial :color="'#CCC'" />
        </TresMesh>
      </Suspense>

      <Suspense>
        <TresMesh ref="textAnd" :position="[0, -0.8, 0]" :rotation="[0, 0, 0]" :scale="[1, 1, 1]" cast-shadow
          @click="goToYoutube">
          <TresTextGeometry :args="['AND', { font, ...fontOptions }]" center />
          <TresMeshPhongMaterial :color="'#CCC'" />
        </TresMesh>
      </Suspense>

      <Suspense>
        <TresMesh ref="textWowee" :position="[0, -1.5, 0]" :rotation="[0, 0, 0]" :scale="[1, 1, 1]" cast-shadow
          @click="goToYoutube">
          <TresTextGeometry :args="['WOWEE', { font, ...fontOptions }]" center />
          <TresMeshPhongMaterial :color="'#CCC'" />
        </TresMesh>
      </Suspense>
    </TresCanvas>
  </div>
</template>

<script setup>
import * as d3 from 'd3'
import { TresCanvas, useLoader, extend } from '@tresjs/core'
import { FontLoader } from 'three/addons/loaders/FontLoader'
import { TextGeometry } from 'three/addons/geometries/TextGeometry'
import { SRGBColorSpace } from 'three'

extend({ TextGeometry })

const ytUrl = 'https://www.youtube.com/channel/UCgnvqU8Ibpks9aeIOFPWvnw'

function goToYoutube() {
  window.open(ytUrl, '_blank')
}

const fontPath = 'https://raw.githubusercontent.com/Tresjs/assets/main/fonts/FiraCodeRegular.json'
const loader = new FontLoader()

const fontOptions = {
  size: 0.5,
  height: 1,
  curveSegments: 2
}

const font = await new Promise((resolve, reject) => {
  loader.load(fontPath, (font) => {
    resolve(font)
  }, undefined, reject)
})

const textFox = shallowRef(null)
const textAnd = shallowRef(null)
const textWowee = shallowRef(null)
const camera = shallowRef(null)
const light = shallowRef(null)
const light2 = shallowRef(null)
const light3 = shallowRef(null)

const { onLoop } = useRenderLoop()

const { x, y } = useMouse()

onLoop(({ elapsed }) => {
  if (light.value) {
    light.value.position.x = Math.sin(elapsed) * 5
    light.value.position.y = Math.cos(elapsed) * 5
    light.value.position.z = Math.sin(elapsed) * 5
    light.value.lookAt(0, 0, 0)
  }

  if (light2.value) {
    light2.value.position.x = Math.cos(elapsed) * 5
    light2.value.position.y = Math.sin(elapsed) * 5
    light2.value.position.z = Math.cos(elapsed) * 5
    light2.value.lookAt(0, 0, 0)
  }

  if (light3.value) {
    light3.value.position.x = Math.sin(-elapsed) * 5
    light3.value.position.y = Math.cos(-elapsed) * 5
    light3.value.position.z = Math.sin(-elapsed) * 5
    light3.value.lookAt(0, 0, 0)
  }

  if (camera.value) {
    // camera.value.position.x = Math.sin(elapsed) * 3
    // camera.value.position.y = Math.cos(elapsed) * 3
    // // camera.value.position.z = Math.sin(elapsed) * 2.5
    // camera.value.lookAt(0, 0, 0)
    // use mouse x and y to rotate slightly skew the camera
    // mouse x will be the y rotation
    // mouse y will be the z position
    // camera.value.rotation.y = x.value * 0.1
    camera.value.position.z = y.value * 0.05
    camera.value.lookAt(x.value * 0.005, y.value * -0.0002, 0)

  }
})
</script>
