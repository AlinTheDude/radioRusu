<template>
    <div ref="container"></div>
  </template>
  
  <script>
  import * as THREE from 'three';
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
  
  export default {
    name: 'GlobeView',
    data() {
      return {
        radioStations: [],
      };
    },
    mounted() {
      this.fetchRadioStations();
    },
    methods: {
      fetchRadioStations() {
        fetch('https://nl1.api.radio-browser.info/json/stations/search?limit=1000&country=Italy')
          .then(response => response.json())
          .then(data => {
            this.radioStations = data;
            this.createGlobe();
          })
          .catch(error => {
            console.error('Error fetching radio stations:', error);
          });
      },
      createGlobe() {
        // Create Three.js scene, camera, and renderer
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        this.$refs.container.appendChild(renderer.domElement);
  
        // Create a sphere geometry for the globe
        const geometry = new THREE.SphereGeometry(5, 32, 32);
        const texture = new THREE.TextureLoader().load(require('@/assets/earth.jpg')); // Load the earth texture
        const material = new THREE.MeshBasicMaterial({ map: texture }); // Apply texture to the material
        const globe = new THREE.Mesh(geometry, material);
        scene.add(globe);
  
        // Position the camera to focus on Italy
        camera.position.setFromSphericalCoords(10, (41 * Math.PI) / 180, (12.5 * Math.PI) / 180); // Latitude and longitude for Italy
        camera.lookAt(0, 0, 0);
  
        // Add OrbitControls to enable rotation
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.update();
  
        // Add radio markers
        const radioMarkerGeometry = new THREE.SphereGeometry(0.2, 8, 8); // Increase the size of the markers
        const radioMarkerMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
        this.radioStations.forEach(station => {
          const latitude = station.lat * (Math.PI / 180);
          const longitude = station.lon * (Math.PI / 180);
          const marker = new THREE.Mesh(radioMarkerGeometry, radioMarkerMaterial);
          marker.position.setFromSphericalCoords(5.2, latitude, longitude); // Slightly above the surface of the globe
          globe.add(marker);
        });
  
        // Render loop
        const animate = () => {
          requestAnimationFrame(animate);
          controls.update();
          renderer.render(scene, camera);
        };
        animate();
      },
    },
  };
  </script>
  
  <style scoped>
    #container {
      width: 100%;
      height: 100%;
    }
  </style>
  