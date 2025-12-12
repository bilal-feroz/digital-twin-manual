<script>
	let direction = 'outward';
	const rotation = 225;

	// Upper belt arrows (flow right for outward, left for inward)
	const upperBelt = [
		{ x: 37.4, y: 17.4 },
		{ x: 48.5, y: 25.7 },
		{ x: 57.2, y: 32.2 },
		{ x: 66.4, y: 38.2 },
		{ x: 75.3, y: 44.9 },
		{ x: 85.7, y: 52.1 },
	];

	// Lower belt arrows (flow left for outward, right for inward)
	const lowerBelt = [
		{ x: 17.4, y: 33.3 },
		{ x: 27.5, y: 40.9 },
		{ x: 35.8, y: 47.2 },
		{ x: 43.8, y: 52.5 },
		{ x: 52.1, y: 58.1 },
		{ x: 59.0, y: 63.5 },
		{ x: 66.9, y: 68.6 },
	];

	function getDelay(index, beltLength) {
		if (direction === 'outward') {
			// Outward: reverse direction
			return (beltLength - 1 - index) * 0.15;
		} else {
			// Inward: normal direction (0,1,2...)
			return index * 0.15;
		}
	}

	function toggleDirection() {
		direction = direction === 'outward' ? 'inward' : 'outward';
	}
</script>

<div class="conveyor">
	<h1>Conveyor</h1>
	<p>This page is for the conveyor work you'll do next.</p>

	<div class="controls">
		<button on:click={toggleDirection}>
			Direction: {direction === 'outward' ? 'Outward' : 'Inward'}
		</button>
	</div>

	<div class="conveyor-container">
		<img src="/inner_conv.png" alt="Inner Conveyor" />

		<!-- Upper belt arrows -->
		{#each upperBelt as arrow, i}
			<svg
				class="arrow"
				style="top: {arrow.y}%; left: {arrow.x}%; transform: rotate({direction === 'outward' ? rotation : rotation + 180}deg); animation-delay: {getDelay(i, upperBelt.length)}s;"
				viewBox="0 0 24 24"
				width="40"
				height="40"
			>
				<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
			</svg>
		{/each}

		<!-- Lower belt arrows -->
		{#each lowerBelt as arrow, i}
			<svg
				class="arrow"
				style="top: {arrow.y}%; left: {arrow.x}%; transform: rotate({direction === 'outward' ? rotation : rotation + 180}deg); animation-delay: {getDelay(i, lowerBelt.length)}s;"
				viewBox="0 0 24 24"
				width="40"
				height="40"
			>
				<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
			</svg>
		{/each}
	</div>
</div>

<style>
	.conveyor {
		padding: 24px;
	}

	.controls {
		margin-bottom: 16px;
	}

	.controls button {
		padding: 8px 16px;
		font-size: 14px;
		cursor: pointer;
		background: #333;
		color: white;
		border: none;
		border-radius: 4px;
	}

	.controls button:hover {
		background: #555;
	}

	.conveyor-container {
		position: relative;
		display: inline-block;
	}

	.conveyor-container img {
		display: block;
		max-width: 100%;
	}

	.arrow {
		position: absolute;
		filter: drop-shadow(1px 1px 2px rgba(0,0,0,0.7));
		animation: chase 2s infinite;
		translate: -50% -50%;
	}

	@keyframes chase {
		0%, 100% {
			opacity: 0.2;
			filter: drop-shadow(1px 1px 2px rgba(0,0,0,0.7));
		}
		20%, 30% {
			opacity: 1;
			filter: drop-shadow(0 0 8px rgba(255,0,0,0.9));
		}
	}
</style>
