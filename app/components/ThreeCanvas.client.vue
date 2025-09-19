<script setup lang="ts">
import type {BufferGeometry} from "three";
import {
  BoxGeometry,
  DirectionalLight,
  Mesh,
  MeshPhongMaterial,
  PerspectiveCamera,
  Scene,
  WebGLRenderer
} from "three";

const canvasRef: Ref<HTMLCanvasElement | null> = ref(null);

let renderer: WebGLRenderer | null = null;
let camera: PerspectiveCamera;
const scene = new Scene();

const makeInstance = (geometry: BufferGeometry, color: string | number, x: number) => {
  const material = new MeshPhongMaterial({color});

  const mesh = new Mesh(geometry, material);
  scene.add(mesh);

  mesh.position.x = x;

  return mesh;
}

const meshes = [
  makeInstance(new BoxGeometry(1, 1, 1), 0x44aa88, 0),
  makeInstance(new BoxGeometry(1, 1, 1), 0x8844aa, -2),
  makeInstance(new BoxGeometry(1, 1, 1), 0xaa8844, 2),
];

// LIGHT
const light = new DirectionalLight(0xffffff, 3);
light.position.set(-1, 2, 4);
scene.add(light);

function animate() {
  if (!renderer) return;
  if (resizeRendererToDisplaySize(renderer) && canvasRef.value) {
    camera.aspect = canvasRef.value.clientWidth / canvasRef.value.clientHeight;
    camera.updateProjectionMatrix();
  }

  meshes.forEach((mesh) => {
    mesh.rotation.x += 0.01;
    mesh.rotation.y += 0.01;
  });

  renderer.render(scene, camera);
}

onMounted(() => {
  if (!canvasRef.value) return;

  camera = new PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
  );
  camera.position.z = 5;

  renderer = new WebGLRenderer({canvas: canvasRef.value});
  renderer.setAnimationLoop(animate);
});

const changeMeshColor = (index: number, e: Event) => {
  const input = e.target as HTMLInputElement
  meshes[index]?.material.color.set(input.value)
}


</script>

<template>
  <div>
    <div class="controls">
      <div v-for="(mesh, index) in meshes" :key="index" class="color-control">
        <label for="color">Выберите цвет</label>
        <input type="color" :value="`#${mesh.material.color.getHexString()}`" @input="(e) => changeMeshColor(index, e)">
      </div>
    </div>
    <canvas ref="canvasRef"/>
  </div>
</template>

<style>
canvas {
  width: 100vw;
  height: 100vh;
  display: block;
}

.controls {
  position: absolute;

  label {
    color: #ffffff;
  }
}
</style>
