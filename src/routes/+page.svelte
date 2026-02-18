<script>
	import { onMount } from 'svelte';

	let canvas, rowInput;
	const offset = -Math.PI / 2; // start from top
	let currentRad = 0;

	let canvasDiameter = 500;
	let lineWidth = 125;

	let totalValue = 100;
	let rows = $state([]);
	let colors = ['red', 'blue', 'green', 'orange', 'purple', 'pink'];

	onMount(() => {
		draw({ p: 100, c: 'grey' });
	});

	function handleAddRow() {
		const v = rowInput.value;
		const p = (totalValue / 100) * v;
		const row = { p: p, v: rowInput.value, c: colors[rows.length] };
		rows.push(row);
		draw(row);
	}

	function drawRows() {
		// reset canvas and draw rows
	}

	function draw(obj) {
		var ctx = canvas.getContext('2d');

		ctx.lineWidth = lineWidth;
		ctx.strokeStyle = obj.c;
		let radians = (Math.PI * obj.p) / 50;

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
</script>

<div class="container">
	<div class="donut">
		<canvas bind:this={canvas} width={canvasDiameter} height={canvasDiameter}></canvas>
		<div class="total">total: {totalValue}</div>
	</div>
	<div class="input-container">
		<input type="text" placeholder="Input row here" bind:this={rowInput} />
		<button onclick={handleAddRow}>Add row</button>
	</div>
</div>

<style lang="scss">
	.container {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		gap: 40px;

		> .donut {
			position: relative;

			> .total {
				position: absolute;
				left: 50%;
				top: 50%;
				transform: translateX(-50%) translateY(-50%);
			}
		}

		> .input-container {
			flex-grow: 1;
			flex-basis: 300px;
			border: 1px solid black;
		}
	}
</style>
