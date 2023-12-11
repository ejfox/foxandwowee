<template>
  <div class="bg-red">
    <h1>hello world</h1>

    <TresCanvas window-size shadows power-preference="high-performance" :output-color-space="SRGBColorSpace">
      <OrbitControls />
      <TresPerspectiveCamera ref="camera" :position="[5, 5, 5]" :fov="75" :near="0.1" :far="1000" />
      <!-- <TresAmbientLight :color="0xffffff" :intensity="10" cast-shadow /> -->
      <TresDirectionalLight ref="light" :position="[0, 8, 4]" :intensity="2" cast-shadow />

      <Suspense>
        <TheModel ref="torus" />
      </Suspense>
    </TresCanvas>

  </div>
</template>
<script setup>
import * as d3 from 'd3'
import {
  TresCanvas,
  useLoader
} from '@tresjs/core'

import { SRGBColorSpace } from 'three';

// import { useGLTF } from '@tresjs/cientos'

import { GLTFLoader } from 'three/addons/loaders/GLTFLoader'

// const { scene } = await useLoader(GLTFLoader, '~/assets/8_16_2021.glb')


const torus = shallowRef(null)
const camera = shallowRef(null)
const light = shallowRef(null)

const torusColor = ref('#ff7f00')

const { onLoop } = useRenderLoop()
onLoop(({ delta, elapsed }) => {
  if (camera.value) {
    const radius = 5
    const speed = 0.3

    const angle = elapsed * speed
    const x = Math.cos(angle) * radius
    const z = Math.sin(angle) * radius

    camera.value.position.set(x, 0, z)

    // look at the torus
    camera.value.lookAt(0, 0, 0)

    // have the light point at the torus
    light.value.position.set(x, 0, z)
    light.value.lookAt(0, 0, 0)

    // and have the light get brighter and brighter
    light.value.intensity = Math.sin(elapsed * 2) * 2 + 2

  }
})
</script>