<script>
	let innerDirection = null;
	let outerDirection = null;
	let selectedOuterConveyor = null; // 'left' or 'right'
	const rotation = 225;

	// Inner conveyor - Upper belt arrows
	const innerUpperBelt = [
		{ x: 37.4, y: 17.4 },
		{ x: 48.5, y: 25.7 },
		{ x: 57.2, y: 32.2 },
		{ x: 66.4, y: 38.2 },
		{ x: 75.3, y: 44.9 },
		{ x: 85.7, y: 52.1 },
	];

	// Inner conveyor - Lower belt arrows
	const innerLowerBelt = [
		{ x: 17.4, y: 33.3 },
		{ x: 27.5, y: 40.9 },
		{ x: 35.8, y: 47.2 },
		{ x: 43.8, y: 52.5 },
		{ x: 52.1, y: 58.1 },
		{ x: 59.0, y: 63.5 },
		{ x: 66.9, y: 68.6 },
	];

	// Outer conveyor - Left belt arrows
	const outerLeftBelt = [
		{ x: 59.2, y: 82.6 },
		{ x: 49.7, y: 74.5 },
		{ x: 44.9, y: 70.3 },
		{ x: 37.1, y: 63.2 },
		{ x: 30.8, y: 58.3 },
		{ x: 19.5, y: 48.8 },
	];

	// Outer conveyor - Right belt arrows
	const outerRightBelt = [
		{ x: 83.6, y: 60.8 },
		{ x: 76.7, y: 54.2 },
		{ x: 70.1, y: 47.6 },
		{ x: 63.4, y: 42.4 },
		{ x: 56.8, y: 36.3 },
		{ x: 50.2, y: 30.2 },
		{ x: 43.9, y: 24.8 },
	];

	function getDelay(index, beltLength, direction) {
		if (direction === 'outward') {
			return (beltLength - 1 - index) * 0.15;
		} else {
			return index * 0.15;
		}
	}

	function setInnerDirection(dir) {
		innerDirection = dir;
	}

	function setOuterDirection(dir) {
		outerDirection = dir;
	}

	function selectOuterConveyor(side) {
		selectedOuterConveyor = side;
	}

</script>

<div class="conveyor">
	<h1>Conveyor</h1>
	<p>This page is for the conveyor work you'll do next.</p>

	<div class="conveyors-wrapper">
		<!-- Inner Conveyor -->
		<div class="conveyor-section">
			<h2>Inner Conveyor</h2>

			<div class="conveyor-container">
				<img src="/inner_conv.png" alt="Inner Conveyor" />

				{#if innerDirection}
					{#each innerUpperBelt as arrow, i}
						<svg
							class="arrow"
							style="top: {arrow.y}%; left: {arrow.x}%; transform: rotate({innerDirection === 'outward' ? rotation + 180 : rotation}deg); animation-delay: {getDelay(i, innerUpperBelt.length, innerDirection === 'outward' ? 'inward' : 'outward')}s;"
							viewBox="0 0 24 24"
							width="40"
							height="40"
						>
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}

					{#each innerLowerBelt as arrow, i}
						<svg
							class="arrow"
							style="top: {arrow.y}%; left: {arrow.x}%; transform: rotate({innerDirection === 'outward' ? rotation + 180 : rotation}deg); animation-delay: {getDelay(i, innerLowerBelt.length, innerDirection === 'outward' ? 'inward' : 'outward')}s;"
							viewBox="0 0 24 24"
							width="40"
							height="40"
						>
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="red" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
				{/if}
			</div>

			<div class="direction-controls">
				<button class="dir-btn" class:active={innerDirection === 'inward'} on:click={() => setInnerDirection('inward')}>
					<div class="btn-icon">
						<svg viewBox="0 0 24 24" width="32" height="32">
							<path d="M19 12H5M12 19l-7-7 7-7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					</div>
					<span>Inward</span>
				</button>
				<button class="dir-btn" class:active={innerDirection === 'outward'} on:click={() => setInnerDirection('outward')}>
					<div class="btn-icon">
						<svg viewBox="0 0 24 24" width="32" height="32">
							<path d="M5 12h14M12 5l7 7-7 7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					</div>
					<span>Outward</span>
				</button>
			</div>
		</div>

		<!-- Outer Conveyor -->
		<div class="conveyor-section">
			<h2>Outer Conveyor</h2>

			<div class="conveyor-selector">
				<button class="selector-btn" class:active={selectedOuterConveyor === 'left'} on:click={() => selectOuterConveyor('left')}>
					L
				</button>
				<button class="selector-btn" class:active={selectedOuterConveyor === 'right'} on:click={() => selectOuterConveyor('right')}>
					R
				</button>
			</div>

			<div class="conveyor-container">
				<img src="/outconv.png" alt="Outer Conveyor" />

				{#if outerDirection}
					{#each outerLeftBelt as arrow, i}
						<svg
							class="arrow blue-arrow"
							style="top: {arrow.y}%; left: {arrow.x}%; transform: rotate({outerDirection === 'outward' ? rotation + 180 : rotation}deg); animation-delay: {getDelay(i, outerLeftBelt.length, outerDirection)}s;"
							viewBox="0 0 24 24"
							width="40"
							height="40"
						>
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="yellow" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}

					{#each outerRightBelt as arrow, i}
						<svg
							class="arrow blue-arrow"
							style="top: {arrow.y}%; left: {arrow.x}%; transform: rotate({outerDirection === 'outward' ? rotation + 180 : rotation}deg); animation-delay: {getDelay(i, outerRightBelt.length, outerDirection)}s;"
							viewBox="0 0 24 24"
							width="40"
							height="40"
						>
							<path d="M2 12 L18 12 L13 5 M18 12 L13 19" stroke="yellow" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					{/each}
				{/if}
			</div>

			<div class="direction-controls">
				<button class="dir-btn" class:active={outerDirection === 'inward'} on:click={() => setOuterDirection('inward')}>
					<div class="btn-icon">
						<svg viewBox="0 0 24 24" width="32" height="32">
							<path d="M19 12H5M12 19l-7-7 7-7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					</div>
					<span>Inward</span>
				</button>
				<button class="dir-btn" class:active={outerDirection === 'outward'} on:click={() => setOuterDirection('outward')}>
					<div class="btn-icon">
						<svg viewBox="0 0 24 24" width="32" height="32">
							<path d="M5 12h14M12 5l7 7-7 7" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
						</svg>
					</div>
					<span>Outward</span>
				</button>
			</div>
		</div>
	</div>
</div>

<style>
	.conveyor {
		padding: 24px;
	}

	.conveyors-wrapper {
		display: flex;
		flex-wrap: wrap;
		gap: 32px;
	}

	.conveyor-section {
		flex: 1;
		min-width: 300px;
	}

	.conveyor-section h2 {
		margin-bottom: 12px;
		font-size: 18px;
	}

	.direction-controls {
		display: flex;
		gap: 16px;
		justify-content: center;
		margin-top: 20px;
	}

	.dir-btn {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 8px;
		padding: 16px 24px;
		background: linear-gradient(145deg, #2a2a2a, #1a1a1a);
		border: 2px solid #3a3a3a;
		border-radius: 12px;
		color: #888;
		cursor: pointer;
		transition: all 0.2s ease;
	}

	.dir-btn:hover {
		background: linear-gradient(145deg, #333, #222);
		border-color: #555;
		color: #bbb;
		transform: translateY(-2px);
	}

	.dir-btn .btn-icon {
		width: 56px;
		height: 56px;
		display: flex;
		align-items: center;
		justify-content: center;
		background: rgba(255,255,255,0.05);
		border-radius: 50%;
		transition: all 0.2s ease;
	}

	.dir-btn:hover .btn-icon {
		background: rgba(255,255,255,0.1);
	}

	.dir-btn span {
		font-size: 13px;
		font-weight: 500;
		text-transform: uppercase;
		letter-spacing: 1px;
	}

	.dir-btn.active {
		background: linear-gradient(145deg, #1e40af, #1e3a8a);
		border-color: #3b82f6;
		color: #fff;
		box-shadow: 0 4px 20px rgba(59, 130, 246, 0.3);
	}

	.dir-btn.active .btn-icon {
		background: rgba(255,255,255,0.15);
	}

	.dir-btn.active:hover {
		background: linear-gradient(145deg, #2563eb, #1d4ed8);
		transform: translateY(-2px);
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

	.blue-arrow {
		animation: chase-blue 2s infinite;
	}

	@keyframes chase-blue {
		0%, 100% {
			opacity: 0.2;
			filter: drop-shadow(1px 1px 2px rgba(0,0,0,0.7));
		}
		20%, 30% {
			opacity: 1;
			filter: drop-shadow(0 0 8px rgba(255,255,0,0.9));
		}
	}

	.conveyor-selector {
		display: flex;
		gap: 12px;
		justify-content: center;
		margin-bottom: 16px;
	}

	.selector-btn {
		width: 50px;
		height: 50px;
		border-radius: 10px;
		border: 2px solid #3a3a3a;
		background: linear-gradient(145deg, #2a2a2a, #1a1a1a);
		color: #888;
		font-size: 20px;
		font-weight: 700;
		cursor: pointer;
		transition: all 0.2s ease;
	}

	.selector-btn:hover {
		background: linear-gradient(145deg, #333, #222);
		border-color: #555;
		color: #bbb;
		transform: translateY(-2px);
	}

	.selector-btn.active {
		background: linear-gradient(145deg, #1e40af, #1e3a8a);
		border-color: #3b82f6;
		color: #fff;
		box-shadow: 0 4px 15px rgba(59, 130, 246, 0.4);
	}

	.selector-btn.active:hover {
		background: linear-gradient(145deg, #2563eb, #1d4ed8);
		transform: translateY(-2px);
	}

</style>
