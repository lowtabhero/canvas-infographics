<script>
	import DonutCanvas from '$lib/components/DonutCanvas.svelte';

	let valueInput, colorInput;

	let colors = ['red', 'blue', 'green', 'orange', 'purple', 'pink'];

	let totalValue = $state(100);
	let rows = $state([]);

	function handleAddRow() {
		const v = valueInput.value;
		const p = (totalValue / 100) * v;
		const c = colorInput.value;
		if (!v || !p || !c) return;

		rows.push({ p: p, v: v, c: c });
	}
</script>

<div class="container">
	<DonutCanvas total={totalValue} {rows} />
	<div class="input-container">
		<input type="number" placeholder="Input number here" bind:this={valueInput} />
		<input type="color" bind:this={colorInput} />
		<button onclick={handleAddRow}>Add row</button>
	</div>
</div>

<style lang="scss">
	.container {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		gap: 40px;

		> .input-container {
			flex-grow: 1;
			flex-basis: 300px;
			border: 1px solid black;
			padding: 1rem;
		}
	}
</style>
