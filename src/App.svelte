<script>
	import { csv } from 'd3-fetch';

	import ScatterPlot from './ScatterPlot.svelte';
	import LineChart from './LineChart.svelte';

	let data1;
	// https://www.gapminder.org/fw/world-health-chart/
	csv('banque_mondiale.csv').then(data => data1 = data);

	let data2;
	setTimeout(() => {
		data2 = new Array(100).fill().map((_, i) => ({ x: i, y: Math.random() }));
	}, 100);

</script>

<h1>Svelte × D3</h1>
<main>
	{#if data1}
		<ScatterPlot data={data1} />
	{/if}
	{#if data2}
		<LineChart data={data2} />
	{/if}
</main>

<style>
	h1 {
		font: 3rem 'Alata', sans-serif;
		text-align: center;
		margin: 1rem 0;
	}

	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		/* width: 100%; */
		max-width: 48rem;
		margin: 0 auto;
	}

</style>
