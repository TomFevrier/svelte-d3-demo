<script>
	import { select, selectAll } from 'd3-selection';
	import { axisBottom, axisLeft } from 'd3-axis';

	export let width;
	export let height;
	export let margin;
	export let position;
	export let scale;

	let transform;
	let g;

	$: {
		select(g).selectAll('*').remove();

		let axis;
		switch(position) {
			case 'bottom':
				axis = axisBottom(scale).tickSizeOuter(0);
				transform = `translate(0, ${height - margin})`;
				break;
			case 'left':
				axis = axisLeft(scale).tickSizeOuter(0);
				transform = `translate(${margin}, 0)`;
		}

		select(g).call(axis);
	}
</script>

<g class='axis' bind:this={g} {transform} />
