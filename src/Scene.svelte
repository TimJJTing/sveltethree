<script>
  import { onMount, afterUpdate } from "svelte";
  import * as THREE from "three";
  // svelte props
  export let color = 0x00ff00;
  export let usePointLight = true;
  export let rotateCube = true;
  export let rotateLight = false;
  // threejs
  let el;
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0x323237);
  let renderer;
  // camera
  const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.z = 10;
  // cube
  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshStandardMaterial({ color });
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);
  // lights
  const pointLight = new THREE.PointLight(0xffffff, 5, 100);
  const pointLightHelper = new THREE.PointLightHelper(pointLight, 1);
  const ambientLight = new THREE.AmbientLight(0xd6d6d6); // soft white light
  // render loop
  const animate = () => {
    requestAnimationFrame(animate);
    if (rotateCube) {
      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;
    }
    if (rotateLight) {
      let time = performance.now() * 0.001;
      pointLight.position.set(5 * Math.sin(time), 0, 5 * Math.cos(time));
    }
    renderer.render(scene, camera);
  };
  // resizer
  const resize = () => {
    renderer.setSize(window.innerWidth, window.innerHeight);
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
  };
  window.addEventListener("resize", resize);
  // entrypoint
  const createScene = (el) => {
    renderer = new THREE.WebGLRenderer({ antialias: true, canvas: el });
    resize();
    animate();
  };

  // svelte lifecycle
  onMount(() => {
    createScene(el);
  });
  afterUpdate(() => {
    if (material) material.color.set(color);
    if (usePointLight) {
      scene.add(pointLight);
      scene.add(ambientLight);
      scene.add(pointLightHelper);
    } else {
      scene.remove(pointLight);
      scene.remove(ambientLight);
      scene.remove(pointLightHelper);
    }
  });
</script>

<canvas bind:this={el} />

<style>
  canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
    z-index: 0;
  }
</style>
