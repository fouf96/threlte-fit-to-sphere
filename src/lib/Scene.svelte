<script module lang="ts">
	let installed = false;
</script>

<script lang="ts">
	import { onMount } from 'svelte';
	import * as THREE from 'three';
	import { T, useThrelte, useStage, useTask } from '@threlte/core';
	import CameraControls from 'camera-controls';
	import {
		Box3,
		Matrix4,
		Quaternion,
		Raycaster,
		Sphere,
		Spherical,
		Vector2,
		Vector3,
		Vector4
	} from 'three';
	const subsetOfTHREE = {
		Vector2,
		Vector3,
		Vector4,
		Quaternion,
		Matrix4,
		Spherical,
		Box3,
		Sphere,
		Raycaster
	};

	if (!installed) {
		CameraControls.install({ THREE: subsetOfTHREE });
		installed = true;
	}

	const { renderer, invalidate, renderStage } = useThrelte();

	const conditionalStage = useStage('after-render', {
		after: renderStage,
		callback: (delta, runTasks) => {
			if (created) {
				runTasks();
				created = false;
			}
		}
	});

	let cameraControls;
	let camera = $state();
	let object = $state();
	let created = false;
	
	let sphereRadiusPercentage = 0.6;
	function helperSphere(object3d, percentage) {
		const boundingSphere = CameraControls.createBoundingSphere(object3d);
		const sphereHelper = new THREE.Mesh(
			new THREE.SphereGeometry(boundingSphere.radius * percentage, 16, 16)
		);
		sphereHelper.position.copy(boundingSphere.center);
		return sphereHelper;
	}

	let fitObject = () => {
		cameraControls.fitToSphere(
			helperSphere(object, sphereRadiusPercentage),
			true
		);
		console.log('Moving camera to fit object into view');
	};

	useTask(
		'main-task',
		(delta) => {
			const updated = cameraControls.update(delta);
			if (updated) invalidate();
		},
		{
			autoInvalidate: false
		}
	);

	useTask(
		'fit-camera-once',
		(delta) => {
			fitObject();
		},
		{ stage: conditionalStage }
	);

	onMount(() => {
		window.addEventListener('resize', fitObject);
	});
</script>

<T.PerspectiveCamera
	makeDefault
	position={[-8.3989, 17.6849, 21.8665]}
	fov={45}
	bind:ref={camera}
	oncreate={() => {
		cameraControls = new CameraControls(camera, renderer?.domElement);
	}}
	visible
	rotation={[-0.6494, -0.2093, -0.1565]}
	scale={[1, 1, 1]}
	near={0.1}
	far={2000}
	zoom={1}
	filmOffset={-5}
	filmGauge={35}
></T.PerspectiveCamera>

<T.DirectionalLight
	intensity={1.8}
	position={[0, 10, 0]}
	castShadow
	shadow.bias={0.0001}
/>
<T.AmbientLight intensity={0.2} />

{#await import('$lib/Spaceship.svelte') then { default: Spaceship }}
	<T.Group bind:ref={object}>
		<Spaceship
			oncreate={() => {
				created = true;
			}}
		/>
	</T.Group>
{/await}

