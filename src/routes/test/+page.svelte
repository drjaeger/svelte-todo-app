<script>
	import { onMount } from 'svelte';
	import { quadInOut } from 'svelte/easing';
	import { tweened } from 'svelte/motion';

	let timeline = [
		{ year: 1995, label: 'DHTML' },
		{ year: 2006, label: 'jQuery' },
		{ year: 2010, label: 'Angular 1' },
		{ year: 2013, label: 'React' },
		{ year: 2019, label: 'Svelte3' }
	];
	const width = 900;

	let duration = 1000;
	let index = tweened(0, { duration, easing: quadInOut });
	let dotSize = tweened(3, { duration: 200, easing: quadInOut });

	onMount(() => {
		const interval = setInterval(next, 1500);

		return () => {
			clearInterval(interval);
		};
	});

	function next() {
		$dotSize = 3;
		if ($index < timeline.length - 1) {
			$index++;
		} else {
			$index = 0;
		}

		setTimeout(() => ($dotSize = 8), 500);
	}
</script>

<svg viewBox="{-60 + $index * 200} 0 {width / timeline.length} 120">
	<line x1="20" y1="95" x2={30 + $index * 200} y2="95" stroke="black" />

	{#each timeline as event, i}
		<g>
			<text class="label" x={25 + i * 40 * timeline.length} y="20" text-anchor="middle"
				>{event.label}</text
			>
			<text class="year" x={25 + i * 40 * timeline.length} y="60" text-anchor="middle"
				>{event.year}</text
			>
			<circle cx={25 + i * 40 * timeline.length} cy="95" r={$dotSize} />
		</g>
	{/each}
</svg>

<style>
	:global(body) {
		display: flex;
	}
	svg {
		overflow: visible;
	}
	text {
		fill: #555;
	}
	.year {
		font-size: 2rem;
		font-weight: 400;
	}
	.label {
		fill: #666;
	}
	line {
		stroke-width: 5px;
		stroke: #ccc;
		stroke-dasharray: 2 2;
	}
	circle {
		fill: orange;
	}
</style>
