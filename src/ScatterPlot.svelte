<script>
	import { scaleLinear, scaleLog, scaleSqrt } from 'd3-scale';
	import { extent } from 'd3-array';
	import { select } from 'd3-selection';

	import Axis from './Axis.svelte';

	export let data;

	const height = 400;
	const margin = 40;

	let width;

	$: xScale = scaleLog()
		.domain(extent(data, d => +d.gdp / +d.population))
		.range([margin, width - margin]);

	$: yScale = scaleLinear()
		.domain(extent(data, d => +d.life_expectancy))
		.range([height - margin, margin]);

	$: radiusScale = scaleSqrt()
		.domain(extent(data, d => +d.population))
		.range([2, 50]);

	const reveal = (node, { duration }) => {
		const radius = select(node).attr('r');
		return {
			duration,
			tick: (t) => select(node).attr('r', t * radius)
		};
	}
</script>

<div class='scatter-plot' bind:clientWidth={width}>
	{#if width}
		<svg width={width} height={height}>
			<Axis {width} {height} {margin} scale={xScale} position='bottom' />
			<Axis {width} {height} {margin} scale={yScale} position='left' />
			{#each data as d}
				<circle
					cx={xScale(+d.gdp / +d.population)}
					cy={yScale(+d.life_expectancy)}
					r={radiusScale(+d.population)}
					fill='rebeccapurple'
					in:reveal={{ duration: 1000 }}
				/>
			{/each}
		</svg>
	{/if}
</div>

<style>
	circle {
		opacity: 0.9;
	}
</style>
