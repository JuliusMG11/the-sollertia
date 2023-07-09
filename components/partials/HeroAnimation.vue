<template>
<div class="home-section">
  <div class="big-container" ref="bigContainer">
    <canvas class="canvas" ref="canvasRef"></canvas>
     <!-- <div class="background-one absolute">
      <img src="@/assets/img/background-city.jpg" alt="">
    </div> -->
    <div class="counter">
      <DesignTimeCounter />
    </div>
    <div class="big-title absolute">
        <strong>
            Lorem, ipsum dolor sit amet consectetur adipisicing elit. Nostrum, iure provident 
            blanditiis impedit officiis laborum labore voluptates vero omnis 
            aliquid mollitia excepturi minus qui iste nobis tenetur deleniti at. Inventore.
            Lorem, ipsum dolor sit amet consectetur adipisicing elit. Nostrum, iure provident 
            blanditiis impedit officiis laborum labore voluptates vero omnis 
            aliquid mollitia excepturi minus qui iste nobis tenetur deleniti at. Inventore.
        </strong>
      </div>
  </div>
  <div class="container-to-right" ref="contentRight">
     <div class="section" id="section-1">
        Section 1
      </div>
      <div class="section" id="section-2">
        Section 2
      </div>
      <div class="section" id="section-3">
        Section 3
      </div>
   </div>
</div>
</template>


<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
import { FontLoader } from '../../node_modules/three/examples/jsm/loaders/FontLoader';
import { TextGeometry } from '../../node_modules/three/examples/jsm/geometries/TextGeometry';
import { OrbitControls } from "https://unpkg.com/three@0.112/examples/jsm/controls/OrbitControls.js";

import { gsap } from 'gsap';
import { ScrollTrigger } from '../../node_modules/gsap/ScrollTrigger';

// import fontStyle from '~/assets/font/Unbounded_Regular.json.json'

// IMPORT IMAGES
import panoramaPub from '~/assets/img/panoramaPub.jpeg'
import avatar from '~/assets/img/avatar.png'
import imageBuilding1 from '~/assets/img/building1.jpg'
import imageBuilding2 from '~/assets/img/building2.jpg'
import imageMoon from '~/assets/img/moon.jpg'
import imageMoonTexture from '~/assets/img/normal.jpg'
import space from '~/assets/img/space.jpg'
import space2 from '~/assets/img/background-city.jpg'
import gameImageLoad from '~/assets/img/games.png'
import { Raycaster } from 'three';

const imageOne = ref(avatar)
const imageTwo = ref(imageBuilding2)
const gameImage = ref(gameImageLoad)

// THREE JS ANIMATION

const bigContainer = ref(null)
const containerRef = ref(null);
const canvasRef = ref(null);

let firstImage
let secondImage
let titleText
let moon
let cubeNft

let camera, scene, renderer, controls;
let mouseX = 0, mouseY = 0;

let aspectRatio = 0

// GSAP ANIMATION SCROLL TO RIGHT

const contentRight = ref(null)

// STARS
  let stars = ref([])
  let star
  const addStar = () => {
    const geometryStars = new THREE.SphereGeometry(0.009, 24, 24);
    const materialStars = new THREE.MeshStandardMaterial({ color: 0xffffff });
    star = new THREE.Mesh(geometryStars, materialStars);

    // const [x, y, z] = Array(3)
    //   .fill()
    //   .map(() => THREE.MathUtils.randFloatSpread(10));

    //   star.position.set(x, y, z);
      star.position.set(
        THREE.MathUtils.randFloatSpread(10),
        THREE.MathUtils.randFloatSpread(10),
        THREE.MathUtils.randFloatSpread(10)
      );
      stars.value.push(star);
      scene.add(star);
  }

  const scrollY = ref();

onMounted(() => {

  const width = window.innerWidth;
  const height = window.innerHeight;

  // Initialize Three.js components CAMERA
  camera = new THREE.PerspectiveCamera(11, width / height, 1, 1000);
  camera.position.z = 12;
  scene = new THREE.Scene();

    // LIGHTS
  // const light = new THREE.PointLight(0xffffff, 0.4, 1);
  // light.position.set(0, 0, 5);
  // scene.add(light);

  const ambientLight = new THREE.AmbientLight(0xffffff, 0.4, 1)
  ambientLight.position.set(1, 1, 1);
  scene.add(ambientLight)

  // POINT LIGHT
  const cubeLight = new THREE.PointLight( 0x6A2EFD, 1, 100 );
  cubeLight.position.set(-1, -0.6, -0.8);

  scene.add(cubeLight);

  // SPACE BACKGROUND
  // const spaceTexture = new THREE.TextureLoader().load(space2);
  // scene.background = spaceTexture;


  //  IMAGE ONE
  // const textureImageOne = new THREE.TextureLoader().load(imageOne.value);
  // const geometryImageOne = new THREE.PlaneGeometry(1.4, 1);
  // const materialImageOne = new THREE.MeshBasicMaterial({ map: textureImageOne, transparent: true });
  // firstImage = new THREE.Mesh(geometryImageOne, materialImageOne);
  // firstImage.position.set(-1, 0.1, 1);
  // scene.add(firstImage);

  // GAME IMAGE
  const cubeGameTexture = new THREE.TextureLoader().load(gameImage.value);
  const cubeGameMaterial = new THREE.MeshBasicMaterial( { map: cubeGameTexture, transparent: true } ); 

  const cubeGameGeometry = new THREE.PlaneGeometry( 1.4, 0.8 ); 
  cubeNft = new THREE.Mesh( cubeGameGeometry, cubeGameMaterial );
  cubeNft.position.x = -1.5;
  cubeNft.position.y = -0.8;
  cubeNft.position.z = -0.8; 
  // cubeNft.position.set(-1.5, -0.8, -0.8)
  cubeNft.rotation.y = 0.7;

  scene.add(cubeNft);

  // IMAGE TWO
  // const textureImageTwo = new THREE.TextureLoader().load(imageTwo.value);
  // const geometryImageTwo = new THREE.PlaneGeometry(1.8, 1);
  // const materialImageTwo = new THREE.MeshBasicMaterial({ map: textureImageTwo, transparent: true });
  // secondImage = new THREE.Mesh(geometryImageTwo, materialImageTwo);
  // secondImage.position.set(0.8, 0.1, -10);
  // scene.add(secondImage);

  // Moon
  const moonTexture = new THREE.TextureLoader().load(imageMoon);
  const normalTexture = new THREE.TextureLoader().load(imageMoonTexture);

  moon = new THREE.Mesh(
    new THREE.SphereGeometry(0.3, 32, 32),
    new THREE.MeshStandardMaterial({
      map: moonTexture,
      normalMap: normalTexture,
    })
  );

  scene.add(moon);
  moon.position.set(-2, 1, -2);


  // ADD STARS
  Array(200).fill().forEach(addStar);


  
  // Load font
  const fontLoader = new FontLoader();
  fontLoader.load(
    'https://cdn.jsdelivr.net/npm/three/examples/fonts/optimer_bold.typeface.json',
    (font) => {
      // Create text geometry
      const geometryText = new TextGeometry('The Sollertia', {
        font: font,
        size: 0.36,
        height: 0.3,
        curveSegments: 8,
        bevelEnabled: false,
        bevelThickness: 0.0008,
        bevelSize: 0.0008,
        bevelOffset: 0.001, 
        bevelSegments: 10
      })
      

      // Create material and mesh for the text
      const materialText = new THREE.MeshBasicMaterial({ color: 0xffffff })
      titleText = new THREE.Mesh(geometryText, materialText)
      titleText.position.y = 0.2;
      titleText.position.x = -1.4;
      titleText.position.z = -2.2;
      // Add Three.js objects to the scene
      scene.add(titleText)
    }
  )

    

  // RENDERER
  renderer = new THREE.WebGLRenderer({ canvas: canvasRef.value, preserveDrawingBuffer: true });
  renderer.setClearColor(0x000000, 0);
  renderer.setSize(width, height);
  renderer.setPixelRatio(window.devicePixelRatio)

  document.body.onscroll = moveCamera;
  animate();

  cubeNft.userData.clickable = true;


  // CREATE PANORAMATICK IMAGE BG
  const texturePanorama = new THREE.TextureLoader().load(panoramaPub);
  const sphereGeometry = new THREE.SphereGeometry(2.5, 32, 32);
  sphereGeometry.scale(-1, 1, 1);
  const sphereMaterial = new THREE.MeshBasicMaterial({ map: texturePanorama });

  controls = new OrbitControls(camera, canvasRef.value);
  controls.enableZoom = false;

  // Vytvorenie sférického objektu pre panoramatický obrázok
  const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
  scene.add(sphere);

  // CLICK ON CANVAS ELEMENT
  const raycaster = new THREE.Raycaster();
  const canvas = renderer.domElement;
  canvas.addEventListener('click', onClick, false);

  function onClick(event) {
    // Calculate normalized device coordinates (NDC) from mouse position
    const canvasBounds = canvasRef.value.getBoundingClientRect();
    const mouseX = ((event.clientX - canvasBounds.left) / canvasBounds.width) * 2 - 1;
    const mouseY = -((event.clientY - canvasBounds.top) / canvasBounds.height) * 2 + 1;

  // Set the raycaster's origin and direction
  const mouseCoords = new THREE.Vector2(mouseX, mouseY);
  raycaster.setFromCamera(mouseCoords, camera);

  // Find intersected objects
  const intersects = raycaster.intersectObjects(scene.children, true);

  // IF CLICK ON GAME ID
  if (intersects[0].object.id === 9) {
    // Handle the click on the intersected object(s)
    console.log('Clicked object GAME');
  }
}

  gsap.registerPlugin(ScrollTrigger);



  let sections = gsap.utils.toArray(".section");
  gsap.to(sections, {
    xPercent: -100 * (sections.length - 1),
    ease: "none",
    scrollTrigger: {
      trigger: contentRight.value,
      pin: true,
      scrub: 1,
      // snap: 1 / (sections.length - 1),
      end: () => "+=" + document.querySelector(".container-to-right").offsetWidth
    }
  });

  // Pridanie classy po scrollovani na 300px vysky
    // ScrollTrigger.create({
    //   trigger: bigContainer.value,
    //   start: 'top 400vh',
    //   onEnter: () => bigContainer.value.classList.add('activeSticky')
    // });

    // Odobratie classy po scrollovani na 500px vysky
    // ScrollTrigger.create({
    //   trigger: box,
    //   start: 'top 500px',
    //   onEnterBack: () => box.classList.remove('active')
    // });


});



// SCROLL ANIMATION
const moveCamera = () => {
    const t = document.body.getBoundingClientRect().top;

    console.log('TOTO JE SCROLL' + document.body.getBoundingClientRect().top)


    // FIRST IMAGE
    // firstImage.position.z = 0 + (t * -0.02);
    // firstImage.position.z = t * -0.02;
    // firstImage.position.x = -1 + (t * 0.0002);

    // SECOND IMAGE
    // secondImage.position.z = -10 + (t * -0.02);
    // secondImage.position.x = 0.8 + (t * 0.002);

    // TITLE TEXT
    titleText.position.y = 0.2 + (t * 0.002);
    titleText.position.x = -1.4 + (t * 0.002);
    titleText.position.z = -2.2 + (t * -0.02);


    // GAMES IMAGE
    cubeNft.position.x = -1.5 + (t * -0.02);
    cubeNft.position.y = -0.8 + (t * 0.002);
    cubeNft.position.z = -0.8 + (t * -0.02); 
    cubeNft.rotation.y = 0.7 + (t * 0.02);

    // MOON
    moon.position.z = -2 + (t * -0.05);

}   


const animate = () => {
  requestAnimationFrame(animate);
  // controls.update();
  
  mouseX += 0.2;
  mouseY += 0.2;

  // MOON ANIMATION
  moon.rotation.x += 0.01;
  moon.rotation.y += 0.01;

  // STARS FALLING
  stars.value.forEach((star) => {
    star.position.y -= 0.005;
    if (star.position.y < -5) {
      star.position.y = 5;
    }
  });

  renderer.render(scene, camera);
}
</script>

<style lang="scss" scoped>
.big-container {
  position: relative;
  height: 400vh;
}

.big-container.activeSticky {
  position: fixed;
  height: 100%;
  width: 100%;
  z-index: 1;
}
.big-title {
  top: 30%;
  z-index: 3;
  left: 25%;
  max-width: 740px;

  strong {
    font-size: 18px;
  }
}

.background-one {
  width: 100%;
  height: 100vh;
  left: 0px;
  top: -90px;
  z-index: 1;

  &::after {
    content: '';
    position: absolute;
    background: linear-gradient(0deg, rgba(0,0,0,1) 10%, rgba(0,0,0,0) 100%); 
    width: 100%;
    height: 60vh;
    bottom: 0px;
    left: 0px;
    z-index: 1;
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.container {
  position: relative;
  height: 400px;
}

.canvas {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 2;
}

.counter {
  position: absolute;
  top: 9%;
  // left: 0px;
  z-index:  4;
  transform: translateX(50%);
  right: 50%;
  padding: 16px 32px;
  background: rgba(18, 18, 18, 0.60);
  backdrop-filter: blur(7px);
  width: 500px;
  display: flex;
  justify-content: center;
}

.image {
  position: relative;
  max-width: 100%;
  max-height: 100%;
  transition: transform 0.3s ease-out;

  img {
    object-fit: cover;
  }
}

.container-to-right{

	width: 400%;
	height: 100vh;
	display: flex;
	flex-wrap: nowrap;
  }

  .section {
    height: 100vh;
    width: 100vw;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 5rem;
    color: white;
  }

  #section-1 {
    background-color: transparent;
  }

  #section-2 {
    background-color: transparent;
  }

  #section-3 {
    background-color: transparent;
  }

</style>
