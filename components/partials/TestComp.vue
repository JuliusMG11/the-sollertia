<template>
  <div class="big-container">
    <canvas class="canvas" ref="canvasRef"></canvas>
  </div>
</template>


<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';


const containerRef = ref(null);
const canvasRef = ref(null);
const imageRef = ref(null);
const image = 'https://picsum.photos/800/600';

let camera, scene, renderer;
let mouseX = 0, mouseY = 0;

onMounted(() => {
  const width = window.innerWidth;
  const height = window.innerHeight;

  // Initialize Three.js components
  camera = new THREE.PerspectiveCamera(11, width / height, 1, 200);
  camera.position.z = 12;

  scene = new THREE.Scene();


  const light = new THREE.PointLight(0xffffff, 1, 0);
  light.position.set(0, 0, 5);
  scene.add(light);

  const texture = new THREE.TextureLoader().load(image);
  const geometry = new THREE.PlaneGeometry(1, 1);
  const material = new THREE.MeshBasicMaterial({ map: texture, transparent: true });
  const plane = new THREE.Mesh(geometry, material);
  plane.position.set(0, 0, -2);
  scene.add(plane);

  renderer = new THREE.WebGLRenderer({ canvas: canvasRef.value });
  renderer.setSize(width, height);

  // Initialize scroll listener
  // window.addEventListener('scroll', handleScroll);
  document.body.onscroll = moveCamera;
  animate();
});

onUnmounted(() => {
  // Remove scroll listener
  // window.removeEventListener('scroll', handleScroll);
  
});

const handleScroll = () => {
  //const scrollTop = window.pageYOffset;
  // const elementTop = containerRef.value.getBoundingClientRect().top + scrollTop;
  // const elementHeight = containerRef.value.offsetHeight;
  // const viewHeight = window.innerHeight;

  // Calculate the distance between the element and the center of the viewport
  // const distance = (scrollTop + viewHeight / 1);

  // Set the image scale based on the distance
  // const scale = 1 + Math.min(Math.max(Math.abs(distance) / viewHeight, 0), 1);
  // image.value.style.transform = `scale(${scale})`;

  // Move the camera based on the distance
  // camera.position.z = t * -0.01;
  // camera.position.x = mouseX + distance * 0.2;
  // camera.position.y = mouseY + distance * 0.2;
  // camera.lookAt(scene.position);


}

const moveCamera = () => {
    const t = document.body.getBoundingClientRect().top;
     
    // plane.rotation.x += 0.01;

    camera.position.z = t * -0.01;
    camera.position.x = t * -0.0002;
    camera.rotation.y = t * -0.0002;

}   


const animate = () => {
  requestAnimationFrame(animate);

  mouseX += 0.2;
  mouseY += 0.2;

  renderer.render(scene, camera);
}
</script>

<style>
.big-container {
  position: relative;
  height: 400vh;
}

.container {
  position: relative;
  height: 400px;
}

.canvas {
  position: fixed;
  top: 0;
  left: 0;
  z-index: -1;
}

.image {
  position: relative;
  max-width: 100%;
  max-height: 100%;
  transition: transform 0.3s ease-out;
}
</style>
