<script>
	import { onMount } from 'svelte';

	let canvas;
	let currentRad = 0;

	// const offset = -Math.PI / 2; // start from the top
	const offset = 0; // start from the top
	let canvasDiameter = 500;
	let canvasCenter = canvasDiameter / 2;
	let lineWidth = 125;

	let { total, rows } = $props();
	let sectors = [];

	onMount(() => {
		if (!canvas) return;

		resetCanvas();

		canvas.addEventListener('mousemove', handleMousemove);
	});

	$effect(() => {
		resetCanvas();

		rows.forEach((row) => {
			draw(row);
		});
	});

	function handleMousemove(event) {
		// if (sectors.length === 0) return;
		// if canvas is not on the left top
		// const rect = canvas.getBoundingClientRect();
		// console.log(rect);

		const x = event.clientX;
		const y = event.clientY;
		const dx = x - canvasCenter;
		const dy = y - canvasCenter;

		let theta = Math.atan2(dy, dx);
		if (theta < 0) theta += Math.PI * 2;

		const test = sectors[0];
		if (test.start <= theta && test.end >= theta) console.log('intersecting first section');
	}

	function draw(obj) {
		var ctx = canvas.getContext('2d');

		ctx.lineWidth = lineWidth;
		ctx.strokeStyle = obj.c;
		let radians = (Math.PI * obj.p) / 50;
		sectors.push({ ...obj, start: currentRad, end: currentRad + radians });

		ctx.beginPath();
		ctx.arc(
			canvasDiameter / 2,
			canvasDiameter / 2,
			(canvasDiameter - lineWidth) / 2,
			currentRad + offset,
			currentRad + radians + offset,
			false
		);
		ctx.stroke();

		currentRad = currentRad + radians;
	}

	function initialDraw() {
		var ctx = canvas.getContext('2d');

		ctx.lineWidth = lineWidth;
		ctx.strokeStyle = 'grey';
		let radians = Math.PI * 2;

		ctx.beginPath();
		ctx.arc(
			canvasDiameter / 2,
			canvasDiameter / 2,
			(canvasDiameter - lineWidth) / 2,
			currentRad + offset,
			currentRad + radians + offset,
			false
		);
		ctx.stroke();
	}

	function resetCanvas() {
		var ctx = canvas.getContext('2d');
		ctx.clearRect(0, 0, canvasDiameter, canvasDiameter);
		currentRad = 0;
		sectors = [];
		initialDraw();
	}
</script>

<div class="donut">
	<canvas bind:this={canvas} width={canvasDiameter} height={canvasDiameter}></canvas>
	<div class="total">total: {total}</div>
</div>

<style lang="scss">
	.donut {
		position: relative;

		> .total {
			position: absolute;
			left: 50%;
			top: 50%;
			transform: translateX(-50%) translateY(-50%);
		}
	}
</style>
