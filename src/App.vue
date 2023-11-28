<template>
  <div id="app"></div>
</template>

<script>
import { ref, onMounted } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

export default {
  name: 'App',
  setup() {
    const init = () => {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.getElementById('app').appendChild(renderer.domElement);

      // Set up camera position
      camera.position.z = 10;

      // Set up controls
      const controls = new OrbitControls(camera, renderer.domElement);

      // Add ambient light
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
      scene.add(ambientLight);

      // Add directional light
      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
      directionalLight.position.set(5, 5, 5);
      scene.add(directionalLight);

      // Create a torus knot
      const torusKnotGeometry = new THREE.TorusKnotGeometry(3, 1, 100, 16);
      const torusKnotMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 });
      const torusKnot = new THREE.Mesh(torusKnotGeometry, torusKnotMaterial);
      scene.add(torusKnot);

      // Create a sphere
      const sphereGeometry = new THREE.SphereGeometry(2, 32, 32);
      const sphereMaterial = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
      const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
      sphere.position.set(6, 0, 0);
      scene.add(sphere);

      // Create a cone
      const coneGeometry = new THREE.ConeGeometry(2, 4, 32);
      const coneMaterial = new THREE.MeshStandardMaterial({ color: 0x0000ff });
      const cone = new THREE.Mesh(coneGeometry, coneMaterial);
      cone.position.set(-6, 0, 0);
      scene.add(cone);

      // Render loop
      const animate = () => {
        requestAnimationFrame(animate);

        // Rotate the geometries (optional)
        torusKnot.rotation.x += 0.01;
        torusKnot.rotation.y += 0.01;
        sphere.rotation.x += 0.01;
        sphere.rotation.y += 0.01;
        cone.rotation.x += 0.01;
        cone.rotation.y += 0.01;

        controls.update();

        renderer.render(scene, camera);
      };

      animate();
    };

    onMounted(() => {
      init();
    });

    return {};
  },
};
</script>

<style scoped>
/* Your component styles go here */
</style>
