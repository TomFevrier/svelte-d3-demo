<script>
	import { csv } from 'd3-fetch';

	import ScatterPlot from './ScatterPlot.svelte';
	import LineChart from './LineChart.svelte';

	let scatterPlotData;
	csv('world_bank.csv').then(data => scatterPlotData = data);

	let lineChartData;
	csv('DCOILBRENTEU.csv').then(data => lineChartData = data);

</script>

<h1>Svelte × D3</h1>
<main>
	{#if scatterPlotData}
		<div class='chart'>
			<h3>Espérance de vie en fonction du PIB par habitant (2018)</h3>
			<ScatterPlot data={scatterPlotData} />
			<p>Source&nbsp;: Banque Mondiale</p>
		</div>
	{/if}
	{#if lineChartData}
		<div class='chart'>
			<h3>Évolution du prix du barril de pétrole brut en Europe (en dollars)</h3>
			<LineChart data={lineChartData} />
			<p>Source&nbsp;: Federal Reserve Bank of St Louis</p>
		</div>
	{/if}
</main>

<style>
	h1 {
		font: 2.5rem 'Alata', sans-serif;
		text-align: center;
		margin: 1rem 0;
	}

	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		max-width: 48rem;
		margin: 0 auto;
	}

	.chart {
		width: 100%;
		position: relative;
		margin-bottom: 1rem;
	}

	.chart h3 {
		position: absolute;
		width: 100%;
		top: 0;
		left: 50%;
		transform: translateX(-50%);
		margin: 0;
		font-size: 1.1rem;
		text-align: center;
		z-index: 42;
	}

	.chart p {
		position: absolute;
		width: 100%;
		bottom: 0;
		right: 0;
		margin: 0;
		font-size: 0.9rem;
		font-style: italic;
		text-align: right;
		z-index: 42;
	}
</style>
