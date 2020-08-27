<script>
	import { scaleLinear } from 'd3-scale';
	import { extent } from 'd3-array';
	import { line } from 'd3-shape';

	import Axis from './Axis.svelte';

	export let data;

	let width;

	const height = 400;
	const margin = 50;

	$: xScale = scaleLinear()
		.domain(extent(data, d => d.x))
		.range([margin, width - margin]);

	$: yScale = scaleLinear()
		.domain(extent(data, d => d.y))
		.range([height - margin, margin]);

	$: lineGenerator = line()
		.x(d => xScale(d.x))
		.y(d => yScale(d.y));

	const reveal = (node, { duration }) => {
		if (!xScale || !yScale) return;
		const length = node.getTotalLength();
		node.style.strokeDasharray = length;
		return {
			duration,
			css: (t, u) => `stroke-dashoffset: ${u * length}`
		};
	}
</script>

<div class='line-chart' bind:clientWidth={width}>
	{#if width}
		<svg width={width} height={height}>
			<Axis {width} {height} {margin} scale={xScale} position='bottom' />
			<Axis {width} {height} {margin} scale={yScale} position='left' />

				<path
					d={lineGenerator(data)}
					stroke='rebeccapurple'
					stroke-width={2}
					stroke-linecap='round'
					fill='none'
					in:reveal={{ duration: 3000 }}
				/>
		</svg>
	{/if}
</div>

<style>
	div {
		width: 100%;
	}
</style>
