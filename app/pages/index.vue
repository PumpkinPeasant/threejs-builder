<script setup lang="ts">
import {Mesh, MeshBasicMaterial, PerspectiveCamera, Scene, SphereGeometry, WebGLRenderer} from "three";

const canvasRef: Ref<HTMLCanvasElement | null> = ref(null)
let renderer: WebGLRenderer;
const scene = new Scene();
const camera = new PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(0, 0, 2);

scene.add(camera);

const sphere = new Mesh(
    new SphereGeometry(1, 32, 32),
    new MeshBasicMaterial({color: 0x008080})
)

scene.add(sphere);


const setRenderer = () => {
  if (canvasRef.value) {
    renderer = new WebGLRenderer({canvas: canvasRef.value,});
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.render(scene, camera);
  }
}

onMounted(() => {
  setRenderer();
})
</script>

<template>
  <div>
    <canvas ref="canvasRef"/>
  </div>
</template>

<style scoped>

</style>