<script lang="ts">
  import * as THREE from "three";
  import { onMount } from "svelte";
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
  $: innerWidth = 0;
  $: innerHeight = 0;

  // scene
  const scene = new THREE.Scene();

  // Create our sphere
  const geometry = new THREE.SphereGeometry(3, 64, 64);
  const material = new THREE.MeshStandardMaterial({
    color: "#00ff83",
  });
  const mesh = new THREE.Mesh(geometry, material);
  scene.add(mesh);

  //light
  const light = new THREE.PointLight(0xffffff, 120, 100);
  light.position.set(0, 5, 10);
  scene.add(light);
  let canvas: HTMLCanvasElement;
  onMount(() => {
    const camera = new THREE.PerspectiveCamera(
      45,
      innerWidth / innerHeight,
      0.1,
      100
    );
    camera.position.z = 15;
    scene.add(camera);
    // camera

    const renderer = new THREE.WebGLRenderer({ canvas });

    renderer.setSize(innerWidth, innerHeight);

    renderer.render(scene, camera);

    //controls
    const controls = new OrbitControls(camera, canvas);

    window.addEventListener("resize", () => {
      innerWidth = window.innerWidth;
      innerHeight = window.innerHeight;
      // update camera
      camera.aspect = innerWidth / innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(innerWidth, innerHeight);
    });
    const loop = () => {
      renderer.render(scene, camera);
      window.requestAnimationFrame(loop);
    };
    loop();
  });
</script>

<svelte:window bind:innerWidth bind:innerHeight />

<canvas bind:this={canvas} />
