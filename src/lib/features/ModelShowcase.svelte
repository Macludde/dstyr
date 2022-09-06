<script lang="ts">
	import { onMount } from 'svelte';
	import * as THREE from 'three';
	import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';

	export let speed = 2;

	// Initialize scene-wide variables
	let el: HTMLCanvasElement;
	const scene = new THREE.Scene();
	const camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);
	let renderer: THREE.WebGLRenderer;
	const loader = new GLTFLoader();
	let model: any;

	const MODEL_MIN_Y = -2;
	const MODEL_MAX_Y = 5;

	// Lights
	scene.add(new THREE.AmbientLight(0x404040, 3));
	const light = new THREE.DirectionalLight(0xffffff, 1.25);
	light.position.set(-2, -1, 10);
	light.rotation.set(0, 0, Math.PI / 4);
	light.castShadow = true;
	scene.add(light);

	// Setup camera and scene
	camera.position.set(0, 0, 4);
	scene.background = new THREE.Color(0x444444);

	// Rotate model
	const animate = () => {
		requestAnimationFrame(animate);
		model.rotation.z += speed / 100;
		if (speed > 40 && model.position.y < MODEL_MAX_Y) {
			model.position.y += (speed - 40) / 100;
			if (model.position.y > MODEL_MAX_Y) {
				model.position.y = MODEL_MAX_Y;
			}
		} else if (speed < 40 && model.position.y > MODEL_MIN_Y) {
			model.position.y -= (40 - speed) / 100;
			if (model.position.y < MODEL_MIN_Y) {
				model.position.y = MODEL_MIN_Y;
			}
		}
		renderer.render(scene, camera);
	};

	// Setup scene
	const createScene = (el: HTMLCanvasElement) => {
		loader.load('models/model.gltf', (gltf) => {
			console.log(gltf);
			model = gltf.scene.children[0];
			model.scale.set(0.05, 0.05, 0.05);
			model.position.y = MODEL_MIN_Y;
			model.rotation.x = (-3 * Math.PI) / 8;
			scene.add(model);
		});
		renderer = new THREE.WebGLRenderer({ antialias: true, canvas: el });
		renderer.setSize(400, 400);
		animate();
	};

	onMount(() => {
		createScene(el);
	});
</script>

<div class="centered">
	<canvas bind:this={el} />
	<label for="speed">Speed</label>
	<input type="range" min="1" max="50" name="speed" id="speed" bind:value={speed} />
	{speed}
</div>

<style>
	.centered {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 0 auto;
		text-align: center;
	}
</style>
