<template>
  <canvas ref="canvas"></canvas>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue'
import * as THREE from 'three'

export default defineComponent({
  setup() {
    const canvasRef = ref(null)
    let renderer, camera, scene, clock, texture1, texture2, material1, material2, geometry1, geometry2, mesh1, mesh2

    const init = () => {
      renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true,
        alpha: true
      })

      camera = new THREE.PerspectiveCamera(50, window.innerWidth / window.innerHeight, 0.1, 1000)
      camera.position.z = 5

      scene = new THREE.Scene()

      clock = new THREE.Clock()

      texture1 = new THREE.TextureLoader().load('https://picsum.photos/800/600', animate)
      texture2 = new THREE.TextureLoader().load('https://picsum.photos/800/600', animate)

      material1 = new THREE.MeshBasicMaterial({ map: texture1 })
      material2 = new THREE.MeshBasicMaterial({ map: texture2 })

      geometry1 = new THREE.PlaneGeometry(2, 2)
      geometry2 = new THREE.PlaneGeometry(2, 2)

      mesh1 = new THREE.Mesh(geometry1, material1)
      mesh2 = new THREE.Mesh(geometry2, material2)

      scene.add(mesh1)
      scene.add(mesh2)

      renderer.setClearColor(0xffffff, 0)
      renderer.setSize(window.innerWidth, window.innerHeight)
      renderer.setPixelRatio(window.devicePixelRatio)
    }

    const animate = () => {
      const elapsed = clock.getElapsedTime()

      mesh1.material.opacity = THREE.MathUtils.lerp(1, 0, elapsed / 3)
      mesh1.scale.set(
        THREE.MathUtils.lerp(1, 2, elapsed / 3),
        THREE.MathUtils.lerp(1, 2, elapsed / 3),
        1
      )

      mesh2.material.opacity = THREE.MathUtils.lerp(0, 1, elapsed / 3)
      mesh2.scale.set(
        THREE.MathUtils.lerp(2, 1, elapsed / 3),
        THREE.MathUtils.lerp(2, 1, elapsed / 3),
        1
      )

      renderer.render(scene, camera)

      if (elapsed < 3) {
        requestAnimationFrame(animate)
      }
    }

    onMounted(() => {
      init()
    })

    return { canvasRef }
  }
})
</script>

<style>
canvas {
  width: 100%;
  height: 200vh;
  display: block;
}
</style>