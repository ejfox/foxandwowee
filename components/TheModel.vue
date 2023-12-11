<script setup>
import { useGLTF } from '@tresjs/cientos';

// const { scene: model } = await useGLTF(
//   'https://raw.githubusercontent.com/Tresjs/assets/main/models/gltf/blender-cube.glb',
//   { draco: true }
// );
</script>

<template>
  <TresMesh>
    <!-- <TresTorusGeometry :args="[1, 0.5, 16, 32]" /> -->
    <TresSphereGeometry :args="[1, 32, 32]" />
    <!-- <TresMeshBasicMaterial color="red" /> -->
    <TresMeshPhongMaterial :color="0xff7f00" />
  </TresMesh>
</template>
